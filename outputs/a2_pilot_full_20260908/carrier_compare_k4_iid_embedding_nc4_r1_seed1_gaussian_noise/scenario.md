# A2 Pilot Summary: carrier_compare

- Scenario: `{'tag': 'carrier_compare', 'seed': 1, 'corruption': 'gaussian_noise', 'severity': 5, 'split_type': 'iid', 'client_count': 4, 'rounds': 1, 'n_c': 4, 'carrier': 'embedding'}`
- Split type: `iid`
- Carrier: `embedding`
- Clients: `4`
- Rounds: `1`
- n_c: `4`
- Corruption: `gaussian_noise`

## Utility
- Local-only Top-1: 0.95
- NoRaw Top-1: 0.95
- Raw Top-1: 15.36

## Message Leakage
- NoRaw payload-only member AUC: 0.4656
- NoRaw full-transcript member AUC: 0.5432
- NoRaw payload-only source-id acc: 0.2000
- NoRaw full-transcript source-id acc: 0.2000
- NoRaw payload-only class presence acc: 0.0000
- NoRaw full-transcript class presence acc: 0.0000
