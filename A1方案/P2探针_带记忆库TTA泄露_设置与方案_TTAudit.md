# P2 探针：带逐样本记忆库回放的 TTA（BufTTA）会不会泄露（设置与方案）

> 性质：受控对比探针。BufTTA 与 Pilot 的 Tent 之间只差一个模块——“高置信样本记忆库 + 回放”，用来回答：泄露源到底是“逐样本存储/复用”，还是“适应本身”。
> 前置：请先读 `Pilot实验设置与方案_TTAudit.md`。共用环境、切分、攻击脚本。
> 状态：v1（待跑）。

## 0. 为什么跑 P2

- Pilot 显示纯 BN+affine 的 Tent（不逐样本存东西）在最终模型上几乎不泄露（≈0.51）。
- 我们此前的机制判断是：真正的泄露载体可能是“逐样本记忆”（很多 TTA 方法会存高置信样本/伪标签用于稳定或回放），而不是 BN 统计这种聚合量。
- P2 造一个只比 Tent 多“记忆库+回放”的最小系统 BufTTA。若 BufTTA 泄露而 Tent 不泄露，就把泄露源定位到“逐样本存储/复用”，论文可以重定位为“TTA 泄露的机制审计：聚合适应安全、逐样本机制危险”。

## 1. BufTTA 是什么（与 Tent 的差异只有加粗那块）

BufTTA = Tent 的全部原设置 ＋ 记忆库回放：

| 组件 | 设置 |
|---|---|
| 基础适应 | 与 Pilot Tent 完全相同（BN+affine、熵最小化、batch 64、单遍、lr 1e-3） |
| 记忆库（buffer） | batch 内 softmax 置信度 > tau 的样本，连同其伪标签存入 buffer；buffer 上限 M，满则先进先出 |
| 回放 | 每 16 个流 batch 后做 1 次回放：从 buffer 抽 64 个，用 CE(伪标签) 更新 BN+affine（同一优化器） |
| tau / M | 默认 0.7 / 500（先跑通这一个配置，再决定要不要扫） |
| 适应结束后 | **丢弃 buffer，只保存网络权重**（攻击者拿到的只是权重，拿不到 buffer） |
| 攻击 / 评测 | 与 Pilot 完全相同（ent / conf / ce AUC + eval Top-1） |

> 关键设计：buffer 只在适应期间存在、不进共享产物。这样 BufTTA 与 Tent 的对比是公平的——如果 BufTTA 泄露，说明泄露是通过回放被“焊进权重”的功能性信号，而不是“攻击者直接读到了存下来的图”。

## 2. 判读（诊断表）

| 观察 | 含义与决定 |
|---|---|
| BufTTA 的 ent 或 conf AUC ≥0.60，且 eval Top-1 没明显低于 Tent（不低于约 35） | 结论 C：逐样本记忆/回放是泄露源，适应本身不是 → “TTA 泄露机制审计”方向成立，下一步扫 buffer 大小 / tau / 各类 n_c |
| BufTTA AUC <0.53 | 单机“最终模型”通道整体无货 → 停 A1 线，转 A2 或换题 |
| BufTTA 的 eval Top-1 崩（<20） | 回放过拟合导致坍缩，AUC 高不可信；先降回放频率或调 tau / M |

> 可选加分：若服务器上有现成开源 CoTTA / EATA，在同一流上再跑一遍作为“真实方法”的外部有效性（它们不是受控对比，结果只作参考）。
## 3. 数据与切分（与 Pilot 完全相同，seed 0）

```python
import numpy as np

idx = np.random.RandomState(0).permutation(10000)
member_idx    = idx[0:2000]     # 私有适应流（成员）
nonmember_idx = idx[2000:4000]  # 非成员
eval_idx      = idx[4000:5000]  # 评测流（测 Top-1）
```

直接复用 Pilot 生成的 split_indices.npz 与候选打分脚本。

## 4. BufTTA 实现要点（伪代码，只加记忆库与回放）

```python
model = load_source_checkpoint()
optimizer = SGD(bn_affine_params, lr=1e-3, momentum=0.9)
buffer = []                       # 元素：(x_tensor, pseudo_label)，上限 M=500
M, tau, replay_every = 500, 0.7, 16

for step, (x) in enumerate(stream_batches(member_idx, batch=64)):
    model.train()
    logits = model(x)
    loss = entropy(softmax(logits)); loss.backward()
    optimizer.step(); optimizer.zero_grad()

    # --- 记忆库写入（仅比 Tent 多这部分） ---
    p = softmax(model(x), dim=1).detach()          # 适应后重算一次置信度
    conf, yhat = p.max(dim=1)
    keep = conf > tau
    for xi, yi in zip(x[keep], yhat[keep]):
        buffer.append((xi.clone(), yi.item()))
    if len(buffer) > M:
        buffer = buffer[-M:]                       # FIFO

    # --- 回放（每 replay_every 个流 batch 做一次） ---
    if (step + 1) % replay_every == 0 and len(buffer) >= 64:
        sample = random_choice(buffer, 64, seed=step)
        xb = torch.stack([s[0] for s in sample])
        yb = torch.tensor([s[1] for s in sample])
        logits_b = model(xb)
        replay_loss = cross_entropy(logits_b, yb)
        replay_loss.backward(); optimizer.step(); optimizer.zero_grad()

# 结束：丢弃 buffer，只保存网络权重
torch.save(model.state_dict(), out/adapted_buftta_seed0.pth)
```

要点：
- 攻击者拿到的只有 model.state_dict()（网络权重），buffer 不进共享产物；
- 记录 buffer 实际使用情况：最终存了多少条、每条伪标签对应的类计数 n_c（为后续按 n_c 分桶预留）；
- 攻击与评测前切回 model.eval()。
## 5. 攻击与指标（与 Pilot 完全一致）

| 打分 | 公式 | 方向 |
|---|---|---|
| score_ent（主） | -H(p)，预测熵取负 | 成员预期更高 |
| score_conf（主） | max softmax 概率 | 成员预期更高 |
| score_ce（诊断） | -CE(y_true)，真实标签交叉熵取负 | 攻击者无标签，仅作确认 |

AUC = roc_auc_score(y, score)，y=1 成员、0 非成员，0.5=瞎猜。打分脚本沿用 Pilot 文档 8.3 的 collect_scores。

## 6. 输出与汇报表（跑完回填）

| 系统 | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 | buffer 实际规模 |
|---|---|---|---|---|---|
| Source | 0.5023 | 0.5029 | 0.4976 | 11.7 | - |
| Tent（Pilot） | 0.5096 | 0.5126 | 0.4997 | 41.2 | - |
| BufTTA（tau=0.7, M=500） | ? | ? | ? | ? | ? |
| （可选）CoTTA / EATA | ? | ? | ? | ? | ? |

结果建议存到 outputs/p2_seed0/，json 里附带 buffer 规模与各类 n_c 分布。

## 7. 跑之前 checklist

- [ ] 直接复用 Pilot 的 split_indices.npz 与候选打分脚本，不重新切分
- [ ] BufTTA 与 Tent 的差异只有记忆库+回放（先跑通 tau=0.7 / M=500 / 每 16 batch 回放一次这一配置）
- [ ] 适应结束后丢弃 buffer，只保存网络权重
- [ ] 记录 eval Top-1（防回放坍缩假象）
- [ ] 记录 buffer 实际规模（若为 0，说明 tau 太高，先调低再读数）
- [ ] 先跑 seed 0，时间允许补 seed 1、2

## 附注：P2 结果怎么用

- 结论 C 成立（BufTTA ≥0.60、Tent ≈0.51）：论文重定位为“TTA 泄露机制审计”——聚合统计适应不泄露，逐样本记忆机制泄露；下一步主实验 = 扫 buffer 大小 / tau / 伪标签质量 / 各类 n_c，并给出“既能稳定适应又不留个体痕迹”的设计准则。
- 结论 C 不成立（BufTTA <0.53）：单机“最终模型”泄露通道整体无货，A1 线终止；回到 A2（观测适应过程中间消息的多端设定）或另选题。
