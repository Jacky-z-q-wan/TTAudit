# A2 Pilot Summary: split_compare

- Scenario: `{'tag': 'split_compare', 'seed': 2, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'tail', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `tail`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 0.92
- NoRaw Top-1: 0.92
- Raw Top-1: 14.53

## Message Leakage
- NoRaw payload-only member AUC: 0.4372
- NoRaw full-transcript member AUC: 0.4683
- NoRaw payload-only source-id acc: 0.4000
- NoRaw full-transcript source-id acc: 0.4000
- NoRaw payload-only class presence acc: 0.5682
- NoRaw full-transcript class presence acc: 0.6205
