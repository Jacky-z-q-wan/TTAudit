# Tent 1遍 BN+affine Summary

- Seed: 2
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed2/case_B_tent1/case_B_tent1_seed2.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed2/case_B_tent1

| Bucket | count | score_ent AUC | score_conf AUC | score_ce AUC |
|---|---:|---:|---:|---:|
| tail | 120 | 0.4561 | 0.4725 | 0.4861 |
| mid | 600 | 0.4733 | 0.4736 | 0.4562 |
| head | 3200 | 0.4981 | 0.4944 | 0.4803 |

- Tail minus head (score_ent): -0.0420
- Tail minus head (score_conf): -0.0219
- Top-1: 38.50

| Split | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.5112 | 0.5106 | 0.4776 | 11.70 |
| Tent 1遍 BN+affine | 0.4893 | 0.4873 | 0.4837 | 38.50 |
