# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'domain_skew', 'client_count': 4, 'rounds': 10, 'class_cap': 16, 'carrier': 'prototype', 'n_c': 16}`
- Split type: `domain_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `10`
- class_cap: `16`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 16.50
- Local-EATA Top-1: 23.35
- NoRaw-silent Top-1: 23.35
- NoRaw Top-1: 21.80
- Raw oracle Top-1: 47.10

## Leakage
- NoRaw payload-only member AUC*: 0.5065
- NoRaw payload-only source-ID acc: 0.8250
- NoRaw payload-only class presence BA: 0.5088
- NoRaw full-transcript member AUC*: 0.5065
