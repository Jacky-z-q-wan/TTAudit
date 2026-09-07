# Tent 10遍 + 裁剪 C=1 Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p3_seed0/line_5_clip_c1/clipped_seed0.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p3_seed0/line_5_clip_c1

| System | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.5023 | 0.5029 | 0.4976 | 11.70 |
| Tent 10遍 + 裁剪 C=1 | 0.5032 | 0.5071 | 0.4999 | 38.60 |

- Adaptation steps: 320
- Clip norm: 1.0
- Frozen affine: False
