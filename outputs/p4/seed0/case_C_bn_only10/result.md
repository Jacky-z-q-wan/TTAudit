# Tent 10遍 BN-only Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed0/case_C_bn_only10/case_C_bn_only10_seed0.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed0/case_C_bn_only10

| Bucket | count | score_ent AUC | score_conf AUC | score_ce AUC |
|---|---:|---:|---:|---:|
| tail | 120 | 0.5542 | 0.5706 | 0.5169 |
| mid | 600 | 0.5023 | 0.5116 | 0.4972 |
| head | 3200 | 0.4926 | 0.4934 | 0.4884 |

- Tail minus head (score_ent): 0.0616
- Tail minus head (score_conf): 0.0772
- Top-1: 36.30

| Split | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.4878 | 0.4889 | 0.4630 | 11.70 |
| Tent 10遍 BN-only | 0.4737 | 0.4800 | 0.4733 | 36.30 |
