# TTAudit P1 Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/datasets/best_model.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p1_seed0

| System | Rounds | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 | adapt loss |
|---|---:|---:|---:|---:|---:|---:|
| Source | 0 | 0.5023 | 0.5029 | 0.4976 | 11.70 | - |
| Tent | 1 | 0.5096 | 0.5126 | 0.4997 | 41.20 | 1.508513 |
| Tent | 2 | 0.5238 | 0.5271 | 0.5006 | 44.40 | 1.405709 |
| Tent | 5 | 0.5506 | 0.5531 | 0.5059 | 49.40 | 1.213206 |
| Tent | 10 | 0.5895 | 0.5915 | 0.5078 | 49.70 | 1.038730 |
