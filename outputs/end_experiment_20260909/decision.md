# End 终局验证结论

真实方法与 commit：Latte / 7c2e02501e56e86dc6fa9e78ec606201a9674c55
官方效用复现：失败
协作相对 local-only 的 3-seed 增益：-0.0579 Top-1 points
审计 hook 等价性：通过
攻击训练/测试客户端是否完全隔离：否/未运行
时间窗口图像是否完全隔离：否/未运行
固定攻击预算：否/未运行

## 门槛
- G1：失败
- G2：失败
- G3：失败
- G4：失败

## 详细记录
```json
{
  "official_repository": "https://github.com/baowenxuan/Latte.git",
  "official_commit": "7c2e02501e56e86dc6fa9e78ec606201a9674c55",
  "g1": {
    "passed": false,
    "mean_gain_top1_points": -0.05787037037037016,
    "positive_seed_count": 1,
    "seed_count": 3,
    "seed_rows": [
      {
        "seed": 0,
        "mean_collaborative_minus_local_top1": 0.6076388888888895,
        "benchmark_gains": [
          0.0,
          1.215277777777779
        ],
        "positive_benchmark_count": 1,
        "minimum_client_gap": -0.02083333333333337,
        "minimum_collaborative_client_accuracy": 0.0625,
        "hook_max_logit_abs_diff": 0.0,
        "hook_predictions_identical": true
      },
      {
        "seed": 1,
        "mean_collaborative_minus_local_top1": -0.08680555555555247,
        "benchmark_gains": [
          0.0,
          -0.17361111111110494
        ],
        "positive_benchmark_count": 0,
        "minimum_client_gap": -0.0625,
        "minimum_collaborative_client_accuracy": 0.10416666666666667,
        "hook_max_logit_abs_diff": 0.0,
        "hook_predictions_identical": true
      },
      {
        "seed": 2,
        "mean_collaborative_minus_local_top1": -0.6944444444444475,
        "benchmark_gains": [
          0.0,
          -1.388888888888895
        ],
        "positive_benchmark_count": 0,
        "minimum_client_gap": -0.10416666666666663,
        "minimum_collaborative_client_accuracy": 0.08333333333333333,
        "hook_max_logit_abs_diff": 0.0,
        "hook_predictions_identical": true
      }
    ],
    "no_client_collapse": true,
    "minimum_client_gap": -0.10416666666666663,
    "hook_equivalence_passed": true,
    "thresholds": {
      "mean_gain_top1_points": 1.0,
      "positive_seed_count": 2,
      "maximum_allowed_client_drop": -0.2,
      "maximum_logit_abs_diff": 0.0001
    }
  },
  "g2": {
    "passed": false,
    "status": "not_run_because_G1_failed"
  },
  "g3": {
    "passed": false,
    "status": "not_run_because_G1_failed"
  },
  "g4": {
    "passed": false,
    "status": "not_run_because_G1_failed"
  },
  "final_decision": "D1 全面归档"
}
```

## 最终决定
**D1 全面归档**
