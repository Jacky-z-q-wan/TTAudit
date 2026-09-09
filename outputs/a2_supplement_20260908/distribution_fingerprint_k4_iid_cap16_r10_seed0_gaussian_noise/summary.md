# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 10, 'class_cap': 16, 'carrier': 'prototype', 'n_c': 16}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `10`
- class_cap: `16`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 11.85
- Local-EATA Top-1: 13.15
- NoRaw-silent Top-1: 13.15
- NoRaw Top-1: 19.10
- Raw oracle Top-1: 52.55

## Leakage
- NoRaw payload-only member AUC*: 0.5031
- NoRaw payload-only source-ID acc: 0.6250
- NoRaw payload-only class presence BA: 0.5078
- NoRaw full-transcript member AUC*: 0.5031
