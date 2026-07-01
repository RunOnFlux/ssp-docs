---
icon: building-shield
---

# Flux Foundation

How the Flux Foundation moved from scattered multisig and custom scripts to a single self-custody platform — securing the Fusion bridge and Foundation treasury without giving up keys to anyone.

> "We needed full control of our funds, transparency for the community, and no black box MPC sitting between us and our own treasury. SSP Enterprise was the first platform we found that matches all three — across every chain we actually use."
>
> — Tadeas Kmenta, Flux Foundation

## At a glance

| | |
|---|---|
| **Organization** | Flux Foundation |
| **Industry** | Decentralized infrastructure |
| **Use case** | Foundation treasury + Fusion bridge multisig |
| **Chains** | BTC, ETH, FLUX, EVM L2s |
| **Vault model** | M-of-N multisig, two-device per signer |
| **Status** | In production |

## Before: scattered multisig and custom scripts

The Flux Foundation had been managing multi-party signing for years. The problem wasn't a lack of multisig — it was that nothing tied it together across chains, and the commercial alternatives all required a tradeoff the team wasn't willing to make.

* **Scattered multisig setups.** Different multisig wallets across different chains, each with its own quirks, signers, and recovery procedures.
* **Custom scripts to glue it together.** Internal tooling for transaction construction, signing coordination, and broadcast — maintained by hand.
* **No unified audit trail.** Approvals lived in chat threads and shared docs. Reconstructing who signed what required digging.
* **No middle ground in the market.** Custodians and MPC vendors meant trusting a black box. Off-the-shelf multisig tools were Ethereum-only.

## The decision

The team evaluated the alternatives and grouped them into three buckets. Every bucket had a dealbreaker.

**Bucket 1 — MPC custodians.** Fireblocks, BitGo, and similar. The vendor holds part of every key. Not true self-custody, regardless of how it's marketed.

**Bucket 2 — Single-chain multisig.** Safe and the like. Ethereum-only, no native UTXO support. The Foundation holds Bitcoin and Flux too — a treasury solution that ignores half the assets is not a treasury solution.

**Bucket 3 — Bare-bones multisig wallets.** Open-source multisig tools without a policy engine, analytics, or proper audit trail. Choosing one of these would just put the team back to building the management layer themselves — which was the original problem.

SSP Enterprise was the only platform that addressed all three at once: native multi-chain (UTXO and EVM), true self-custody with no vendor key share, and a built-in policy engine, analytics, and audit trail.

## What changed

One platform, every chain the Foundation interacts with, one consistent signing model. The custom scripts are retired. The chat-thread audit trail is replaced by attributable on-platform records. Treasury and bridge multisig live side by side under the same governance.

* **M-of-N vaults across every chain.** Bitcoin, Ethereum, Flux, and the EVM L2s — same platform, same mental model, same security guarantees.
* **Two devices per signer (default Dual mode).** With Dual Signing, the browser extension and mobile app are both required for every signature, so a single compromised device cannot sign. (Optional Key Only / Wallet Only vault modes use one device per signer — see [Creating Multisig Vaults](../creating-vaults.md).)
* **Audit trail that holds up.** Every proposal, approval, and rejection logged with the signer and timestamp. Replaces the chat-thread reconstruction.
* **Self-custody, end to end.** No vendor key share. No MPC infrastructure to trust. If SSP went away tomorrow, funds remain recoverable from signer seeds.

## The Fusion bridge specifically

The Fusion bridge is one of the most security-sensitive surfaces the Foundation operates — it coordinates value transfer between Flux and other chains, and a single signing failure could be catastrophic. It's also the workload where SSP Enterprise's model pays off most clearly.

* **On-chain multisig.** Native multisig on every chain the bridge touches — no smart-contract custodian, no relayer that can stall or front-run.
* **Two-device signing per signer.** Compromising one signer's laptop or phone is not enough to push a bridge transaction. Both devices, every time.
* **Per-signer policy controls.** Limits and whitelists configured at the org and vault level, enforced before any signature is collected.
* **Auditable end to end.** Every approval and rejection attributed and timestamped. Community accountability without giving up self-custody.

> "The Fusion bridge runs on infrastructure we control end to end now. Same security guarantees we have for our own treasury, applied to one of the highest-stakes workflows we operate."
>
> — Tadeas Kmenta, Flux Foundation

## Want the same setup?

The platform the Flux Foundation runs the Fusion bridge on is the same platform available to any team that wants self-custody multisig without compromise.

* Start with [Getting Started](../getting-started.md) → [Creating Your First Organization](../creating-organization.md) → [Creating Multisig Vaults](../creating-vaults.md)
* For DAOs and foundations specifically, see [DAO Treasury Management](../use-cases/dao-treasury.md)
* For migrating off another platform, see [Migration Guides](../migration/README.md)
* Email [tadeas@sspwallet.com](mailto:tadeas@sspwallet.com) or [book a call](https://calendar.app.google/NZd7n1d6Hjmd7XFD6) to discuss your setup
