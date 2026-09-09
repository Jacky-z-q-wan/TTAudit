# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'label_skew', 'client_count': 4, 'rounds': 1, 'class_cap': 1, 'carrier': 'prototype', 'n_c': 1}`
- Split type: `label_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- class_cap: `1`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 10.30
- Local-EATA Top-1: 12.80
- NoRaw-silent Top-1: 12.80
- NoRaw Top-1: 12.60
- Raw oracle Top-1: 48.30

## Leakage
- NoRaw payload-only member AUC*: 0.5042
- NoRaw payload-only source-ID acc: 0.6250
- NoRaw payload-only class presence BA: 0.8333
- NoRaw full-transcript member AUC*: 0.5042
