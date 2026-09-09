# A2 Pilot Summary: extra_corruption

- Scenario: `{'tag': 'extra_corruption', 'seed': 2, 'corruption': 'impulse_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `impulse_noise`

## Utility
- Local-only Top-1: 1.15
- NoRaw Top-1: 1.15
- Raw Top-1: 12.39

## Message Leakage
- NoRaw payload-only member AUC: 0.5816
- NoRaw full-transcript member AUC: 0.6039
- NoRaw payload-only source-id acc: 0.4000
- NoRaw full-transcript source-id acc: 0.6000
- NoRaw payload-only class presence acc: 0.0000
- NoRaw full-transcript class presence acc: 0.0000
