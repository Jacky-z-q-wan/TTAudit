# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 1, 'class_cap': 1, 'carrier': 'prototype', 'n_c': 1}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- class_cap: `1`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 11.85
- Local-EATA Top-1: 16.00
- NoRaw-silent Top-1: 16.00
- NoRaw Top-1: 16.10
- Raw oracle Top-1: 52.55

## Leakage
- NoRaw payload-only member AUC*: 0.5006
- NoRaw payload-only source-ID acc: 0.2500
- NoRaw payload-only class presence BA: 0.4909
- NoRaw full-transcript member AUC*: 0.5006
