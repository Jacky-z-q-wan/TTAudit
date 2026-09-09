# Tent 1遍 BN+affine Summary

- Seed: 1
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed1/case_B_tent1/case_B_tent1_seed1.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed1/case_B_tent1

| Bucket | count | score_ent AUC | score_conf AUC | score_ce AUC |
|---|---:|---:|---:|---:|
| tail | 120 | 0.5642 | 0.5619 | 0.5744 |
| mid | 600 | 0.5102 | 0.5167 | 0.5196 |
| head | 3200 | 0.5052 | 0.5060 | 0.5095 |

- Tail minus head (score_ent): 0.0590
- Tail minus head (score_conf): 0.0560
- Top-1: 37.60

| Split | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.4846 | 0.4838 | 0.5116 | 11.70 |
| Tent 1遍 BN+affine | 0.5020 | 0.5045 | 0.5004 | 37.60 |
