# Tent 10遍 BN+affine Summary

- Seed: 1
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed1/case_A_tent10/case_A_tent10_seed1.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed1/case_A_tent10

| Bucket | count | score_ent AUC | score_conf AUC | score_ce AUC |
|---|---:|---:|---:|---:|
| tail | 120 | 0.6333 | 0.6225 | 0.5578 |
| mid | 600 | 0.5956 | 0.6000 | 0.5078 |
| head | 3200 | 0.5909 | 0.5928 | 0.5110 |

- Tail minus head (score_ent): 0.0424
- Tail minus head (score_conf): 0.0297
- Top-1: 49.90

| Split | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.4846 | 0.4838 | 0.5116 | 11.70 |
| Tent 10遍 BN+affine | 0.6032 | 0.6046 | 0.5212 | 49.90 |
