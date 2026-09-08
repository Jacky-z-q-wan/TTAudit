# P1 探针：多遍适应会不会让 Tent 开始泄露（设置与方案）

> 性质：诊断性探针，不是新方法。回答一个问题：Pilot 里 Tent 单遍 AUC≈0.51（不泄露），是不是仅仅因为“每张只过一次、还没记住个体”？如果让模型反复看同一批样本（过拟合到个体），泄露会不会出现、从第几遍开始出现。
> 前置：请先读 `Pilot实验设置与方案_TTAudit.md`。本探针与 Pilot 共用环境、数据切分、攻击脚本，唯一差异是适应遍数。
> 状态：v1（待跑）。

## 0. 为什么跑 P1

- Pilot 结论：Source≈0.50、Tent(1遍)≈0.51，Tent 没有把个体焊进最终模型。
- 一个合理的怀疑是：单遍流太短（2000 张 = 32 个 batch），模型还没来得及“记住”个体。
- P1 把同一批样本重复喂给模型 2/5/10 遍，人为制造对个体的过拟合，看泄露信号是否随遍数出现。
- 用途：判断“泄露是否必须依赖过拟合个体”。这决定论文还能不能往“单机 TTA 泄露”方向写，还是该转向 A2 / 换题。

## 1. 总览（与 Pilot 的差异只有“适应遍数”）

| 项 | 值 |
|---|---|
| 数据集 / corruption | CIFAR-100-C / gaussian_noise，severity 5 |
| 模型 | 同一个 CIFAR-100 ResNet-18 源模型 |
| 适应方法 | Tent（BN+affine、熵最小化），与 Pilot 相同 |
| 适应遍数 | 1 / 2 / 5 / 10（1 遍 = 复现 Pilot，作 sanity） |
| 每遍样本顺序 | 每个 epoch 内重洗牌，种子固定（seed=0） |
| 优化器 / batch / lr | SGD momentum 0.9 / 64 / 1e-3（与 Pilot 相同） |
| 每遍结束后 | 重置回源模型再开始下一遍（各遍相互独立） |
| 攻击 | 与 Pilot 相同的成员推断打分（ent / conf / ce） |
| 汇报 | 每个遍数的 AUC×3 + eval Top-1 |

## 2. 判读（这是诊断表，不是 pass/fail）

| 观察 | 含义与决定 |
|---|---|
| 1 遍复现 Pilot（ent≈0.5096、conf≈0.5126） | sanity：环境与脚本没被改坏 |
| 所有遍数 AUC 都 ≤0.53 | 结论 A：即使过拟合到个体，最终模型快照也测不出可提取信号 → 单机“最终模型”通道无货，停 A1，转 A2 或换题 |
| AUC 随遍数明显上升（10 遍 ≥0.60） | 结论 B：泄露来自对个体的反复拟合，正常单遍 TTA 不泄露 → 论文可重定位为“泄露何时出现”的机制审计；但先跑 P2 再拍板 |
| 遍数上去后 Top-1 暴跌（如 <20） | 模型坍缩/退化，此时 AUC 高要当作假象处理，先修设置再读数 |

> 说明：多遍适应不是真实部署形态，P1 只用来做机制判断（泄露是否必须靠过拟合），不代表我们要做“多遍 TTA 方法”。
## 3. 数据与切分（与 Pilot 完全相同，seed 0）

```python
import numpy as np

idx = np.random.RandomState(0).permutation(10000)
member_idx    = idx[0:2000]     # 私有适应流（成员）
nonmember_idx = idx[2000:4000]  # 非成员
eval_idx      = idx[4000:5000]  # 评测流（测 Top-1）
```

直接复用 Pilot 生成的 split_indices.npz 与候选打分脚本即可，不必重新切分。

## 4. 执行步骤（伪代码）

对每个目标遍数 ep in [1, 2, 5, 10]：

```python
model = load_source_checkpoint()          # 每遍从同一源模型开始
optimizer = SGD(bn_affine_params, lr=1e-3, momentum=0.9)

for epoch in range(ep):                   # 重复 ep 遍
    order = shuffle(member_idx, seed=epoch)  # 每 epoch 重洗，种子固定
    for each batch(64) in order:
        model.train()                     # BN 用 batch 统计并更新 running 统计
        loss = entropy(softmax(model(x)))
        loss.backward(); optimizer.step(); optimizer.zero_grad()

torch.save(model.state_dict(), out/adapted_tent_ep{ep}_seed0.pth)   # 只存网络权重

# 攻击：用与 Pilot 完全相同的 candidate loader 与 collect_scores
# 指标：score_ent / score_conf / score_ce 的 AUC，以及 eval_idx 上的 Top-1
```

要点：
- 每个 ep 独立从源模型开始（episodic），不要在同一模型上连续累加 ep；
- 1 遍那行应能复现 Pilot 的 Tent 数字（0.5096 / 0.5126 / 0.4997 / Top-1 41.2）；
- 攻击与评测前切回 model.eval()。
## 5. 攻击与指标（与 Pilot 完全一致）

| 打分 | 公式 | 方向 |
|---|---|---|
| score_ent（主） | -H(p)，预测熵取负 | 成员预期更高 |
| score_conf（主） | max softmax 概率 | 成员预期更高 |
| score_ce（诊断） | -CE(y_true)，真实标签交叉熵取负 | 攻击者无标签，仅作确认 |

AUC = roc_auc_score(y, score)，y=1 成员、0 非成员，0.5=瞎猜。打分脚本沿用 Pilot 文档 8.3 的 collect_scores。

## 6. 输出与汇报表（跑完回填）

| 系统 | 遍数 | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---|---|---|---|---|
| Source | 0 | 0.5023 | 0.5029 | 0.4976 | 11.7 |
| Tent | 1 | 0.5096 | 0.5126 | 0.4997 | 41.2 |
| Tent | 2 | ? | ? | ? | ? |
| Tent | 5 | ? | ? | ? | ? |
| Tent | 10 | ? | ? | ? | ? |

结果建议存到 outputs/p1_seed0/，每个 ep 一行 json。

## 7. 跑之前 checklist

- [ ] 直接复用 Pilot 的 split_indices.npz 与候选打分脚本，不重新切分
- [ ] 1 遍那行复现 Pilot 数字（sanity）
- [ ] 每遍从同一源模型开始，各遍独立
- [ ] 记录每个 ep 的 eval Top-1（防坍缩假象）
- [ ] 先跑 seed 0，时间允许补 seed 1、2

## 附注：P1 结果怎么用

- 若出现结论 A（全部 ≈0.5）：不要马上放弃整个隐私方向，先跑 P2——泄露可能不在“适应次数”，而在“是否逐样本存储”；
- 若出现结论 B（随遍数上升）：说明“正常单遍 TTA 不泄露、泄露要过拟合个体”，这本身是一个可写进论文的机制结论，但论文主线要重新定位。
