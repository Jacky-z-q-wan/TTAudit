# A2 Pilot Summary: k_compare

- Scenario: `{'tag': 'k_compare', 'seed': 2, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 1.34
- NoRaw Top-1: 1.34
- Raw Top-1: 15.34

## Message Leakage
- NoRaw payload-only member AUC: 0.4231
- NoRaw full-transcript member AUC: 0.4433
- NoRaw payload-only source-id acc: 0.4000
- NoRaw full-transcript source-id acc: 0.4000
- NoRaw payload-only class presence acc: 0.0000
- NoRaw full-transcript class presence acc: 0.0000
