# A2 Pilot Summary: main_nc

- Scenario: `{'tag': 'main_nc', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 1.05
- NoRaw Top-1: 1.05
- Raw Top-1: 15.36

## Message Leakage
- NoRaw payload-only member AUC: 0.5351
- NoRaw full-transcript member AUC: 0.5486
- NoRaw payload-only source-id acc: 0.4000
- NoRaw full-transcript source-id acc: 0.2000
- NoRaw payload-only class presence acc: 0.0000
- NoRaw full-transcript class presence acc: 0.0000
