# TTAudit P4 Seed Summary

- Seed: 1
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/datasets/best_model.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed1

| Config | tail(1~3) AUC | mid(10) AUC | head(40) AUC | tail−head | Top-1 |
|---|---:|---:|---:|---:|---:|
| Tent 10遍 BN+affine | 0.6333 | 0.5956 | 0.5909 | 0.0424 | 49.90 |
| Tent 1遍 BN+affine | 0.5642 | 0.5102 | 0.5052 | 0.0590 | 37.60 |
| Tent 10遍 BN-only | 0.5581 | 0.4996 | 0.5063 | 0.0517 | 36.60 |
