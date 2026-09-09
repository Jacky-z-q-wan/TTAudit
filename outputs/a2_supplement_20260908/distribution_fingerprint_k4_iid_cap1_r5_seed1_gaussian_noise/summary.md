# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 5, 'class_cap': 1, 'carrier': 'prototype', 'n_c': 1}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `5`
- class_cap: `1`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 9.70
- Local-EATA Top-1: 15.20
- NoRaw-silent Top-1: 15.20
- NoRaw Top-1: 15.60
- Raw oracle Top-1: 50.15

## Leakage
- NoRaw payload-only member AUC*: 0.5048
- NoRaw payload-only source-ID acc: 0.5750
- NoRaw payload-only class presence BA: 0.5158
- NoRaw full-transcript member AUC*: 0.5048
