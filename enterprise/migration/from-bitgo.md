---
icon: right-from-bracket
---

# Migrating from BitGo to SSP Enterprise

This guide is for teams currently using BitGo (typically the BitGo Custody or BitGo Wallet platform) who want to move to SSP Enterprise. The biggest shift is moving from BitGo's **2-of-3 model where one key sits with BitGo** to a fully self-custodial M-of-N where your team holds **all** keys.

## Why teams move from BitGo

* **True self-custody** — BitGo's standard model is 2-of-3 multisig where BitGo holds one key (the "BitGo key") to enable recovery. This means BitGo can theoretically participate in moves even though they typically don't. SSP Enterprise has no vendor key — your signers hold every key.
* **Cost** — BitGo's enterprise pricing typically starts around $50K/year and scales. SSP Enterprise has a free tier and lower paid tiers.
* **No vendor lock-in** — if BitGo went away, you'd need their cooperation to recover funds (you have your two keys, but the BitGo key is theirs). With SSP Enterprise, your funds are recoverable using only your signers' seed phrases — no vendor cooperation needed.
* **Multi-chain consistency** — BitGo's coverage is broad but uneven; some chains use full multisig, others use MPC, others are custodial. SSP Enterprise uses the same two-device M-of-N model on every supported chain.
* **Open source** — verify the security yourself.

## What you keep, what changes

| Aspect | BitGo | SSP Enterprise |
|---|---|---|
| **Custody model** | 2-of-3 multisig with BitGo holding 1 key (or fully custodial on some products) | M-of-N multisig, your team holds all keys |
| **Vendor key share** | Yes (BitGo holds the third key) | None |
| **Recovery without vendor** | Difficult (need BitGo cooperation in many scenarios) | Always possible from signer seed phrases |
| **Compliance** | Built-in (BitGo-mediated) | Self-managed |
| **Insurance** | Available via BitGo Trust | Bring your own |
| **Cost** | $50K+/year | Free tier + paid plans |

> **BitGo Trust note:** if you're using BitGo Trust (the qualified custodian product), the move to SSP Enterprise means moving from regulated qualified custody to self-custody. This is a regulatory and operational change, not just a technical one. Make sure your compliance team and any LPs/investors understand the implications.

## Pre-migration checklist

* [ ] List all BitGo wallets, their coins, balances, addresses, and approver structure
* [ ] Note for each wallet: is it BitGo Express (multisig with BitGo as 3rd key), BitGo Custody (qualified custody), or another product?
* [ ] Identify your new SSP Enterprise signers — typically your existing BitGo approvers
* [ ] Each signer installs **SSP Wallet** + **SSP Key** and completes [first-time setup](../../quick-start/first-time-setup.md)
* [ ] Each signer logs in to SSP Enterprise once
* [ ] List all on-chain and off-chain integrations: exchanges with whitelisted BitGo addresses, internal accounting systems, partner relationships
* [ ] **Critical:** if you're under regulatory custody requirements, confirm that self-custody is permitted for your entity type and jurisdiction *before* migrating

## Step 1 — Regulatory and policy review

Unique to BitGo migrations: you may have non-technical blockers.

* **For investment funds and trusts** — qualified custody requirements may legally require a third-party custodian. Self-custody may not be permitted depending on your fund structure and jurisdiction. Consult legal before migrating.
* **For corporates** — internal treasury policy may require third-party custody. Update the policy or confirm self-custody is allowed.
* **For insurers** — BitGo's coverage doesn't follow your funds. If you need insurance, source a self-custody-compatible policy.

If any of these are blockers, talk to us before proceeding — there are sometimes hybrid arrangements that work.

## Step 2 — Set up SSP Enterprise

1. Create your organization — see [Creating Your First Organization](../creating-organization.md)
2. Invite all signers — see [Inviting Team Members](../inviting-members.md)
3. Confirm all signers are in **Members**

## Step 3 — Decide your new threshold

BitGo's 2-of-3 (with BitGo as the 3rd key) was effectively 1-of-2 from your team's perspective on the day-to-day, with BitGo as a backstop. Your new SSP threshold is now entirely your team's responsibility.

Common new thresholds when migrating from BitGo:

* **2-of-3 with three of your signers** — same number of signatures, no vendor
* **3-of-5** — adds resilience; tolerates one signer being unavailable
* **2-of-4** — easier daily ops, still requires team consensus

For high-value vaults, prefer at least 3-of-5. Don't try to replicate BitGo's "team plus vendor backstop" with N-of-N — that creates a single point of failure (any unavailable signer freezes the vault).

## Step 4 — Create vaults

Use the [Creating Multisig Vaults](../creating-vaults.md) walkthrough.

For each BitGo wallet, create a corresponding SSP vault on the same chain.

## Step 5 — Configure policies

BitGo offers Webhooks and Policy enforcement at the wallet level. Translate to SSP Enterprise:

| BitGo policy | SSP Enterprise equivalent |
|---|---|
| Velocity limits | **Daily / Weekly / Monthly Spending Limits** |
| Spending limits per user | **Per-Signer Spending Limits** |
| Whitelist (Address Verification) | **Address Whitelist: Strict Mode** |
| Approval thresholds | **Admin Approval** (USD threshold) |
| Manual review queue | **Time-Lock Delay** + **Warning Mode whitelist** |

See [Configuring Policy Controls](../policies.md).

## Step 6 — Test on testnet

* Create a Bitcoin Signet vault and an Ethereum Sepolia vault with the same signer and threshold structure
* Send testnet funds, run through the proposal-and-sign flow
* Trigger each policy you've configured to confirm it works as expected

## Step 7 — Move funds in tranches

* **Tranche 1:** 10%, leave overnight
* **Tranche 2:** 40%, verify
* **Tranche 3:** 50%, complete

For BitGo specifically: if you're using BitGo's hot/warm/cold tier separation, migrate the **hot wallets first** (lowest balance, highest churn) so you can validate the operational flow with the smallest blast radius. Migrate cold storage last.

## Step 8 — Update counterparties and integrations

* Exchanges, OTC desks, and DeFi platforms with whitelisted BitGo addresses need your new addresses
* Accounting systems (Cryptio, Bitwave, etc.) that pull from BitGo's API need to be repointed to SSP Enterprise's API or re-configured
* Internal alerting tools that watched BitGo addresses need updates

## Step 9 — Decommission BitGo

* **Don't close the account immediately** — keep it for at least 60 days for audit trail and any straggling reconciliation
* **Export all transaction history** for your records (BitGo may impose limits on historical access for closed accounts)
* **Cancel API keys** that interacted with BitGo
* **Cancel the subscription** at your renewal date
* **For BitGo Trust accounts:** notify them in writing per your service agreement

## Common gotchas

* **BitGo's "user keys" are encrypted with a passphrase** — make sure you have both the encrypted backup AND the passphrase before migration. Don't lose access to old funds during transition.
* **Address types** — BitGo uses specific address derivation paths. Your new SSP addresses will be different — never assume "the same wallet's new address." Always verify the SSP address from the SSP Enterprise UI directly.
* **BitGo's API is integrated into many treasury tools** — Cryptio, TaxBit, Bitwave, etc. all support BitGo natively. If you rely on these, check their SSP Enterprise integration status before migrating (and consider keeping BitGo open in read-only mode if needed for historical reporting).
* **Insurance carry-over** — BitGo Trust insurance does not transfer. Source replacement coverage if needed *before* moving funds.

## Estimated timeline

| Team profile | Typical duration |
|---|---|
| Small team, 1–2 chains | 1–2 weeks |
| Mid-size, multi-chain | 3–4 weeks |
| Investment fund (with regulatory review) | 2–3 months |

The regulatory review step is often the longest part for funds and institutional clients.

## Need help?

BitGo migrations often involve regulatory and operational considerations beyond the technical migration. For hands-on support — including legal/compliance discussions and treasury team training — email **[tadeas@sspwallet.com](mailto:tadeas@sspwallet.com)** or [book a call](https://calendar.app.google/NZd7n1d6Hjmd7XFD6).
