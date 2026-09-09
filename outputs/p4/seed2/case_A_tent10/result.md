# Tent 10遍 BN+affine Summary

- Seed: 2
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed2/case_A_tent10/case_A_tent10_seed2.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed2/case_A_tent10

| Bucket | count | score_ent AUC | score_conf AUC | score_ce AUC |
|---|---:|---:|---:|---:|
| tail | 120 | 0.6358 | 0.6428 | 0.5183 |
| mid | 600 | 0.5503 | 0.5536 | 0.4698 |
| head | 3200 | 0.5758 | 0.5760 | 0.4892 |

- Tail minus head (score_ent): 0.0601
- Tail minus head (score_conf): 0.0668
- Top-1: 49.60

| Split | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.5112 | 0.5106 | 0.4776 | 11.70 |
| Tent 10遍 BN+affine | 0.5810 | 0.5830 | 0.4991 | 49.60 |
