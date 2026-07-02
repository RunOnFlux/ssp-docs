---
icon: arrow-right-arrow-left
---

# Migrating to SSP Enterprise

Switching from another custody platform to SSP Enterprise is a deliberate process — your funds need to move on-chain, your team needs new device setups, and your operational habits need to adapt. This section covers the major platforms teams migrate from and what to expect at each step.

## Why teams migrate

Common reasons we hear:

* **Cost** — SSP Enterprise has a free tier and is dramatically cheaper than enterprise custody platforms (often $50K–$500K/year savings)
* **True self-custody** — eliminate counterparty risk, vendor key shares, MPC trust assumptions
* **No vendor lock-in** — your funds are recoverable from your signers' seed phrases regardless of SSP's status
* **Multi-chain in one place** — manage Bitcoin, Ethereum, EVM L2s, and more from one platform
* **Open source** — verify the security claims yourself instead of taking a vendor's word

## Migration guides

* **[From Fireblocks](from-fireblocks.md)** — moving off MPC custody to true self-custody multisig
* **[From Safe (Gnosis Safe)](from-safe.md)** — adding Bitcoin/UTXO chains, two-device security, and policies
* **[From BitGo](from-bitgo.md)** — moving off 2-of-3-with-vendor to M-of-N self-custody

## Universal pre-migration checklist

Regardless of which platform you're leaving:

1. **Inventory your assets** — list every chain, address, and balance you currently hold
2. **Decide your new vault structure** — typically one vault per chain, possibly multiple per chain (operations vs reserve)
3. **Identify your signers** — who will be on the new vaults? Plan for resilience: at least one extra signer beyond your threshold
4. **Set up SSP Wallet + SSP Key for every signer** before migration day
5. **Test on testnets first** — Bitcoin Signet and Sepolia are free; run through the full proposal-and-sign flow before touching real funds
6. **Communicate with stakeholders** — exchanges, DEXes, off-ramps that have your old addresses on whitelists need to be updated

## Common pitfalls

* **Underestimating signer availability** — N-of-N setups freeze funds if any signer is unavailable. Use majority thresholds (e.g., 3-of-5) for any high-value vault.
* **Forgetting whitelisted addresses** — counterparties (custodians, exchanges, OTC desks) often have your old addresses whitelisted. Coordinate the address change *before* migrating.
* **Migrating everything at once** — move one vault, validate it works for a few days, then continue. Don't make migration day also your first day on the new platform.
* **Not archiving the old platform immediately** — once funds are out, lock down the old platform's admin access to prevent confusion.

## How long it takes

| Team size | Typical timeline |
|---|---|
| Small (2–5 signers, single chain) | 1–2 days |
| Mid (5–10 signers, 2–4 chains) | 1 week |
| Large (10+ signers, full multi-chain treasury) | 2–4 weeks (phased) |

The bottleneck is almost always coordinating signer availability for vault setup and the first batch of test transactions, not the technical migration itself.

## Need help?

For larger or more complex migrations, we offer hands-on migration support. Email **[tadeas@sspwallet.com](mailto:tadeas@sspwallet.com)** or [book a call](https://calendar.app.google/NZd7n1d6Hjmd7XFD6).
