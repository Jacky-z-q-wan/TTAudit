# Tent 10遍 BN-only Summary

- Seed: 1
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed1/case_C_bn_only10/case_C_bn_only10_seed1.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed1/case_C_bn_only10

| Bucket | count | score_ent AUC | score_conf AUC | score_ce AUC |
|---|---:|---:|---:|---:|
| tail | 120 | 0.5581 | 0.5694 | 0.5814 |
| mid | 600 | 0.4996 | 0.5017 | 0.5220 |
| head | 3200 | 0.5063 | 0.5092 | 0.5092 |

- Tail minus head (score_ent): 0.0517
- Tail minus head (score_conf): 0.0602
- Top-1: 36.60

| Split | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.4846 | 0.4838 | 0.5116 | 11.70 |
| Tent 10遍 BN-only | 0.4990 | 0.5004 | 0.5014 | 36.60 |
