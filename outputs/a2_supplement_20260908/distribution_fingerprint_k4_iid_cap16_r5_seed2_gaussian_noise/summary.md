# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 2, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 5, 'class_cap': 16, 'carrier': 'prototype', 'n_c': 16}`
- Split type: `iid`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `5`
- class_cap: `16`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 9.55
- Local-EATA Top-1: 13.85
- NoRaw-silent Top-1: 13.85
- NoRaw Top-1: 13.80
- Raw oracle Top-1: 50.70

## Leakage
- NoRaw payload-only member AUC*: 0.5066
- NoRaw payload-only source-ID acc: 0.5250
- NoRaw payload-only class presence BA: 0.5293
- NoRaw full-transcript member AUC*: 0.5066
