# A2 Pilot Summary: split_compare

- Scenario: `{'tag': 'split_compare', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'tail', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'prototype'}`
- Split type: `tail`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 0.93
- NoRaw Top-1: 0.93
- Raw Top-1: 15.77

## Message Leakage
- NoRaw payload-only member AUC: 0.4076
- NoRaw full-transcript member AUC: 0.3428
- NoRaw payload-only source-id acc: 0.8000
- NoRaw full-transcript source-id acc: 0.6000
- NoRaw payload-only class presence acc: 0.6455
- NoRaw full-transcript class presence acc: 0.7068
