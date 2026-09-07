# Tent 1遍 BN+affine Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed0/case_B_tent1/case_B_tent1_seed0.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed0/case_B_tent1

| Bucket | count | score_ent AUC | score_conf AUC | score_ce AUC |
|---|---:|---:|---:|---:|
| tail | 120 | 0.5792 | 0.6025 | 0.5111 |
| mid | 600 | 0.5247 | 0.5393 | 0.4915 |
| head | 3200 | 0.5029 | 0.4986 | 0.4852 |

- Tail minus head (score_ent): 0.0762
- Tail minus head (score_conf): 0.1039
- Top-1: 38.50

| Split | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.4878 | 0.4889 | 0.4630 | 11.70 |
| Tent 1遍 BN+affine | 0.4841 | 0.4866 | 0.4683 | 38.50 |
