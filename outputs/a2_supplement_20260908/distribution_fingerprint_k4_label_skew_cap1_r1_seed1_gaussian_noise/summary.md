# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'label_skew', 'client_count': 4, 'rounds': 1, 'class_cap': 1, 'carrier': 'prototype', 'n_c': 1}`
- Split type: `label_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- class_cap: `1`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 9.35
- Local-EATA Top-1: 14.10
- NoRaw-silent Top-1: 14.10
- NoRaw Top-1: 13.85
- Raw oracle Top-1: 50.35

## Leakage
- NoRaw payload-only member AUC*: 0.5146
- NoRaw payload-only source-ID acc: 0.3750
- NoRaw payload-only class presence BA: 0.7708
- NoRaw full-transcript member AUC*: 0.5146
