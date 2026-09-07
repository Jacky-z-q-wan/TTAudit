# TTAudit P2 Summary

- Seed: 0
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/datasets/best_model.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/p2_seed0

| System | score_ent AUC | score_conf AUC | score_ce AUC | Top-1 |
|---|---:|---:|---:|---:|
| Source | 0.5023 | 0.5029 | 0.4976 | 11.70 |
| BufTTA | 0.5104 | 0.5138 | 0.5003 | 40.80 |

- Buffer size cap: 500
- Buffer threshold: 0.7
- Replay every: 16
- Replay batch size: 64
- Final buffer length: 500
