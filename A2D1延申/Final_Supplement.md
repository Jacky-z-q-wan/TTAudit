# 最终方案补充实验

## ——协作路径校验、数据划分修复与分布指纹泄露复验

**版本：**v1.0  
**日期：**2026-09-08  
**对应主文档：**`最终版项目方案.md`  
**对应已有结果：**`outputs/a2_pilot_full_20260908/`

---

## 0. 实验目的与当前结论

本补充实验不是为了继续扩大原 Pilot 的场景数量，而是为了判断已有结果究竟代表：

1. 协作 TTA 的真实隐私现象；
2. NoRaw 协议没有真正使用接收消息；
3. 客户端划分或攻击实现造成的统计假象。

### 0.1 已有结果的客观摘要

当前 `a2_pilot_full_20260908` 共包含 63 个场景，主要观察如下：

- 58/63 个场景中，Local-only 与 NoRaw 的 Top-1 完全相同；
- K=4 IID 场景中，client 0/1/2 各只有 250 个 eval 样本，而 client 3 有 9250 个；
- K=8 IID 场景中，client 0–6 各有 125 个 eval 样本，而 client 7 有 9125 个；
- prototype 的 (n_c=1,2,4,8,16,32) 实验没有呈现稳定的成员 AUC 单调趋势；
- IID、label-skew、tail 场景的 Local-only 与 NoRaw 效用约为 1%，而 Raw oracle 约为 12%–15%；
- label-skew 且重复发布 5/10 轮时，source-ID 和 class-presence 识别率明显高于随机基线；
- 消息对象中同时记录了 `client_id`、`domain_label`、`class_presence`、`class_counts`、`sample_count`、`n_c` 等字段，但尚未明确哪些字段是真正传输的，哪些只是诊断字段。

### 0.2 当前实验判定

原始 D1 的“样本级成员泄露随 (n_c) 减小而增强，并且 NoRaw 明显提升效用”尚未得到支持。

但是，当前结果提出了一个值得复验的新问题：

> 在非 IID、多轮协作 TTA 中，消息是否会泄露客户端的类别结构、域属性和分布指纹？

在本补充实验完成前，不得开始 DP、随机投影、预算分配或复杂安全算法实验。

---

## 1. 总体实验顺序

补充实验必须按下面顺序执行，不能跳步：

```text
A. 协议路径单元测试
        ↓
B. 数据划分与 TTA 效用基线修复
        ↓
C. transcript 可见性和攻击实现校验
        ↓
D. 最小分布指纹泄露复验
        ↓
E. Go / Conditional Go / No-Go 决策
```

如果 A、B 或 C 中任何一项没有通过，D 阶段的隐私数字都不能写成论文结果。

---

## 2. 实验 A：NoRaw 协作路径单元测试

### 2.1 目标

确认 NoRaw 是否真的执行了以下完整流程：

```text
客户端读取上一轮收到的消息
    → 使用本地无标注流进行适应
    → 生成本轮 outgoing message
    → 服务器聚合
    → 广播聚合结果
    → 下一轮客户端使用聚合结果
```

特别注意：`rounds=1` 时，消息只能在评估之后生成，不能用于同一轮的预测。因此正式测试必须在至少 2 轮时比较“收到消息后”的状态变化。

### 2.2 必须实现的四条对照线

|协议|发送消息|接收消息|是否使用接收消息|用途|
|---|---:|---:|---:|---|
|Local-only|否|否|否|本地适应基线|
|NoRaw-silent|是|是|否|排除“仅发送消息”效应|
|NoRaw|是|是|是|真正的协作协议|
|Raw oracle|原始数据|集中式|是|效用上界|

`NoRaw-silent` 很重要：如果 NoRaw-silent 和 NoRaw 完全一样，说明消息没有被真正消费。

### 2.3 正向控制实验

构造一个极小的人工任务，不依赖 CIFAR-100-C：

- client 0 只含类别 A；
- client 1 只含类别 B；
- 两端分别生成明显不同的原型消息；
- 服务器计算平均原型并广播；
- 下一轮客户端使用接收的平均原型进行预测或适应。

必须观察到：

- client 0 和 client 1 收到相同的服务器聚合消息；
- 接收前后模型或预测发生可测变化；
- NoRaw 与 NoRaw-silent 的结果不同；
- 如果交换两个客户端的消息，下一轮结果随之改变。

如果正向控制都无法通过，则不得继续隐私实验，说明协作实现尚未闭环。

### 2.4 每轮必须记录的日志

每个客户端、每一轮至少记录：

```text
round_id
outgoing_message_hash
received_message_count
received_message_hash
received_payload_norm
model_hash_before_receive
model_hash_after_receive
model_delta_l2
prediction_disagreement_before_after_receive
adapt_loss_before_after_receive
```

其中 `model_delta_l2=0` 或 prediction disagreement 始终为 0，通常意味着消息没有进入模型计算路径。

### 2.5 实验 A 的通过标准

实验 A 只有在以下条件全部满足时才算通过：

1. 正向控制中交换消息会改变下一轮预测；
2. NoRaw 与 NoRaw-silent 至少在一个可控任务上不同；
3. CIFAR 实验中能记录非零的接收消息和模型/预测变化；
4. 评估发生在最后一轮消息广播之后，或明确报告“广播前”和“广播后”两种结果。

---

## 3. 实验 B：修正客户端划分与 TTA 效用基线

### 3.1 划分原则

重新生成并保存所有客户端索引。每个客户端必须明确拥有：

- 相同或近似相同的成员样本数；
- 相同或近似相同的非成员样本数；
- 相同或近似相同的评测样本数；
- 成员、非成员、评测集合互不重叠；
- 不同客户端之间是否重叠必须显式记录。

建议第一版使用：

|设置|K=4 每客户端|K=8 每客户端|
|---|---:|---:|
|成员|1000|500|
|非成员|1000|500|
|评测|500|250|

如果为了模拟不同数据量而使用不均衡客户端，必须把它单独命名为 `size_heterogeneous`，不能命名为 `iid`。

### 3.2 三种数据划分

#### IID

每个客户端的类别比例和 corruption 比例近似相同，数据量也近似相同。

#### Label-skew

客户端类别比例不同，但每个客户端总样本量和评测量相同。应记录每个客户端的真实标签直方图和伪标签直方图。

#### Domain-skew

客户端面对不同 corruption 或不同 severity。应明确这是“域识别风险”场景，而不是 IID 场景。

### 3.3 先跑不带消息的效用基线

在重新跑隐私攻击前，先运行：

1. Source/no adaptation；
2. Local-only Tent；
3. Local-only EATA 或 SAR；
4. Raw centralized adaptation；
5. NoRaw-silent；
6. NoRaw。

所有方法必须使用同一批评测索引和同一测试流顺序。

### 3.4 TTA 效用通过标准

在 CIFAR-100-C Gaussian noise severity 5 上，Local-only 不应出现无解释的灾难性坍缩。如果 Local-only Top-1 仍约为 1%，必须先检查：

- 模型是否在正确的 train/eval 模式；
- BN 统计是否被错误重置或错误聚合；
- 评测标签和预测索引是否错位；
- 每轮是否重复使用同一批数据；
- 客户端是否因伪标签坍缩到单一类别；
- 全局效用是否被一个异常大客户端支配。

只有当 Source、Local-only 和 Raw oracle 的数值关系合理，才能解释 NoRaw 的隐私—效用权衡。

### 3.5 效用报告方式

同时报告两种平均：

- **client-macro：**先算每客户端 Top-1，再对客户端平均；
- **sample-weighted：**所有客户端评测样本合并后计算全局 Top-1。

如果两者差异很大，主文必须解释客户端数据量异质性，不能只报告一个加权总数。

---

## 4. 实验 C：定义 transcript 可见性

### 4.1 三类字段

所有消息字段必须归入下表之一：

|类别|含义|示例|
|---|---|---|
|wire payload|真正跨信任边界传输的内容|原型向量、域向量、输出分布|
|public header|协议公开给接收方的字段|消息长度、时间戳、client ID、版本号|
|private diagnostics|仅用于实验记录，不应被攻击者看到|真实标签、完整 class histogram、候选 membership label|

`scenario.json` 可以保存 diagnostics，但攻击器不得自动读取它们。

### 4.2 三种攻击观察面

每种攻击都要分别跑：

1. **payload-only：**只读取 wire payload；
2. **payload + header：**读取 payload 和公开 header；
3. **full transcript：**读取协议声明的全部可见字段。

如果 `class_presence`、`class_counts` 或 `client_id` 是公开字段，应另外报告一个“直接读取 baseline”。例如公开类别计数时，类别存在性泄露不应通过训练攻击器间接测量，而应直接报告可恢复率。

### 4.3 当前输出必须整改的地方

当前消息对象中已经出现 `client_id`、`domain_label`、`class_presence`、`class_counts` 等字段，但攻击结果没有表现出一致的直接泄露。这表明以下两种情况至少有一种成立：

- 这些字段只是诊断字段，实际没有被攻击器读取；
- 攻击标签或字段映射存在错误。

下一版输出必须同时保存：

- `wire_payload.npz` 或等价的压缩文件；
- `public_header.jsonl`；
- `private_diagnostics.jsonl`；
- 攻击器实际读取的字段清单；
- 候选样本级攻击分数。

---

## 5. 实验 D：修正攻击定义

### 5.1 样本级成员推断

目标是判断候选样本 (x) 是否属于客户端 (k) 的适应流，而不是判断某一条消息属于哪个客户端。

每个候选样本必须有一个分数。候选集应满足：

- member 与 non-member 数量相同；
- 相同 corruption 和 severity；
- 相同预处理；
- 候选样本不参与攻击器训练；
- 每个客户端独立计算分数；
- 报告每客户端结果和总体结果。

攻击指标包括：

\[
\mathrm{AUC}^{*}=\max(\mathrm{AUC},1-\mathrm{AUC}),
\]

以及 attack advantage、bootstrap/DeLong 置信区间。不能因为原始 AUC 小于 0.5 就直接得出“没有泄露”，因为攻击者可能反转分数方向。

### 5.2 客户端/域识别

目标是根据一条或一段消息判断来源客户端或域。

- K=4 时随机准确率为 0.25；
- K=8 时随机准确率为 0.125；
- 同时报告 accuracy 和 balanced accuracy；
- 训练型攻击必须使用独立的攻击训练集和测试集；
- full transcript 中如果含有 client ID，直接读取应作为上限，而不是与 payload 攻击混在一起。

### 5.3 类别存在性推断

目标是判断客户端或窗口是否出现类别 (c)。

- 正负类别窗口数量必须平衡；
- 不能让所有“正样本”都来自一个客户端；
- 如果 class ID/count 明文传输，直接读取结果就是基线；
- 如果 class ID/count 不传输，攻击器只能读取 payload 和声明的 header；
- 报告 macro accuracy、balanced accuracy 和每类别结果。

### 5.4 攻击指标一致性检查

当前部分结果中 AUC、advantage 和 accuracy 的数值关系不稳定。下一版代码必须给出指标定义，并通过人工构造的可分离数据测试：

- 完美可分时 AUC 应接近 1；
- 完全随机时 AUC 应接近 0.5；
- 反向可分时应通过 (mathrm{AUC}^{*}) 反映攻击能力；
- 二分类直接读取标签时 balanced accuracy 应接近 1。

---

## 6. 实验 E：最小分布指纹泄露复验矩阵

在 A–D 全部通过后，只运行下面的最小矩阵，不要立刻恢复原来的 63 场景。

### 6.1 固定配置

- 数据集：CIFAR-100-C；
- corruption：Gaussian noise severity 5；
- 模型：ResNet-18；
- 客户端数：K=4；
- carrier：prototype；
- 每个主条件：3 个随机种子；
- 客户端 eval 数量：均衡；
- 适应器：先使用已经验证不坍缩的版本。

### 6.2 实验因素

|因素|取值|
|---|---|
|split|IID、label-skew、domain-skew|
|发布轮数|1、5、10|
|聚合规模|精确 (n_c=1,4,16)|
|协议|Local-only、NoRaw-silent、NoRaw|
|观察面|payload-only、payload+header、full transcript|

这里的 (n_c) 必须表示“实际参与该消息的样本数”，而不是简单的选择上限。每条 prototype 消息应明确保存：

```text
actual_count
effective_count
class_id
aggregation_weight
```

如果仍然使用 per-class cap，必须将变量命名为 `class_cap`，不能直接把它解释成匿名集大小。

### 6.3 第一优先级结果

先看客户端/域/类别级风险：

- source-ID accuracy；
- domain-ID accuracy；
- class-presence balanced accuracy；
- 随发布轮数变化的风险曲线；
- 随 label/domain heterogeneity 变化的风险曲线。

样本级 MIA 作为第二优先级，不得让其成为唯一的 go/no-go 条件。

### 6.4 主结果表模板

|split|rounds|actual (n_c)|协议|utility macro|utility weighted|MIA AUC*|source/domain ID|class presence|bytes|
|---|---:|---:|---|---:|---:|---:|---:|---:|---:|
|IID|1|1|Local-only|||||||
|IID|1|1|NoRaw-silent|||||||
|IID|1|1|NoRaw|||||||
|label-skew|5|4|NoRaw|||||||
|label-skew|10|4|NoRaw|||||||
|domain-skew|5|4|NoRaw|||||||

---

## 7. 实验 E 的判定规则

### 7.1 Go：进入新的 D1 主线

满足以下条件时，正式将方向改为“协作 TTA 中的分布指纹隐私审计”：

1. NoRaw 已被正向控制证明真正消费接收消息；
2. 修正划分后 NoRaw 相对 Local-only 有稳定、可重复的效用收益；
3. label/domain heterogeneity 或重复发布导致 source/domain/class 风险显著高于随机；
4. 结果在至少两个 seed/多个客户端划分中保持方向一致；
5. 泄露不能仅由未声明的诊断字段造成。

此时研究重点应从“恢复单张样本”改为“分布指纹泄露”，后续再设计隐私保护消息协议。

### 7.2 Conditional Go：做审计基准，不做安全算法

如果客户端/类别泄露稳定，但 NoRaw 效用收益很小，则把项目收缩为：

> 协作 TTA 消息级隐私审计基准与分布指纹风险地图。

此时不应强行加入复杂的 DP、随机投影和风险自适应预算，只交付统一威胁模型、攻击套件、协议复现和风险分析。

### 7.3 No-Go：停止 D1

如果修复后同时出现以下情况：

- NoRaw 仍无稳定效用收益；
- source/domain/class 攻击接近随机；
- 样本级 MIA 也无可重复信号；
- 结果高度依赖单个 seed 或数据划分；

则停止 D1，不再继续增加实验复杂度，转向 D6 LT²A 或其他备选方向。

---

## 8. 若新方向通过，后续论文问题如何改写

### 8.1 推荐问题定义

> Collaborative TTA messages communicate distribution knowledge across clients. This work asks whether the same messages expose client-, domain-, and class-level fingerprints under heterogeneous and repeated communication.

中文可表述为：

> 协作 TTA 消息在共享分布知识的同时，是否会暴露客户端、域和类别结构指纹？这种泄露如何随异质性、消息粒度和重复发布变化？

### 8.2 推荐贡献结构

1. 定义协作 TTA 的消息级 transcript 和分布指纹隐私威胁模型；
2. 建立覆盖 payload、header 和 full transcript 的统一审计基准；
3. 发现并验证非 IID 和重复发布下的客户端/域/类别泄露规律；
4. 在规律成立后设计带正式隐私会计的消息发布机制。

第 4 项只有在第 1–3 项结果稳定后才加入主论文。

### 8.3 暂定题目

- `Distribution Fingerprints in Collaborative Test-Time Adaptation`；
- `When Collaborative Test-Time Adaptation Reveals Client Distributions`；
- `Auditing Client-, Domain-, and Class-Level Leakage in Collaborative TTA`。

题目暂时不要使用“secure”“private”或“provably safe”，除非已经完成正式的隐私定义和证明。

---

## 9. 实验输出与复现清单

每次正式运行必须保存：

- 配置文件；
- 随机种子；
- 客户端划分索引；
- 成员/非成员/评测索引；
- source、Local-only、NoRaw-silent、NoRaw、Raw 的模型 hash；
- 每轮 outgoing/received message hash；
- 实际 payload 文件；
- public header 和 private diagnostics 分离文件；
- 候选样本级攻击分数；
- 每客户端效用；
- client-macro 和 sample-weighted 汇总；
- bootstrap/DeLong 置信区间；
- 失败日志和异常客户端报告。

建议目录结构：

```text
run_id/
├── config.json
├── splits.npz
├── protocol_manifest.json
├── messages/
│   ├── wire_payload.npz
│   ├── public_header.jsonl
│   └── private_diagnostics.jsonl
├── utility/
│   ├── source.json
│   ├── local_only.json
│   ├── noraw_silent.json
│   ├── noraw.json
│   └── raw_oracle.json
├── attacks/
│   ├── member_scores.npz
│   ├── source_domain_scores.npz
│   └── class_presence_scores.npz
└── summary.json
```

---

## 10. 操作优先级

### 现在立即做

- [ ] 实现 NoRaw-silent；
- [ ] 实现 received-message 日志和模型 hash；
- [ ] 完成人工正向控制；
- [ ] 重新生成均衡客户端划分；
- [ ] 跑 Source/Local-only 效用回归。

### A、B 通过后做

- [ ] 分离 wire payload、public header、private diagnostics；
- [ ] 保存实际 payload；
- [ ] 修正 candidate-level MIA；
- [ ] 加入 AUC* 和置信区间；
- [ ] 验证 class/source 直接读取 baseline。

### 最后做

- [ ] 运行 K=4 最小分布指纹矩阵；
- [ ] 生成 Go / Conditional Go / No-Go 表；
- [ ] 与导师讨论是否正式改题；
- [ ] 只有 Go 后才开始保护协议。

---

## 11. 最终判断模板

补充实验完成后，用下面的格式向导师汇报：

```text
协议有效性：通过 / 不通过
客户端划分：均衡且可复现 / 仍有问题
Local-only 效用：正常 / 坍缩
NoRaw 协作收益：显著 / 微弱 / 无
样本级 MIA：显著 / 不稳定 / 随机
客户端或域识别：显著 / 不稳定 / 随机
类别存在性：显著 / 不稳定 / 随机
重复发布效应：成立 / 不成立
最终决策：Go / Conditional Go / No-Go
```

在当前已有结果基础上，最可能的合理路线是：

> 先修复实验协议；如果群体级泄露现象仍然存在，则把 D1 重定位为“协作 TTA 的分布指纹隐私审计”；如果现象消失，则停止 D1，而不是继续用复杂防护模块挽救一个没有稳定现象的问题。

