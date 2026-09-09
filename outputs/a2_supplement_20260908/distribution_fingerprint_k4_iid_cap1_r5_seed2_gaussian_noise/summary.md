# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 2, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 5, 'class_cap': 1, 'carrier': 'prototype', 'n_c': 1}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `5`
- class_cap: `1`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 9.55
- Local-EATA Top-1: 12.95
- NoRaw-silent Top-1: 12.95
- NoRaw Top-1: 12.00
- Raw oracle Top-1: 50.80

## Leakage
- NoRaw payload-only member AUC*: 0.5064
- NoRaw payload-only source-ID acc: 0.5000
- NoRaw payload-only class presence BA: 0.5220
- NoRaw full-transcript member AUC*: 0.5064
