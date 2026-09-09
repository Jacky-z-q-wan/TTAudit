# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 10, 'class_cap': 16, 'carrier': 'prototype', 'n_c': 16}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `10`
- class_cap: `16`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 9.70
- Local-EATA Top-1: 7.50
- NoRaw-silent Top-1: 7.50
- NoRaw Top-1: 7.35
- Raw oracle Top-1: 50.20

## Leakage
- NoRaw payload-only member AUC*: 0.5078
- NoRaw payload-only source-ID acc: 0.7625
- NoRaw payload-only class presence BA: 0.5145
- NoRaw full-transcript member AUC*: 0.5078
