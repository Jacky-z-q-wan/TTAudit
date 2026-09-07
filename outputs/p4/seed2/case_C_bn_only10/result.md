# Tent 10遍 BN-only Summary

- Seed: 2
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed2/case_C_bn_only10/case_C_bn_only10_seed2.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p4/seed2/case_C_bn_only10

| Bucket | count | score_ent AUC | score_conf AUC | score_ce AUC |
|---|---:|---:|---:|---:|
| tail | 120 | 0.4283 | 0.4642 | 0.4939 |
| mid | 600 | 0.4766 | 0.4778 | 0.4543 |
| head | 3200 | 0.4912 | 0.4863 | 0.4795 |

- Tail minus head (score_ent): -0.0629
- Tail minus head (score_conf): -0.0221
- Top-1: 36.70

| Split | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.5112 | 0.5106 | 0.4776 | 11.70 |
| Tent 10遍 BN-only | 0.4793 | 0.4788 | 0.4797 | 36.70 |
