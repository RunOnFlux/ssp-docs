---
icon: right-from-bracket
---

# Migrating from Fireblocks to SSP Enterprise

The biggest change isn't the UI. It's the custody model. Fireblocks holds part of every key on its infrastructure (MPC). SSP Enterprise doesn't — your team holds every key on your own devices. Everything else (policies, audit, multi-chain) is just engineering.

This guide covers what to plan for, what to migrate in what order, and what to watch out for.

## Why teams leave Fireblocks

We hear four reasons, in roughly this order:

* **Cost.** Fireblocks pricing typically starts around $100K/year and scales with AUM. SSP Enterprise has a free tier and significantly cheaper paid tiers.
* **Counterparty risk.** Fireblocks holds part of your key material. If they have an outage, get acquired, change terms, or have an internal incident, you have a dependency. Self-custody removes the dependency.
* **Operational friction.** KYC, AML, vendor onboarding, and policy approvals can stack up. SSP Enterprise is a tool you operate yourself — no vendor in your transaction flow.
* **Open source.** You can read the SSP Wallet and SSP Key code. You can verify the cryptography. With Fireblocks, you take it on trust.

## What changes for your team

| Aspect | Fireblocks | SSP Enterprise |
|---|---|---|
| Custody | MPC, vendor holds part of every key | Self-custody, your team holds all keys |
| Signing UX | Mobile approval, often 1-of-1 | M-of-N multisig, two devices per signer |
| Addresses | Provided by vendor | Derived from your signers' xpubs |
| Transaction history | Lives in Fireblocks dashboard | Lives on-chain, indexed by SSP |
| Policies | Transaction Authorization Policy (TAP) | SSP policy engine (UI-configured) |
| Compliance integrations | Built-in (Chainalysis, Elliptic) | Bring your own / API-driven |
| Recovery | Vendor-assisted | Signer seed phrases |

The two big mental shifts: (1) signing requires two devices per signer, not one, and (2) recovery is your problem now, with the upside that nothing can prevent it.

## Before you start

Get this out of the way before migration day:

* Inventory every Fireblocks vault — chains, addresses, balances, pending transactions
* Decide who your new signers will be (often the same humans who were Fireblocks approvers)
* Get every signer set up with **SSP Wallet + SSP Key** and confirm they completed the [first-time setup](../../quick-start/first-time-setup.md)
* Have each signer log in to SSP Enterprise once, so their wallet is linked to your org
* List every external counterparty that has a Fireblocks address whitelisted — exchanges, OTC desks, custodians, dApps. They all need updating.
* Decide your new vault structure (typically one vault per chain, sometimes split by purpose)
* Decide your new thresholds — 2-of-3, 3-of-5, etc.

## The actual migration

### 1. Set up the SSP Enterprise side

Create the org, invite signers, confirm everyone shows up under Members. See [Creating Your First Organization](../creating-organization.md) and [Inviting Team Members](../inviting-members.md).

### 2. Build vaults that mirror your Fireblocks structure

For each Fireblocks vault you're migrating, create the equivalent SSP vault:

* Same chain
* Same or stricter threshold (if Fireblocks was effectively 1-of-N or 2-of-N, consider 3-of-N for added resilience)
* Match the naming so the team's mental model translates

Walk through [Creating Multisig Vaults](../creating-vaults.md) for the wizard.

### 3. Translate your TAPs into SSP policies

Your Fireblocks TAPs become SSP policies, mostly 1:1:

| Fireblocks TAP rule | SSP Enterprise equivalent |
|---|---|
| Daily transaction limit | Daily Spending Limit (USD) |
| Per-transaction max | Per Transaction Limit |
| Whitelisted destinations only | Address Whitelist: Strict Mode |
| Manual approval over $X | Admin Approval (USD threshold) |
| Cooldown / delay before broadcast | Time-Lock Delay |
| Spending limits per user | Per-Signer Spending Limits |

Full reference: [Configuring Policy Controls](../policies.md).

### 4. Test on testnet first

Don't skip this. Spin up a Bitcoin Signet vault and a Sepolia vault with the same signers and threshold. Send testnet funds, run through the proposal-and-sign flow end to end, and trigger each policy you've configured to confirm it actually blocks what it should.

This catches signer device issues, policy misconfiguration, and team coordination problems before any real money is at stake. The cost of testnet validation is hours. The cost of skipping it can be six figures.

### 5. Move funds in tranches

Three batches, not one transaction:

1. **10%.** Verify it arrives. Leave it 24 hours. Confirm the SSP Enterprise UI shows it correctly.
2. **40%.** Same drill.
3. **The rest.**

Each tranche is a chance to catch something — wrong chain, address typo, fee misconfiguration. The first tranche being small means a mistake costs $X, not $10X.

### 6. Update everyone who has your Fireblocks addresses whitelisted

This is usually the longest step. Common list:

* Exchanges (Coinbase, Kraken, Binance, etc.) — often require KYC re-verification
* OTC desks — usually a quick email, sometimes contractual changes
* Custodians — process can take weeks
* DeFi protocols — anywhere you've authorized a smart contract to spend tokens
* Internal counterparties — subsidiaries, partners, vendors

Coordinate this *during* tranches 1 and 2 so it's not blocking tranche 3.

### 7. Decommission Fireblocks

Once funds are out and counterparties updated:

* **Don't immediately cancel.** Keep Fireblocks active (read-only if possible) for at least 30 days for audit and any straggling reconciliation.
* **Revoke API keys** that interacted with Fireblocks (treasury management tools, internal scripts).
* **Archive vaults** in Fireblocks once empty.
* **Cancel the subscription** at your renewal date.

## The gotchas

A few things specific to the Fireblocks → SSP Enterprise transition that have bitten teams:

* **EVM smart accounts.** Fireblocks uses standard EOA + MPC. SSP Enterprise EVM vaults are smart accounts (ERC-4337). The address format is the same but the contract logic differs. Any ERC-20 token approvals you held under your old Fireblocks address need to be reissued from your new SSP vault.
* **Token whitelists.** Some Fireblocks accounts had tokens whitelisted by contract. SSP Enterprise watches tokens per vault — make sure to add every token you held.
* **Pending transactions.** Finish or cancel them before migrating. They don't carry over.
* **Audit reports.** Export your full Fireblocks transaction history before you close the account. You won't be able to pull it later.

## How long it takes

Honestly, the technical migration is fast. The slow parts are coordinating signers and updating counterparties.

* Setup (org, signers, vaults, policies): 1–2 days
* Testnet validation: 2–3 days
* Fund migration in tranches: 1 week
* Counterparty whitelist updates: 1–4 weeks (this is your bottleneck)
* Fireblocks decommission: 30+ days post-migration

A small team migrating one chain can be done in a week. A full multi-chain treasury migration is realistically 4–6 weeks including counterparty coordination.

## Want help?

We've helped teams migrate from Fireblocks. For hands-on support — signer onboarding sessions, migration day support, policy review — email [tadeas@sspwallet.com](mailto:tadeas@sspwallet.com) or [book a call](https://calendar.app.google/NZd7n1d6Hjmd7XFD6).
