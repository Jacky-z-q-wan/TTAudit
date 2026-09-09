# Tent 10遍 BN-only Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p3_seed0/line_3_bn_only/bn_only_seed0.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p3_seed0/line_3_bn_only

| System | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.5023 | 0.5029 | 0.4976 | 11.70 |
| Tent 10遍 BN-only | 0.4957 | 0.4990 | 0.4998 | 36.80 |

- Adaptation steps: 320
- Clip norm: -
- Frozen affine: True
