# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 2, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'domain_skew', 'client_count': 4, 'rounds': 5, 'class_cap': 4, 'carrier': 'prototype', 'n_c': 4}`
- Split type: `domain_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `5`
- class_cap: `4`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 15.10
- Local-EATA Top-1: 27.00
- NoRaw-silent Top-1: 27.00
- NoRaw Top-1: 27.35
- Raw oracle Top-1: 45.25

## Leakage
- NoRaw payload-only member AUC*: 0.5028
- NoRaw payload-only source-ID acc: 0.7750
- NoRaw payload-only class presence BA: 0.5155
- NoRaw full-transcript member AUC*: 0.5028
