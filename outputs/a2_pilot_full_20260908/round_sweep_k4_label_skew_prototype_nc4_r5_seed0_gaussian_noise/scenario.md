# A2 Pilot Summary: round_sweep

- Scenario: `{'tag': 'round_sweep', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'label_skew', 'client_count': 4, 'rounds': 5, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `label_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `5`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 0.88
- NoRaw Top-1: 0.88
- Raw Top-1: 15.35

## Message Leakage
- NoRaw payload-only member AUC: 0.4584
- NoRaw full-transcript member AUC: 0.5079
- NoRaw payload-only source-id acc: 0.6667
- NoRaw full-transcript source-id acc: 0.6667
- NoRaw payload-only class presence acc: 0.6508
- NoRaw full-transcript class presence acc: 0.7088
