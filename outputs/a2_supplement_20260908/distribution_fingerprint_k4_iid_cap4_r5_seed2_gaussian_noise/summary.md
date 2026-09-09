# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 2, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 5, 'class_cap': 4, 'carrier': 'prototype', 'n_c': 4}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `5`
- class_cap: `4`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 9.55
- Local-EATA Top-1: 13.85
- NoRaw-silent Top-1: 13.85
- NoRaw Top-1: 13.20
- Raw oracle Top-1: 50.70

## Leakage
- NoRaw payload-only member AUC*: 0.5069
- NoRaw payload-only source-ID acc: 0.5500
- NoRaw payload-only class presence BA: 0.5170
- NoRaw full-transcript member AUC*: 0.5069
