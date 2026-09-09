# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'domain_skew', 'client_count': 4, 'rounds': 10, 'class_cap': 1, 'carrier': 'prototype', 'n_c': 1}`
- Split type: `domain_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `10`
- class_cap: `1`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 15.05
- Local-EATA Top-1: 18.50
- NoRaw-silent Top-1: 18.50
- NoRaw Top-1: 11.20
- Raw oracle Top-1: 44.25

## Leakage
- NoRaw payload-only member AUC*: 0.5054
- NoRaw payload-only source-ID acc: 0.7750
- NoRaw payload-only class presence BA: 0.5039
- NoRaw full-transcript member AUC*: 0.5054
