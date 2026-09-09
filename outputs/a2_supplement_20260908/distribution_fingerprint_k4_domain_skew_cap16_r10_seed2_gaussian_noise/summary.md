# A2 Supplement Summary: distribution_fingerprint

- Scenario: `{'tag': 'distribution_fingerprint', 'seed': 2, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'domain_skew', 'client_count': 4, 'rounds': 10, 'class_cap': 16, 'carrier': 'prototype', 'n_c': 16}`
- Split type: `domain_skew`
- Carrier: `prototype`
- Clients: `4`
- Rounds: `10`
- class_cap: `16`
- Corruption: `gaussian_noise`

## Utility
- Source Top-1: 15.10
- Local-EATA Top-1: 13.75
- NoRaw-silent Top-1: 13.75
- NoRaw Top-1: 16.40
- Raw oracle Top-1: 45.25

## Leakage
- NoRaw payload-only member AUC*: 0.5030
- NoRaw payload-only source-ID acc: 0.7625
- NoRaw payload-only class presence BA: 0.5135
- NoRaw full-transcript member AUC*: 0.5030
