# A2 Pilot Summary: k_compare

- Scenario: `{'tag': 'k_compare', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 8, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `8`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 1.22
- NoRaw Top-1: 1.22
- Raw Top-1: 15.56

## Message Leakage
- NoRaw payload-only member AUC: 0.4736
- NoRaw full-transcript member AUC: 0.5117
- NoRaw payload-only source-id acc: 0.0000
- NoRaw full-transcript source-id acc: 0.1000
- NoRaw payload-only class presence acc: 0.6617
- NoRaw full-transcript class presence acc: 0.6850
