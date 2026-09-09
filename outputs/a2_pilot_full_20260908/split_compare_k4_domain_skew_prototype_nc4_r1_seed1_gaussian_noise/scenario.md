# A2 Pilot Summary: split_compare

- Scenario: `{'tag': 'split_compare', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'domain_skew', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `domain_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 17.06
- NoRaw Top-1: 18.06
- Raw Top-1: 8.28

## Message Leakage
- NoRaw payload-only member AUC: 0.4582
- NoRaw full-transcript member AUC: 0.4339
- NoRaw payload-only source-id acc: 0.8000
- NoRaw full-transcript source-id acc: 0.8000
- NoRaw payload-only class presence acc: 0.0000
- NoRaw full-transcript class presence acc: 0.0000
