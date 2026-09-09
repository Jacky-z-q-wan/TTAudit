# D1-F 最终判决

官方仓库与提交：https://github.com/baowenxuan/Latte.git @ 7c2e02501e56e86dc6fa9e78ec606201a9674c55
是否使用作者完整缓存：否（本机未发现满足断言的缓存）
CIFAR 总评测样本数：未运行（硬检查失败）
VLCS 总评测样本数：未运行（硬检查失败）

## 数据与配置硬检查

- 结果目录：`/home/chenyaofo/wanzhangqi/GuessWork2/outputs/d1_f_20260909`
- 运行环境：`cc`（本次由 `cc` 环境执行）
- 模型：`ViT-B/16`
- 截断设置：未启用；未使用每客户端 48/64 张样本。
- 失败阶段：preflight
- 失败原因：CIFAR100CFull: author_full_cache_unavailable, official_full_raw_data_unavailable；VLCS: author_full_cache_unavailable, full_raw_feature_encoding_not_prepared
- 预检观察值：CIFAR-100-C-Full 官方完整目录未找到；本机标准 CIFAR-100-C 为每扰动 10000 张；VLCS 本地目录为 3219 张图像。
- 预检详情：见 `official_reproduction/dataset_assertions.json`。

## 五种子官方复现

- CIFAR 基础模型：未运行
- CIFAR Latte：未运行
- VLCS 基础模型：未运行
- VLCS Latte：未运行
- R1：失败（硬检查未通过，按预注册规则停止）

## 后续阶段

- 完整 Latte 相对只使用本地记忆：未运行
- CIFAR 平均增益及客户端置信区间：未运行
- VLCS 平均增益及客户端置信区间：未运行
- R2：失败/未运行
- 被动记录等价性：未运行
- 被动记录最大输出差：未运行
- 预测是否完全相同：未运行
- R3：失败/未运行
- 隐藏域推断：未运行
- 纯消息：未运行
- 仅消息头：未运行
- 客户端置信区间：未运行
- 匿名跨时间关联：未运行
- q=1/2/4/8：未运行
- 匿名关联客户端置信区间：未运行
- 随机标签控制：未运行
- 客户端是否隔离：未进入攻击阶段
- 新输入图像是否隔离：未进入攻击阶段
- 攻击预算是否固定：未进入攻击阶段

## G 门槛

- G1：失败
- G2：失败/未运行
- G3：失败/未运行
- G4：失败/未运行

## 最终决定

**A2 / D1 / D1-F 全面归档**

依据 D1-F 的硬规则，本机只有标准 `CIFAR-100-C`（每扰动 10000 张）和 3219 张 VLCS，不能将它们替代为官方 `CIFAR-100-C-Full` 的 60000 张和 VLCS 的 10729 张；作者缓存下载端点在本次运行中不可访问。因此没有生成任何不符合预注册规模的科学结论。
