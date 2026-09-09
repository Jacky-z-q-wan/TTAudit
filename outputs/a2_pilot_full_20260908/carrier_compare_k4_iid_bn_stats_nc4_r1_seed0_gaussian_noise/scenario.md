# A2 Pilot Summary: carrier_compare

- Scenario: `{'tag': 'carrier_compare', 'seed': 0, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'bn_stats'}`
- Split type: `iid`
- Carrier: `bn_stats`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 1.24
- NoRaw Top-1: 1.24
- Raw Top-1: 15.48

## Message Leakage
- NoRaw payload-only member AUC: 0.3586
- NoRaw full-transcript member AUC: 0.3765
- NoRaw payload-only source-id acc: 0.4000
- NoRaw full-transcript source-id acc: 0.2000
- NoRaw payload-only class presence acc: 0.0000
- NoRaw full-transcript class presence acc: 0.0000
