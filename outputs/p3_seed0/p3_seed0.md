# TTAudit P3 Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/datasets/best_model.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p3_seed0

| # | System | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 | 备注 |
|---|---|---:|---:|---:|---:|---|
| 0 | Source | 0.5023 | 0.5029 | 0.4976 | 11.70 | anchor |
| 1 | Tent 1遍 BN+affine | 0.5096 | 0.5126 | 0.4997 | 41.20 | anchor |
| 2 | Tent 10遍 BN+affine | 0.5895 | 0.5915 | 0.5078 | 49.70 | anchor |
| 3 | Tent 10遍 BN-only | 0.4957 | 0.4990 | 0.4998 | 36.80 | 关键行 |
| 4 | BufTTA 强回放 | 0.5183 | 0.5240 | 0.5027 | 42.70 | 关键行 |
| 5 | Tent 10遍 + 裁剪 C=1 | 0.5032 | 0.5071 | 0.4999 | 38.60 | 防御行 |
