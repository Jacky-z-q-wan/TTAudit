# P4 探针：稀有类 / 匿名集“死心实验”（设置与方案）

> 性质：A1 族的最后一个门槛实验。前情：泄露只在“反复曝光 + 学 affine”时出现（10遍 AUC≈0.59），纯 BN 统计安全、记忆库回放安全、裁剪能压住但不免费。P4 要回答最后、也是最有新意潜力的一个问题：**反复曝光下，泄露是不是集中在“最没同类掩护的稀有个体”身上（n_c 越小越危险）？**
> 若成立 → 方向多一个不显然的发现，可以“小论文”预期做下去；若不成立 → A1 整族关闭，不留遗憾地转 A2 或重新选题。
> 前置：先读 Pilot / P1 / P3 文档。全部复用同一源模型、同一种 corruption、同一攻击打分逻辑。
> 状态：v1（待跑）。预计半天（3 个配置 × 3 seeds）。

## 0. 一句话

> 把成员流改成不均衡（部分类只有 1~5 张成员），跑 10 遍 affine Tent，按每个类实际成员数 n_c 分桶算成员推断 AUC。如果 n_c=1~3 的桶明显高于头部桶 → “稀有个体先被记住”，方向救活；如果各桶一个样 → 死心。

## 1. 成员流设计（不均衡，总数 ≈1960）

设 labels 来自 CIFAR-100-C 的 labels.npy（按真实类标签取索引，不依赖文件顺序）：

| 组 | 类数 | 每类成员数 n_c | 合计 |
|---|---|---|---|
| 尾部（tail） | 30 | 2 | 60 |
| 中部（mid） | 30 | 10 | 300 |
| 头部（head） | 40 | 40 | 1600 |
| 合计 | 100 | - | 1960 |

- 类分组随机选定，seed 与运行 seed 一致（seed 0/1/2 各一次）；
- 每个类的非成员候选：该类剩余图里固定取 60 张（保证与成员不重叠、同域同 severity）；
- 评测流（Top-1）继续复用 pilot 的 eval_idx（1000 张），只测效用。
## 2. 切分代码（示意）

```python
import numpy as np

labels = np.load(path_to_labels_npy)          # 10000 个真实标签
rng = np.random.RandomState(0)
cls_order = rng.permutation(100)
tail, mid, head = cls_order[:30], cls_order[30:60], cls_order[60:]
plan = {}
for c in tail: plan[c] = 2
for c in mid:  plan[c] = 10
for c in head: plan[c] = 40

member_idx, nonmember_idx = [], []
for c, k in plan.items():
    pool = np.where(labels == c)[0]           # 该类全部索引
    rng.shuffle(pool)
    member_idx += list(pool[:k])              # 前 k 张为成员
    nonmember_idx += list(pool[k:k+60])       # 之后 60 张为非成员候选

# member_idx / nonmember_idx 各自按类打上 group 标记：
# tail 组 -> bucket 1-3；mid 组 -> bucket 10；head 组 -> bucket 40
```

## 3. 要跑的配置（攻击打分与 Pilot/P1 完全一致）

| # | 配置 | 作用 |
|---|---|---|
| A | Tent 10遍 BN+affine，不均衡成员流 | 主行：看 n_c 分桶 AUC 是否有差异 |
| B | Tent 1遍 BN+affine，同一不均衡成员流 | 对照：确认效应需要反复曝光 |
| C | BN-only 10遍，同一不均衡成员流 | 对照：确认效应需要学 affine |
| D | （可选）Tent 10遍 + 裁剪 C=1 | 若 A 有信号，看裁剪是否连稀有个体一起护住 |

## 4. 分桶指标（主指标 = 桶内合并 AUC）

对每个桶（tail=1~3、mid=10、head=40）：
- positives = 该桶所有类的成员；
- negatives = 与 positives 等量的该桶非成员（按类分层随机抽取，seed 固定）；
- 桶 AUC = roc_auc_score(合并后的 y, 合并后的 score)，用 score_ent（主）+ score_conf（辅）。

> 用“桶内合并 AUC”而不是“逐类 AUC 平均”，因为 n_c=2 的类只有 2 个正样本，单类 AUC 噪声太大；合并后每桶有 60~1600 个正样本，稳定得多。
## 5. 判据（示意阈值，方向明确即可）

| 结果 | 判断 |
|---|---|
| 主行 A：tail 桶 ≥0.65 且 tail − head ≥0.10 | 方向救活：“稀有个体被记住得最快”，A1 族以“反复曝光机制审计”小论文继续 |
| 主行 A：各桶都在 0.5~0.60 且相互差 <0.05 | 死心：泄露不随 n_c 集中 → A1 族关闭，转 A2 或重新选题 |
| 对照 B / C 各桶 ≈0.5 | 佐证：效应需要“反复曝光 + 学 affine”同时成立 |
| （可选 D）裁剪后 tail 桶明显回落 | 加分：便宜防护连最危险的稀有个体也能护住 |

## 6. 输出与汇报表（跑完回填，3 seeds 平均）

| 配置 | tail(1~3) AUC | mid(10) AUC | head(40) AUC | tail−head | Top-1 |
|---|---|---|---|---|---|
| A: Tent 10遍 | ? | ? | ? | ? | ? |
| B: Tent 1遍 | ? | ? | ? | ? | ? |
| C: BN-only 10遍 | ? | ? | ? | ? | ? |
| D: Tent 10遍 + 裁剪（可选） | ? | ? | ? | ? | ? |

结果存到 outputs/p4_seed{seed}/，每个配置一个 json（含各桶 AUC 与每类成员数）。

## 7. 跑之前 checklist

- [ ] 用 labels.npy 按真实类标签取索引，不依赖文件块顺序
- [ ] 成员与非成员同 corruption、同 severity、互不重叠
- [ ] tail 类每类 2 个成员：先肉眼确认攻击脚本能正确处理（正样本少）
- [ ] 每个配置记录 eval Top-1（防坍缩假象）
- [ ] 先跑 seed 0 看趋势，再补 seed 1、2
- [ ] 时间不够时只跑 A 行 + 判据，B/C 是佐证非必需

## 附注

- 若 A 行出现“head 明显高于 tail”的倒挂：不属于预期故事，按“无信号”处理（不硬凑）；
- 若 tail 桶 AUC 高但 Top-1 崩：先怀疑坍缩，重设后再读数；
- 本探针是 A1 族的终点：无论结果如何，不再加新的“再试一次”实验，直接据此做选题决定。
