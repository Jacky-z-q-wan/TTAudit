# A2 Pilot Summary: split_compare

- Scenario: `{'tag': 'split_compare', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'tail', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `tail`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 0.79
- NoRaw Top-1: 0.79
- Raw Top-1: 15.80

## Message Leakage
- NoRaw payload-only member AUC: 0.5115
- NoRaw full-transcript member AUC: 0.4841
- NoRaw payload-only source-id acc: 0.4000
- NoRaw full-transcript source-id acc: 0.4000
- NoRaw payload-only class presence acc: 0.6338
- NoRaw full-transcript class presence acc: 0.6130
