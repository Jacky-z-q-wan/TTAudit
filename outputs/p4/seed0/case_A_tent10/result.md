# Tent 10遍 BN+affine Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed0/case_A_tent10/case_A_tent10_seed0.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed0/case_A_tent10

| Bucket | count | score_ent AUC | score_conf AUC | score_ce AUC |
|---|---:|---:|---:|---:|
| tail | 120 | 0.6447 | 0.6633 | 0.5464 |
| mid | 600 | 0.6072 | 0.6173 | 0.5138 |
| head | 3200 | 0.5865 | 0.5910 | 0.4857 |

- Tail minus head (score_ent): 0.0582
- Tail minus head (score_conf): 0.0723
- Top-1: 50.00

| Split | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.4878 | 0.4889 | 0.4630 | 11.70 |
| Tent 10遍 BN+affine | 0.5816 | 0.5873 | 0.4876 | 50.00 |
