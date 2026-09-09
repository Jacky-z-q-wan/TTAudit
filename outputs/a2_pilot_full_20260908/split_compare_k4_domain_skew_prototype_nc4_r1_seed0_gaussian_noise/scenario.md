# A2 Pilot Summary: split_compare

- Scenario: `{'tag': 'split_compare', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'domain_skew', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `domain_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 30.33
- NoRaw Top-1: 30.31
- Raw Top-1: 35.23

## Message Leakage
- NoRaw payload-only member AUC: 0.4318
- NoRaw full-transcript member AUC: 0.5155
- NoRaw payload-only source-id acc: 0.8000
- NoRaw full-transcript source-id acc: 1.0000
- NoRaw payload-only class presence acc: 0.0000
- NoRaw full-transcript class presence acc: 0.0000
