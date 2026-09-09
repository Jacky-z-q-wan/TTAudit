# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 1, 'class_cap': 16, 'carrier': 'prototype', 'n_c': 16}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- class_cap: `16`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 9.70
- Local-EATA Top-1: 13.10
- NoRaw-silent Top-1: 13.10
- NoRaw Top-1: 13.05
- Raw oracle Top-1: 50.15

## Leakage
- NoRaw payload-only member AUC*: 0.5047
- NoRaw payload-only source-ID acc: 0.5000
- NoRaw payload-only class presence BA: 0.5074
- NoRaw full-transcript member AUC*: 0.5047
