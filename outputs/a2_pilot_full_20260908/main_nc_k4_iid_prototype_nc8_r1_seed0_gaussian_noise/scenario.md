# A2 Pilot Summary: main_nc

- Scenario: `{'tag': 'main_nc', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 1, 'n_c': 8, 'carrier': 'prototype'}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `8`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 1.24
- NoRaw Top-1: 1.24
- Raw Top-1: 15.48

## Message Leakage
- NoRaw payload-only member AUC: 0.4187
- NoRaw full-transcript member AUC: 0.4879
- NoRaw payload-only source-id acc: 0.0000
- NoRaw full-transcript source-id acc: 0.2000
- NoRaw payload-only class presence acc: 0.0000
- NoRaw full-transcript class presence acc: 0.0000
