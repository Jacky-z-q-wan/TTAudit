# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'domain_skew', 'client_count': 4, 'rounds': 5, 'class_cap': 4, 'carrier': 'prototype', 'n_c': 4}`
- Split type: `domain_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `5`
- class_cap: `4`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 16.50
- Local-EATA Top-1: 27.20
- NoRaw-silent Top-1: 27.20
- NoRaw Top-1: 24.50
- Raw oracle Top-1: 47.10

## Leakage
- NoRaw payload-only member AUC*: 0.5077
- NoRaw payload-only source-ID acc: 0.7750
- NoRaw payload-only class presence BA: 0.5141
- NoRaw full-transcript member AUC*: 0.5077
