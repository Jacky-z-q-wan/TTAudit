# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 2, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'label_skew', 'client_count': 4, 'rounds': 1, 'class_cap': 4, 'carrier': 'prototype', 'n_c': 4}`
- Split type: `label_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- class_cap: `4`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 10.90
- Local-EATA Top-1: 14.70
- NoRaw-silent Top-1: 14.70
- NoRaw Top-1: 14.70
- Raw oracle Top-1: 51.65

## Leakage
- NoRaw payload-only member AUC*: 0.5051
- NoRaw payload-only source-ID acc: 0.5000
- NoRaw payload-only class presence BA: 0.6250
- NoRaw full-transcript member AUC*: 0.5051
