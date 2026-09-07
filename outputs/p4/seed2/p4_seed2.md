# TTAudit P4 Seed Summary

- Seed: 2
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/datasets/best_model.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed2

| Config | tail(1~3) AUC | mid(10) AUC | head(40) AUC | tail−head | Top-1 |
|---|---:|---:|---:|---:|---:|
| Tent 10遍 BN+affine | 0.6358 | 0.5503 | 0.5758 | 0.0601 | 49.60 |
| Tent 1遍 BN+affine | 0.4561 | 0.4733 | 0.4981 | -0.0420 | 38.50 |
| Tent 10遍 BN-only | 0.4283 | 0.4766 | 0.4912 | -0.0629 | 36.70 |
