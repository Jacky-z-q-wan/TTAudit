# A2 Pilot Summary: split_compare

- Scenario: `{'tag': 'split_compare', 'seed': 2, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'domain_skew', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `domain_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 3.24
- NoRaw Top-1: 3.34
- Raw Top-1: 11.62

## Message Leakage
- NoRaw payload-only member AUC: 0.4771
- NoRaw full-transcript member AUC: 0.4207
- NoRaw payload-only source-id acc: 0.6000
- NoRaw full-transcript source-id acc: 0.6000
- NoRaw payload-only class presence acc: 0.0000
- NoRaw full-transcript class presence acc: 0.0000
