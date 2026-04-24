---
icon: lightbulb
---

# Use Case Guides

Recommended setups for common organizational structures. Each guide covers signer composition, threshold choice, vault layout, and policy recommendations specific to that use case.

These are starting points, not prescriptions — adapt them to your team's actual governance and risk tolerance.

## Available guides

* **[DAO Treasury Management](dao-treasury.md)** — council-based governance with on-chain transparency
* **[Corporate Treasury Workflow](corporate-treasury.md)** — executive sign-off with finance team operations

## Coming soon

* Investment fund custody — fund manager + compliance + LP signers
* OTC desk operations — high-velocity sign-off with strict counterparty controls
* Mining operation treasury — rewards consolidation, automated payouts, cold reserve

If your use case isn't listed and you'd like guidance, email **[tadeas@sspwallet.com](mailto:tadeas@sspwallet.com)**.

## How to choose your setup

Three questions to answer before configuring anything:

1. **Who are the signers?** — name the actual humans, not roles. Each one needs SSP Wallet + SSP Key set up.
2. **What's the worst-case scenario you need to survive?** — one signer compromised? Two unavailable? Internal fraud attempt? Your threshold and signer count should make the worst case survivable.
3. **What's the expected transaction frequency and value?** — high frequency, low value calls for looser policies and faster thresholds. Low frequency, high value calls for tight policies, time-locks, and admin approval gates.

Common patterns by team size:

| Team size | Typical setup | Vaults |
|---|---|---|
| 2–4 people | 2-of-3 with all signers, simple policy | 1–2 vaults |
| 5–10 people | 3-of-5 council vault + 2-of-3 ops vault | 2–4 vaults |
| 10+ people | Multi-vault with role separation: cold reserve, ops, payroll, partnership | 4+ vaults |

The key insight: **don't put everything in one vault**. Separate vaults isolate failures. If your operations vault gets compromised somehow, your cold reserve is untouched.
