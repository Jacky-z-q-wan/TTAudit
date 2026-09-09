# 结束实验

## ——真实协作 TTA 方法上的分布指纹泄露终局验证

**版本：**v1.0  
**日期：**2026-09-09  
**性质：**D1 系列的最后一次方向判决实验  
**时间上限：**10–14 天  
**最终结果：**只允许“新方向 Go”或“D1 全面归档”两种科学结论；不再追加第三轮抢救实验。

---

## 0. 本实验到底验证什么

原始 D1 已经结束。本实验不再验证以下假设：

- 不再尝试证明类原型会稳定泄露单张样本成员身份；
- 不再尝试证明样本级 MIA 随 (n_c) 减小而单调增强；
- 不再为当前自制 NoRaw 协议设计 DP、加噪、随机投影或预算分配；
- 不再把当前自制 prototype 协议的效用当作论文方法结果。

本实验只验证一个由 D1 派生的新问题：

> 在真实、已发表且确实能产生协作收益的 TTA 方法中，跨客户端消息是否会暴露可泛化的域/属性信息，或者使匿名客户端的多轮消息可以被持续链接？

如果答案为是，则另立新项目“协作 TTA 的分布指纹泄露”；如果答案为否，则 D1 及其衍生方向全部归档。

---

## 1. 已有证据与本次实验的必要性

补充实验 `outputs/a2_supplement_20260908` 已经确认：

1. NoRaw 正向控制通过，消息确实能改变预测；
2. 81 个场景的客户端划分已经均衡；
3. wire payload、public header、private diagnostics 已经分离；
4. 样本级成员推断 AUC* 约为 0.504–0.508，没有可用泄露；
5. class cap 1/4/16 没有形成稳定的隐私规律；
6. 当前 NoRaw 相对 Local-EATA 的效用不稳定，81 个场景没有一个达到原定协作生死线；
7. payload-only 的客户端识别和 label-skew 类别存在性识别明显高于随机。

最后一条信号仍有三个混杂因素：

- 当前协议是自制的 `prototype_supplement`，不是已发表方法的完整实现；
- 轮数增加时攻击训练消息也从 16 增至 80/160，泄露趋势可能混入“训练样本更多”的效应；
- 旧攻击可能把同一客户端、相邻轮次的高度相关消息随机分到训练和测试，不能证明对新时间或新客户端的泛化。

因此，本实验必须在真实方法、固定攻击预算和严格分组划分下复验。

---

## 2. 方法选择

### 2.1 主方法：Latte

优先使用 Latte 的官方实现，因为它直接交换类原型，最适合检验类别结构、域属性和消息链接风险。

必须完成：

- 保存官方仓库地址、commit hash 和环境文件；
- 运行官方提供的至少一个 corruption 或 domain adaptation 配置；
- 区分 local memory 与 external/shared memory；
- 在不改变算法数值的条件下截获客户端发给服务器的真实原型消息；
- 记录服务器返回给客户端的真实外部原型。

本地目录目前没有 Latte 代码，因此不得把现有自制 prototype 协议重命名为 Latte。

### 2.2 备选/确认方法：CoLA

本地已有 `D:\Desktop\Paper\C-cubed\COLA`。如果 Latte 因环境问题在 4 天内无法跑通，可用 CoLA 作为主验证对象；如果 Latte 通过终局门槛，再用 CoLA 做一次载体外部验证。

CoLA 中需要截获的真实消息是 domain knowledge vector，而不是自行设计的新原型。

### 2.3 方法选择纪律

```text
优先 Latte
  ├─ 4 天内复现成功 → 用 Latte 做完整终局实验
  └─ 仅因工程原因无法复现 → 切换本地 CoLA

若真实方法本身没有复现出协作收益 → 实验直接失败
```

不能因为某个真实方法不产生预期泄露，就换回自制协议寻找更高数字。

---

## 3. 威胁模型重新定义

### 3.1 不再把普通 source-ID 作为主要隐私风险

在经典诚实但好奇服务器模型中，服务器通常知道消息来自哪个客户端，因此“四分类猜客户端 ID”本身不构成有意义的隐私发现。

本实验只保留两个有明确意义的威胁：

### T1：隐藏域/敏感属性推断

服务器知道消息属于客户端 (k)，但不知道该客户端当前数据流的隐藏属性 (a_k)，例如：

- 图像来自哪一种采集域或 corruption；
- 客户端是否正在经历某种特殊分布；
- 某类或某组类别是否以高比例出现；
- 在实际应用解释中，可对应医院扫描设备、天气/道路环境或罕见病理类别存在性。

攻击者从真实消息中预测该隐藏属性。

### T2：匿名消息跨轮链接

消息通过匿名中继或去标识化日志发布，不包含 client ID。攻击者判断两组来自不同时段的消息是否属于同一个客户端。

这个任务不是 K 类封闭集 source-ID，而是 same-client / different-client 的二分类验证任务，并要求在训练时从未出现过的客户端上测试。

### 3.2 次要负对照

样本级 MIA 只作为负对照保留，用于确认此前 AUC*≈0.5 的结论。它不再决定本实验是否 Go，也不再扫描 (n_c) 规律。

---

## 4. 阶段 A：真实方法效用复现

### 4.1 目的

先确认被审计的方法确实“值得通信”。如果真实协作方法不能优于它自己的 local-only 版本，那么隐私审计缺乏论文动机。

### 4.2 对照

以 Latte 为例：

|方法|本地历史消息|外部客户端消息|用途|
|---|---:|---:|---|
|Source/Zero-shot CLIP|否|否|源模型基线|
|Local-only memory|是|否|无跨端协作|
|Latte collaborative|是|是|真实协作方法|
|Centralized/Raw oracle|集中|集中|仅作上界|

对于 CoLA，应使用论文/官方代码定义的单设备 TTA 和 CoLA 协作版本，不自行替换聚合逻辑。

### 4.3 数据与种子

至少选择：

- 一个官方 corruption benchmark 配置；
- 一个官方 domain-shift 配置；
- 3 个随机种子；
- 每个客户端数量相同或报告 client-macro 指标。

如果算力不足，优先完整跑一个 benchmark，而不是在多个 benchmark 上各跑一个不完整子集。

### 4.4 效用复现通过标准

真实协作方法必须同时满足：

1. 3-seed 平均性能优于对应 local-only；
2. 至少 2/3 seeds 的提升方向一致；
3. 平均提升至少 1 个 Top-1 百分点，或者达到原论文所报告协作增益的 50%；
4. 不发生某个客户端或类别的大幅坍缩；
5. 消息截获开关开启/关闭时，模型预测误差差异不超过数值误差，证明审计钩子没有改变算法。

如果上述条件不成立，直接执行最终 No-Go，不再运行攻击大矩阵。

---

## 5. 阶段 B：真实消息截获与数据隔离

### 5.1 消息截获原则

必须截获方法本来就会传输的消息，不得重新计算一个“类似消息”代替。

每条消息保存：

```text
protocol_name
official_commit
round_id
window_id
pseudonym_for_diagnostics_only
payload
payload_shape
payload_dtype
payload_bytes
message_hash
public_header
private_diagnostics
```

### 5.2 三层文件隔离

```text
wire_payload.npz          # 攻击者可见的真实数值
public_header.jsonl       # 协议公开字段
private_diagnostics.jsonl # client/domain/真实标签等评测真值
```

攻击代码不得读取 `private_diagnostics` 作为输入，只能用它生成评测标签。

### 5.3 审计钩子等价性测试

固定同一随机种子分别运行：

- official method without hook；
- official method with passive logging hook。

逐批比较 logits，并记录：

\[
\max |z_{\text{hook}}-z_{\text{official}}|.
\]

通过标准：预测类别完全一致，logit 最大误差处于浮点数值误差范围内。

---

## 6. 阶段 C：审计数据构造

### 6.1 客户端与域设置

建议使用 (K=12) 或 (K=16) 个虚拟客户端，至少保证每个域有 3 个客户端。示例：

```text
4 个隐藏域 × 每域 4 个客户端 = 16 clients
```

不能再让“一个客户端唯一对应一个域”，否则无法区分客户端记忆和域属性推断。

### 6.2 域属性推断数据

每个域使用不同 corruption/domain，但不同客户端使用互不重叠的图像索引。攻击训练和测试按客户端分组：

- 训练：每个域的一部分客户端；
- 测试：每个域剩余的、训练阶段从未出现的客户端；
- 所有 seed 独立运行；
- 不允许同一客户端的消息同时进入攻击训练集和测试集。

这是真正的 leave-client-out domain inference。

### 6.3 类别结构/属性推断数据

不再使用“四个客户端各占互斥 25 类”的极端划分。采用 Dirichlet 划分：

\[
p_k \sim \operatorname{Dirichlet}(\alpha),
\]

至少运行：

- (alpha=0.3)：明显但仍有重叠的 label-skew；
- (alpha=1.0)：较温和的 label-skew。

将敏感属性定义为以下二者之一：

1. 某个预注册类别组的占比是否高于全局中位数；
2. 某个稀有类别是否在当前时间窗口真实出现。

正负样本必须平衡，且攻击训练/测试按客户端分组。

如果真实消息明确以 class ID 为索引发送原型，必须额外报告：

- 直接检查非零原型行的 read-off baseline；
- 学习型攻击相对 read-off baseline 的增量；
- 这类泄露究竟来自协议的明文语义，还是向量内容。

若只有 read-off baseline 有效，应将结果称为“协议直接披露”，不能包装成复杂的隐私攻击发现。

### 6.4 独立时间流

不同轮次不能简单重复同一批1000张适应图像。将每个客户端的数据划分为按时间不重叠的窗口：

```text
calibration/attack-train stream
validation stream
future attack-test stream
utility evaluation stream
```

不同流不得共享图像索引，避免攻击器识别重复样本或近重复原型。

---

## 7. 阶段 D：攻击 1——隐藏域/敏感属性推断

### 7.1 输入观察面

分别运行：

1. payload-only；
2. header-only；
3. payload + public header。

主要结论必须由 payload-only 支持。header-only 用于判断消息长度、计数等侧信道是否已经足够。

### 7.2 攻击模型

首先使用简单攻击：

- logistic regression；
- linear SVM 或线性 probe；
- 最近质心分类器。

只有当线性攻击不稳定但有明确证据表明非线性结构存在时，才增加小型 MLP。禁止通过大量复杂攻击器调参挑最高数字。

### 7.3 分组评估

必须以 client ID 作为 group，采用 leave-client-out 或 GroupKFold：

```text
attack train clients ∩ attack test clients = ∅
```

报告：

- balanced accuracy；
- macro-F1；
- AUROC（二分类属性）；
- 95% client-cluster bootstrap 置信区间；
- 每个域/属性的混淆矩阵。

### 7.4 控制实验

- 随机打乱域/属性标签后，结果应回到随机；
- 仅用 payload norm、message bytes 等简单统计作为侧信道基线；
- 在每个客户端内部做特征标准化，检验结果是否只来自尺度；
- 对消息行/类别索引做一致随机置换，区分类别槽位与向量内容；
- 使用 source/未适应模型的对应消息作为对照。

---

## 8. 阶段 E：攻击 2——匿名消息跨轮链接

### 8.1 任务定义

输入来自两个不同时段的消息集合 (M_a,M_b)，输出它们是否属于同一个客户端：

\[
h(M_a,M_b)\rightarrow\{\text{same client},\text{different clients}\}.
\]

正负 pair 数量相同。

### 8.2 必须使用未见客户端测试

- verifier 在 auxiliary clients 上训练；
- 在完全未见过的 target clients 上测试；
- 测试 pair 的两个时间段不得共享图像；
- 不能把同一客户端的相邻或重复窗口随机拆到训练和测试。

这样测到的是“消息是否存在可泛化客户端指纹”，而不是封闭集客户端分类或记忆具体消息。

### 8.3 固定攻击预算

为了隔离累计曝光效应，攻击训练数据始终固定。例如，无论测试曝光量是多少，攻击器只使用：

```text
每个 auxiliary client 8 条训练消息
固定数量的 same/different pairs
固定训练轮数和超参数
```

测试时改变聚合消息数：

\[
q\in\{1,2,4,8\}.
\]

比较“观察 q 条历史消息后”的链接能力，而不是让攻击训练集随 TTA 轮数同步增大。

### 8.4 时间隔离

使用严格顺序：

```text
早期流：攻击器注册/辅助消息
中期流：验证
未来流：最终链接测试
```

报告 linkability AUROC、balanced accuracy、TPR@FPR=1% 和 client-cluster bootstrap 置信区间。

### 8.5 控制实验

- 随机打乱客户端 pair 标签后回到0.5；
- 只使用 header 的链接基线；
- 匹配同域不同客户端作为困难负样本；
- 匹配不同域同客户端作为诊断样本；
- 固定 q 后比较 IID、label-skew 和 domain-skew。

---

## 9. 阶段 F：负对照 MIA

仅对一个代表性配置运行 candidate-level MIA：

- 每客户端1000 member + 1000 non-member；
- member/non-member 相同域、相同预处理；
- 候选图像与攻击训练消息严格隔离；
- 报告 AUC*、双向 advantage 和95%置信区间。

预期结果仍接近0.5。该负结果用于说明“泄露发生在分布/属性层，而不是单样本层”，不再通过改变攻击分数方向寻找阳性结果。

注意：现有 `metric_sanity.json` 中 reverse-perfect 的 AUC*=1，但 advantage=0。下一版应增加方向不变的：

\[
\mathrm{Adv}^{*}=\max(\mathrm{Adv}(s),\mathrm{Adv}(-s)).
\]

---

## 10. 统计方案

### 10.1 独立单位

- 效用比较的独立单位：客户端和 seed；
- 域/属性推断的独立单位：未见客户端；
- 链接攻击的 bootstrap 单位：客户端，而不是 message pair；
- 不把同一客户端的160条相关消息当作160个独立重复实验。

### 10.2 固定攻击容量

所有 q、轮数和 split 条件使用相同的：

- 攻击训练客户端数；
- 每客户端攻击训练消息数；
- 模型容量；
- 超参数搜索预算；
- 训练轮数。

### 10.3 多重比较

本实验只有两个主要终点：

1. leave-client-out domain/property inference；
2. unseen-client temporal linkability。

其他攻击均为次要或诊断结果。不得从大量类别和设置中事后挑最高值作为主要结论。

### 10.4 结果必须同时报告

- 每 seed 原始值；
- 3-seed 均值和标准差；
- client-cluster bootstrap 95% CI；
- 随机/多数类基线；
- 攻击失败结果；
- 协作效用和通信字节数。

---

## 11. 预注册终局门槛

### 11.1 新方向 Go

只有同时满足以下四项，才将“分布指纹泄露”正式立项：

#### G1：真实方法有效

Latte 或 CoLA 在本实验中满足阶段 A 的效用复现标准。

#### G2：至少一个有意义的隐私任务成立

以下两项至少一项通过：

- leave-client-out 域/属性推断 payload-only balanced accuracy 或 AUROC ≥0.65；
- unseen-client 跨轮链接 payload-only AUROC ≥0.65。

同时要求 client-cluster bootstrap 95% CI 下界 >0.55。

#### G3：不是数据泄漏或简单元数据

- 训练/测试客户端完全隔离；
- 时间窗口图像完全不重叠；
- 固定攻击训练预算后仍成立；
- header-only 明显弱于 payload-only，或论文明确把侧信道作为研究对象；
- 随机标签对照回到随机。

#### G4：至少具有一个外部有效性支撑

满足以下之一：

- 在两个数据集/benchmark 上成立；
- 在 Latte 与 CoLA 两类真实消息上成立；
- 在两种不同的隐藏属性上成立。

若 G1–G4 全部通过，正式另立新题，不再沿用原始 D1 的样本 MIA 叙事。

### 11.2 最终 No-Go

出现以下任一情况即全面归档 D1：

1. 真实方法无法复现协作效用；
2. 严格 client/time 隔离后攻击接近随机；
3. 固定攻击训练消息数后，“轮数越多越泄露”的趋势消失；
4. 只有互斥25类的极端 label-skew 才泄露；
5. 只有直接读取 class ID、client ID 或计数才能成功；
6. 结果只在一个 seed 或一个客户端划分成立；
7. 攻击成立的消息配置不产生任何协作收益。

### 11.3 技术无效不等于增加新实验

如果仅因显存、路径、数据下载或日志错误导致某次运行无效，可以修复后原样重跑一次；不能改变门槛、攻击目标或数据划分来寻找阳性结果。

---

## 12. 最小运行矩阵

### 12.1 必做矩阵

|模块|设置|种子|
|---|---|---|
|官方效用复现|local-only vs collaborative，1个 corruption + 1个 domain benchmark|3|
|隐藏域推断|≥3 clients/domain，leave-client-out|3|
|现实 label-skew|Dirichlet α=0.3、1.0|3|
|跨轮链接|q=1、2、4、8，unseen clients|3|
|观察面对照|payload-only、header-only、payload+header|同上|
|负对照 MIA|一个代表配置|3|

### 12.2 选做矩阵

- 第二个真实协作方法；
- 第二个 backbone；
- 防御方法；
- ImageNet 规模扩展；
- 属性推断的更多类别组。

在终局 Go 之前不得运行选做矩阵。

---

## 13. 输出目录规范

建议输出到：

```text
outputs/end_experiment_YYYYMMDD/
├── decision.md
├── preregistration.json
├── official_reproduction/
│   ├── repo_commit.txt
│   ├── config/
│   └── utility_summary.json
├── audit_equivalence/
│   └── hook_equivalence.json
├── scenarios/
│   └── <method_dataset_split_seed>/
│       ├── split_manifest.json
│       ├── wire_payload.npz
│       ├── public_header.jsonl
│       ├── private_diagnostics.jsonl
│       ├── utility.json
│       └── message_manifest.json
├── attacks/
│   ├── domain_property/
│   │   ├── group_splits.json
│   │   ├── predictions.npz
│   │   └── metrics.json
│   ├── linkability/
│   │   ├── pair_splits.json
│   │   ├── predictions.npz
│   │   └── metrics.json
│   └── member_negative_control/
│       ├── candidate_scores.npz
│       └── metrics.json
└── figures/
```

必须保存攻击训练/测试客户端 ID、时间范围、图像索引 hash 和逐样本/逐 pair 预测，否则无法判断是否发生划分泄漏。

---

## 14. 10–14 天执行计划

### Day 1–4：真实方法复现

- 获取 Latte 官方代码并固定 commit；
- 若 Latte 工程阻塞，Day 4 切换本地 CoLA；
- 完成 local-only 与 collaborative 效用对照；
- 不通过 G1 则立即停止。

### Day 5–6：消息截获与等价性

- 实现被动消息 hook；
- 完成 hook on/off logits 等价性；
- 分离 payload/header/diagnostics；
- 生成严格不重叠的客户端与时间流。

### Day 7–9：两个主要攻击

- leave-client-out 域/属性推断；
- unseen-client 跨轮链接；
- 固定攻击训练预算；
- 完成随机标签和 header-only 对照。

### Day 10–11：统计复核

- 3-seed 汇总；
- client-cluster bootstrap；
- 检查客户端、图像和时间交叉；
- 运行一次负对照 MIA。

### Day 12–14：仅用于必要的原样重跑

- 修复技术无效运行；
- 不增加新假设；
- 写出最终 Go/No-Go 决策。

---

## 15. 最终决策模板

实验结束后，`decision.md` 必须按以下格式填写：

```text
真实方法与 commit：
官方效用复现：通过 / 失败
协作相对 local-only 的 3-seed 增益：
审计 hook 等价性：通过 / 失败
攻击训练/测试客户端是否完全隔离：是 / 否
时间窗口图像是否完全隔离：是 / 否
固定攻击预算：是 / 否

Domain/property inference：
  payload-only：
  header-only：
  95% client-cluster CI：

Unseen-client linkability：
  q=1：
  q=2：
  q=4：
  q=8：
  95% client-cluster CI：

Sample-level MIA negative control：
外部有效性支撑：

G1：通过 / 失败
G2：通过 / 失败
G3：通过 / 失败
G4：通过 / 失败

最终决定：新方向 Go / D1 全面归档
```

禁止使用“Conditional Go”“再补一个实验”“换一个分数”“再换一个极端划分”等表述。

---

## 16. 通过后如何立项

如果 G1–G4 全部通过，新项目暂定为：

> **Distribution Fingerprints in Collaborative Test-Time Adaptation**  
> 协作测试时适应中的分布指纹：隐藏属性推断与跨轮可链接性

新的论文主线应当是：

1. 真实协作消息能够提升适应效用；
2. 同一消息也暴露了可跨客户端泛化的域/属性信息或匿名链接指纹；
3. 风险在严格的客户端与时间隔离下仍然成立；
4. 后续才研究如何消除指纹并保持协作效用。

原始 D1 中样本 MIA、(n_c) 单调规律和自制 NoRaw 方案不再作为核心贡献。

---

## 17. 失败后如何归档

如果任一关键门槛失败：

- 将 A1、A2 Pilot 和 supplement 整理为内部负结果记录；
- 保留消息日志、分组攻击和多客户端评测代码供其他项目复用；
- 不再围绕消息隐私设计保护算法；
- 立即切换到 D6 LT²A 或重新进行选题；
- 对导师如实汇报：原始假设经过预注册、多轮实验后未获得支持，因此及时止损。

负结果不代表研究失败。它已经帮助排除了一个缺少真实效用和样本泄露基础的方向；真正的失败是无视证据继续追加实验。

---

## 18. 最终提醒

本实验的目的不是“想办法把攻击数字跑高”，而是判断一个可发表、可泛化且威胁模型有意义的现象是否真实存在。只有真实方法有效、严格划分后攻击仍有效、结果不是元数据直接披露并且至少具有一种外部有效性支撑，才值得把它发展成论文。除此之外的结果均应执行 No-Go。

