# TTAudit P4 Seed Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/datasets/best_model.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed0

| Config | tail(1~3) AUC | mid(10) AUC | head(40) AUC | tail−head | Top-1 |
|---|---:|---:|---:|---:|---:|
| Tent 10遍 BN+affine | 0.6447 | 0.6072 | 0.5865 | 0.0582 | 50.00 |
| Tent 1遍 BN+affine | 0.5792 | 0.5247 | 0.5029 | 0.0762 | 38.50 |
| Tent 10遍 BN-only | 0.5542 | 0.5023 | 0.4926 | 0.0616 | 36.30 |
