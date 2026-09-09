# A2 Pilot Summary: round_sweep

- Scenario: `{'tag': 'round_sweep', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'label_skew', 'client_count': 4, 'rounds': 10, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `label_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `10`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 1.02
- NoRaw Top-1: 1.02
- Raw Top-1: 15.37

## Message Leakage
- NoRaw payload-only member AUC: 0.4759
- NoRaw full-transcript member AUC: 0.4693
- NoRaw payload-only source-id acc: 0.6458
- NoRaw full-transcript source-id acc: 0.7708
- NoRaw payload-only class presence acc: 0.6406
- NoRaw full-transcript class presence acc: 0.6642
