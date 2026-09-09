# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'domain_skew', 'client_count': 4, 'rounds': 1, 'class_cap': 1, 'carrier': 'prototype', 'n_c': 1}`
- Split type: `domain_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `1`
- class_cap: `1`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 16.50
- Local-EATA Top-1: 23.15
- NoRaw-silent Top-1: 23.15
- NoRaw Top-1: 23.10
- Raw oracle Top-1: 47.10

## Leakage
- NoRaw payload-only member AUC*: 0.5055
- NoRaw payload-only source-ID acc: 0.7500
- NoRaw payload-only class presence BA: 0.4874
- NoRaw full-transcript member AUC*: 0.5055
