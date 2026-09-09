# TTAudit · Pilot（试跑）实验设置与方案

> 目的：用最小成本验证一个前提——**在线 TTA 适应过的模型，是否会让“某张图在不在适应流里”被攻击者猜中**。
> 状态：v1（待跑）。日期：2026-09-07。负责人：（待填）。本文件是上服务器执行的依据，跑完后回填第 9 节结果表。

## 0. Pilot 回答什么（为什么只跑两个系统）

Pilot 只回答一个问题：**泄露信号存不存在、我们的尺子准不准。**

- `Source`（不做任何适应）= 负对照：攻击 AUC 应 ≈0.5；若明显偏离，说明是数据切分/脚本 bug，不是适应在泄露。
- `Tent`（最简单、最主流、最不容易漏的适应方式）= 试金石：如果连 Tent 都明显高于 0.5，前提成立，方向可继续。
- 其他 TTA 方法（EATA / SAR / 带记忆库等）**不在此 Pilot 内**，等前提通过后进主实验系统比较。

## 1. 总览（所有设置写死，中途不改）

| 项 | 值 |
|---|---|
| 数据集 | CIFAR-100-C |
| Corruption | 只跑 `gaussian_noise`（Gauss） |
| Severity | 5 |
| 模型 | ResNet-18（源模型 = CIFAR-100 干净训练集训好的权重） |
| 适应方法 | Source（不做）、Tent（BN+affine，熵最小化） |
| 适应 batch / 轮数 | 64 / 对私有流单遍（每张只过一次） |
| 私有流（成员集） | 2000 张 |
| 非成员集 | 2000 张（同 corruption、同 severity） |
| 公开评测流 | 1000 张（与成员/非成员均不重叠） |
| 攻击 | 成员推断 MIA（熵/置信度打分，现成方法） |
| 指标 | 攻击 AUC（0.5=瞎猜，1=全中）；辅助报 Top-1 |
| Seeds | 0（先跑），时间允许再跑 1、2 |

## 2. go / no-go 判断（先定死）

用 3 个 seed 平均 AUC 判断（只跑 seed 0 时先看趋势，不下最终结论）：

| 情形 | 判断 |
|---|---|
| Source ≈0.5（0.45~0.55）且 Tent ≥0.60 | **过**：前提成立，进主实验 |
| Source ≈0.5 且 0.55 ≤ Tent < 0.60 | **弱信号**：加跑一个带记忆库方法再判 |
| Source ≈0.5 且 Tent < 0.53 | **不过**：前提不成立，止损换方向 |
| Source 明显偏离 0.5（>0.60 或 <0.40） | **尺子坏了**：切分/脚本有 bug，先修再谈 |

> 数值为示意阈值；若首跑方向明确（如 Source 0.50 / Tent 0.75），不必纠结精确边界。
## 3. 环境与依赖

- Python 3.9+；PyTorch ≥2.0；torchvision；numpy；scikit-learn；tqdm。
- 单张 GPU 即可（Pilot 很轻），无 GPU 用 CPU 也能跑。
- 建议独立 conda 环境：

```bash
conda create -n ttaudit python=3.10 -y
conda activate ttaudit
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
pip install numpy scikit-learn tqdm
```

## 4. 服务器上先确认的资产（没齐先补，别直接开跑）

1. 源模型权重：一个在 CIFAR-100 干净训练集上训好的 ResNet-18 checkpoint。
   - 优先复用 C-cubed 项目已有 checkpoint，记录其路径与训练配置。
   - 注意：不能直接用 ImageNet 预训练的 torchvision 权重（输入 32x32、100 类都对不上）。
   - 若无，按文末“附注 A”训练一个。
2. 数据：CIFAR-100-C 目录下应有 `gaussian_noise.npy`（10000x32x32x3，uint8）与 `labels.npy`（10000 个标签）。能复用 C-cubed loader 读取逻辑即可。
3. Tent 实现：C-cubed/CEMA 代码里有就复用；没有按文末“附注 B”的最小实现写。
## 5. 数据切分（写死，seed 固定，三组互不重叠）

设 data 为 gaussian_noise severity 5 的全部 10000 张（顺序即文件顺序），label 为对应标签：

```python
import numpy as np

idx = np.random.RandomState(0).permutation(10000)
member_idx    = idx[0:2000]     # 私有适应流 A：Tent 在上面适应；攻击者想猜的成员
nonmember_idx = idx[2000:4000]  # 非成员 B：同 corruption、同 severity，只是没被适应
eval_idx      = idx[4000:5000]  # 公开评测流 C：测效用；不参与适应，也不进攻击候选
```

三条铁律：
- 成员、非成员、评测流**两两不重叠**；
- 非成员必须与成员来自**同一 corruption、同一 severity**（绝不拿干净 CIFAR-100 当非成员，否则图本身可分，AUC 虚高）；
- 适应时按 member_idx 排列顺序作为时间流，每张只过一次。
## 6. 两个系统的方法设置（写死）

两个系统吃同一份 member_idx 流，唯一差别是有没有做适应。

### 6.1 Source（不做适应）
- 什么都不改。攻击与评测时模型处于 `model.eval()`。

### 6.2 Tent（熵最小化，动 BN + affine）
- 可学习参数：所有 BN 层的 scale（gamma）与 shift（beta），其余参数 `requires_grad=False`。
- 每个 batch 在 `model.train()` 模式下前向（BN 用 batch 统计并更新 running 统计），优化器只更新 gamma/beta。
- 损失：预测 softmax 的熵 `H = -sum p log p`，取平均后反传。
- 优化器：SGD，lr=1e-3，momentum=0.9。
- batch size=64，按流顺序切，单遍；corruption 之间 episodic（Pilot 只有 1 个 corruption，天然满足）。
- 结束后保存：`torch.save(model.state_dict(), 'results/adapted_tent_seed0.pt')`。
- 攻击与评测前先 `model.eval()`，用的是适应后（running 统计已更新）的模型。

固定项（记入结果）：lr、batch、优化器、单遍、无数据增强、随机种子与 seed 一致。
## 7. 效用评测（次要，但顺手记下）

在 eval_idx 这 1000 张上分别算 Source 与 Tent 的 Top-1：

| 系统 | Top-1（eval 1000） |
|---|---|
| Source | ? |
| Tent | ? |

作用：确认 Tent 真的在适应（若 Tent 精度 ≤ Source，先修实现/超参，再谈泄露）。

## 8. 攻击：成员推断（现成方法，最小实现）

### 8.1 攻击者设定
- 攻击者拿到适应后的模型（白盒，Tent 场景；Pilot 不做黑盒）。
- 候选集 = 2000 成员 + 2000 非成员，攻击者不知道谁是谁，目标是猜出成员。
- 打分用无监督信号（适应无监督、攻击者也没标签）：被记住的成员预期熵更低/置信度更高。

### 8.2 三种打分

| 打分 | 公式 | 方向 |
|---|---|---|
| score_ent（主） | -H(p)，预测熵取负 | 成员预期更高 |
| score_conf（主） | max softmax 概率 | 成员预期更高 |
| score_ce（诊断） | -CE(y_true)，真实标签交叉熵取负 | 成员预期更高；攻击者无标签，仅用于确认信号 |

AUC：sklearn.metrics.roc_auc_score(y, score)，y=1 为成员、0 为非成员；分数越高越像成员；0.5=瞎猜。
### 8.3 参考脚本骨架（按你的数据加载改路径即可）

```python
import numpy as np, torch
import torch.nn.functional as F
from sklearn.metrics import roc_auc_score

def collect_scores(model, loader, device):
    rows = []
    model.eval()
    with torch.no_grad():
        for x, y, is_member in loader:      # loader 依次喂成员、非成员
            x, y = x.to(device), y.to(device)
            logits = model(x)
            p = F.softmax(logits, dim=1)
            ent = -(p * torch.log(p + 1e-12)).sum(dim=1)
            conf = p.max(dim=1).values
            ce = F.cross_entropy(logits, y, reduction='none')
            rows.append(torch.stack([-ent, conf, -ce,
                torch.tensor(is_member, dtype=torch.float32).to(device)], dim=1))
    rows = torch.cat(rows).cpu().numpy()
    return rows

# scores = collect_scores(model, candidate_loader, device)
# y = scores[:, 3]
# for name, col in [('ent', 0), ('conf', 1), ('ce', 2)]:
#     print(name, roc_auc_score(y, scores[:, col]))
```

关键点：
- 成员/非成员 loader 都 shuffle=False、无增强；
- 两 loader 分开构造，打分后拼在一起即可；
- 同一打分脚本分别喂 Source 与 Tent，得到两行 AUC。
## 9. 输出与汇报表（跑完回填）

结果建议存到 `TTAudit/results/pilot_seed{seed}.json`（含 AUC、acc、超参与路径）。汇报表：

| 系统 | score_ent AUC | score_conf AUC | score_ce AUC（诊断） | Top-1 |
|---|---|---|---|---|
| Source | ≈0.5 | ≈0.5 | ≈0.5 | ? |
| Tent | ? | ? | ? | ? |

（可选加分项）带记忆库的方法（如 EATA 式存高置信特征）：预期 AUC 最高，用于确认“不同方法泄露程度不同”。

## 10. 跑之前 checklist

- [ ] 源模型是 CIFAR-100 训的 ResNet-18（不是 ImageNet 权重），路径已确认
- [ ] gaussian_noise.npy、labels.npy 路径正确
- [ ] 切分 seed=0，成员/非成员/评测流互不重叠
- [ ] Tent 在 eval 流上 Top-1 ≥ Source（先证明它在适应）
- [ ] 成员与非成员同 corruption、同 severity
- [ ] 打分脚本在 Source 上 ≈0.5（尺子校准通过）
- [ ] seed 0 先跑，能出数再补 seed 1、2

## 附注 A：如果没有 CIFAR-100 预训练 ResNet-18

在 CIFAR-100 干净训练集上从零训一个 ResNet-18（与主流 TTA 论文一致即可，不必调优）：
- 优化器 SGD，lr=0.1（cosine 衰减），momentum=0.9，weight_decay=5e-4；
- batch size 128，训练 100 epoch（约 1 张中端 GPU 数小时）；
- 增强：随机裁剪 + 水平翻转；
- 训完存为源模型 checkpoint，作为 Source 与 Tent 的共同起点。

## 附注 B：如果没有 Tent 实现

最小 Tent 与第 6.2 节一致，可参考 Tent 官方开源实现，注意三点：
1. 只对 BN 的 gamma/beta 开 requires_grad；
2. 前向必须走 model.train()（BN 用 batch 统计并更新 running 统计），不能用 eval；
3. 适应结束后再切回 model.eval() 做评测与攻击。
