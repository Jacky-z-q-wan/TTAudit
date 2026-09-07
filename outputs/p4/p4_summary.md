# TTAudit P4 Summary

- Seeds: [0, 1, 2]
- Output root: /home/chenyaofo/wanzhangqi/GuessWork2/outputs

| Config | tail AUC | mid AUC | head AUC | tail−head | Top-1 |
|---|---:|---:|---:|---:|---:|
| Tent 10遍 BN+affine | 0.6380±0.0049 | 0.5844±0.0245 | 0.5844±0.0064 | 0.0536±0.0079 | 49.83±0.17 |
| Tent 1遍 BN+affine | 0.5331±0.0548 | 0.5027±0.0217 | 0.5021±0.0030 | 0.0311±0.0521 | 38.20±0.42 |
| Tent 10遍 BN-only | 0.5135±0.0603 | 0.4928±0.0115 | 0.4967±0.0068 | 0.0168±0.0565 | 36.53±0.17 |

Optional case D not run by default.
