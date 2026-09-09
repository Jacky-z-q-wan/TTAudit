# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 5, 'class_cap': 16, 'carrier': 'prototype', 'n_c': 16}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `5`
- class_cap: `16`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 11.85
- Local-EATA Top-1: 19.50
- NoRaw-silent Top-1: 19.50
- NoRaw Top-1: 20.85
- Raw oracle Top-1: 52.55

## Leakage
- NoRaw payload-only member AUC*: 0.5030
- NoRaw payload-only source-ID acc: 0.5000
- NoRaw payload-only class presence BA: 0.5008
- NoRaw full-transcript member AUC*: 0.5030
