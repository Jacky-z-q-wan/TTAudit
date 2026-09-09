# A2 Pilot Suite Summary

- Seed: 0
- Scenario count: 63
- Checkpoint: /home/chenyaofo/wanzhangqi/GuessWork2/datasets/best_model.pth
- Data root: /home/chenyaofo/mr/datasets/corruption/CIFAR-100-C
- Output dir: /home/chenyaofo/wanzhangqi/GuessWork2/outputs/a2_pilot_full_20260908

## Scenario groups
- main_nc|iid|4|1|1|prototype|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4311
- main_nc|iid|4|1|2|prototype|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4155
- main_nc|iid|4|1|4|prototype|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4867
- main_nc|iid|4|1|8|prototype|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4299
- main_nc|iid|4|1|16|prototype|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4250
- main_nc|iid|4|1|32|prototype|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4667
- round_sweep|label_skew|4|1|4|prototype|gaussian_noise: local Top-1 mean=0.95, NoRaw Top-1 mean=0.95, Raw Top-1 mean=15.04, member AUC mean=0.4500
- round_sweep|label_skew|4|5|4|prototype|gaussian_noise: local Top-1 mean=0.95, NoRaw Top-1 mean=0.95, Raw Top-1 mean=15.04, member AUC mean=0.4659
- round_sweep|label_skew|4|10|4|prototype|gaussian_noise: local Top-1 mean=0.96, NoRaw Top-1 mean=0.96, Raw Top-1 mean=15.04, member AUC mean=0.4837
- split_compare|iid|4|1|4|prototype|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4867
- split_compare|label_skew|4|1|4|prototype|gaussian_noise: local Top-1 mean=0.95, NoRaw Top-1 mean=0.95, Raw Top-1 mean=15.04, member AUC mean=0.4500
- split_compare|domain_skew|4|1|4|prototype|gaussian_noise: local Top-1 mean=16.88, NoRaw Top-1 mean=17.23, Raw Top-1 mean=18.38, member AUC mean=0.4557
- split_compare|tail|4|1|4|prototype|gaussian_noise: local Top-1 mean=0.88, NoRaw Top-1 mean=0.88, Raw Top-1 mean=15.37, member AUC mean=0.4521
- k_compare|iid|4|1|4|prototype|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4867
- k_compare|iid|8|1|4|prototype|gaussian_noise: local Top-1 mean=1.19, NoRaw Top-1 mean=1.19, Raw Top-1 mean=15.43, member AUC mean=0.4706
- extra_corruption|iid|4|1|4|prototype|impulse_noise: local Top-1 mean=1.05, NoRaw Top-1 mean=1.05, Raw Top-1 mean=12.60, member AUC mean=0.4574
- carrier_compare|iid|4|1|4|prototype|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4867
- carrier_compare|iid|4|1|4|bn_stats|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4044
- carrier_compare|iid|4|1|4|domain_vector|gaussian_noise: local Top-1 mean=1.15, NoRaw Top-1 mean=1.15, Raw Top-1 mean=15.39, member AUC mean=0.4872
- carrier_compare|iid|4|1|4|output_distribution|gaussian_noise: local Top-1 mean=1.21, NoRaw Top-1 mean=1.21, Raw Top-1 mean=15.39, member AUC mean=0.4749
- carrier_compare|iid|4|1|4|embedding|gaussian_noise: local Top-1 mean=1.15, NoRaw Top-1 mean=1.15, Raw Top-1 mean=15.39, member AUC mean=0.4872
