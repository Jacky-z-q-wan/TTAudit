# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'label_skew', 'client_count': 4, 'rounds': 5, 'class_cap': 4, 'carrier': 'prototype', 'n_c': 4}`
- Split type: `label_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `5`
- class_cap: `4`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 9.35
- Local-EATA Top-1: 12.65
- NoRaw-silent Top-1: 12.65
- NoRaw Top-1: 10.85
- Raw oracle Top-1: 50.35

## Leakage
- NoRaw payload-only member AUC*: 0.5163
- NoRaw payload-only source-ID acc: 0.6500
- NoRaw payload-only class presence BA: 0.7224
- NoRaw full-transcript member AUC*: 0.5163
