# BufTTA 强回放 Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p3_seed0/line_4_buftta_strong/buftta_replay1_seed0.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p3_seed0/line_4_buftta_strong

| System | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.5023 | 0.5029 | 0.4976 | 11.70 |
| BufTTA 强回放 | 0.5183 | 0.5240 | 0.5027 | 42.70 |

- Buffer size cap: 500
- Buffer threshold: 0.7
- Replay every: 1
- Replay batch size: 64
- Final buffer length: 500

- Adaptation steps: 32
- Clip norm: -
- Frozen affine: False
