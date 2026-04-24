---
icon: right-from-bracket
---

# Migrating from Safe (Gnosis Safe) to SSP Enterprise

This guide is for teams currently using Safe (formerly Gnosis Safe) who want to consolidate onto SSP Enterprise. The two platforms share a self-custody multisig philosophy but differ on chain coverage, signer security model, and policy capabilities.

## Why teams move from Safe

* **Multi-chain native** — Safe is EVM-only. SSP Enterprise covers Bitcoin, Litecoin, Dogecoin, Bitcoin Cash, Zcash, Ravencoin, Flux, plus all the EVM chains Safe supports. Treasuries holding both BTC and ETH no longer need two platforms.
* **Stronger per-signer security** — Safe relies on whatever wallet each signer happens to use (MetaMask, Ledger, etc.). SSP Enterprise enforces **two-device security per signer** (browser extension + mobile app), so compromising one device of one signer is not enough to sign.
* **Built-in policy engine** — Safe needs Modules/Guards configured per-vault, often with custom Solidity. SSP Enterprise has spending limits, whitelists, time-locks, admin approvals, per-signer overrides — all configured through the UI, no smart contract deployment.
* **Simpler member management** — Safe stores signers as Ethereum addresses; you have to track who each address belongs to manually. SSP Enterprise has named members, roles, audit logs — true team management.
* **No transaction relayer dependencies** — Safe transactions often depend on relayers (especially on L2s). SSP Enterprise broadcasts directly via your chosen RPC.

## What you keep, what changes

| Aspect | Safe | SSP Enterprise |
|---|---|---|
| **Custody model** | Self-custody multisig (smart contract) | Self-custody multisig (native + smart contract) |
| **Chain coverage** | EVM only (~10 chains) | EVM + UTXO (12+ chains across both) |
| **Signer security** | Whatever wallet each signer uses | Two-device per signer (enforced) |
| **Policies** | Modules/Guards (Solidity) | Built-in policy engine (UI configured) |
| **Member management** | Ethereum addresses | Named members + roles |
| **Transaction history** | On-chain + Safe Transaction Service | On-chain + SSP indexer |
| **Recovery** | Signer seed phrases | Signer seed phrases (same model) |
| **Open source** | Yes | Yes |

## Pre-migration checklist

* [ ] List all your Safe wallets, their chains, balances, signer addresses, and current threshold
* [ ] Confirm which signers (humans) own which Ethereum addresses across all Safes
* [ ] Each signer installs **SSP Wallet** browser extension + **SSP Key** mobile app and completes [first-time setup](../../quick-start/first-time-setup.md)
* [ ] Each signer logs in to SSP Enterprise once so their SSP Wallet is linked to your organization
* [ ] Inventory all on-chain integrations: ERC-20 token approvals, dApp connections, vesting contracts, governance delegations
* [ ] Decide whether you're consolidating multiple Safes into fewer SSP vaults, or maintaining 1:1 mapping

## Step 1 — Set up SSP Enterprise

1. Create your organization — see [Creating Your First Organization](../creating-organization.md)
2. Invite all signers — see [Inviting Team Members](../inviting-members.md)
3. Verify all signers appear in **Members**

## Step 2 — Plan your new vault structure

Common patterns:

* **1:1 mapping** — one SSP vault per existing Safe (simplest, lowest cognitive load)
* **Consolidated by purpose** — fewer vaults, organized by what funds are *for* (treasury, payroll, ops) rather than per-chain
* **Hybrid** — keep your most active Safes 1:1, consolidate the long-tail into a single reserve vault

For each new vault, decide:

* **Chain** — same as the Safe you're migrating from (or pick one chain if consolidating)
* **Signers** — same humans, but they'll use their SSP Wallet identity instead of their MetaMask address
* **Threshold** — match your Safe's threshold or revise (this is a good moment to reconsider)

## Step 3 — Create vaults

Walk through [Creating Multisig Vaults](../creating-vaults.md) for each one.

A note on chain choice: Safe supports Ethereum, Optimism, Arbitrum, Base, Polygon, BNB Chain, Avalanche, Gnosis Chain, and a few others. SSP Enterprise currently covers Ethereum, Polygon, BNB Chain, Avalanche, and Base on the EVM side. If you're using Safe on Optimism, Arbitrum, or Gnosis Chain, those are not yet supported and you'll need to bridge funds to a supported chain.

## Step 4 — Configure policies

This is one of the biggest wins moving to SSP Enterprise. Translate your Safe Modules/Guards into native SSP policies:

| Safe approach | SSP Enterprise equivalent |
|---|---|
| Spending Limit Module (per signer) | **Per-Signer Spending Limits** |
| Allowlist Module / Guard | **Address Whitelist: Strict Mode** |
| Cooldown / delay module | **Time-Lock Delay** |
| Multi-tier approval (Safe with Safe owners) | **Admin Approval** (USD threshold) |
| Manual reviews via off-chain process | **Warning Mode whitelist** |

No Solidity required — configure all of this in the UI. See [Configuring Policy Controls](../policies.md).

## Step 5 — Migrate ERC-20 token approvals

This is critical and often missed.

If your Safe had outstanding token approvals (e.g., approved a DEX router to spend USDC), those approvals are tied to the Safe's contract address. They do **not** transfer to your new SSP vault.

For each token approval that matters:

1. Identify all `approve()` calls on your Safe (use Etherscan's "Token Approvals" tool with your Safe address)
2. After moving funds to the new vault, **re-approve** the same spenders from the new vault
3. **Optionally revoke** old approvals on the Safe (using a tool like revoke.cash) for hygiene

## Step 6 — Test on testnet

Create a **Sepolia testnet vault** with the same signers and threshold. Run through:

* A simple ETH transfer
* An ERC-20 transfer
* A policy-blocked transaction (e.g., propose something over your daily limit and confirm it's rejected)
* A multi-recipient batch transaction

This catches misconfigurations and gives signers practice with the new flow before real money is involved.

## Step 7 — Move funds in tranches

For each Safe → SSP vault migration:

1. **Tranche 1 (10%):** move a small amount, verify arrival, leave overnight
2. **Tranche 2 (40%):** move more, verify
3. **Tranche 3 (50%):** move the remainder

If you have multiple Safes, do them sequentially — finish one before starting the next. This isolates problems.

## Step 8 — Update integrations

* **dApps** — anywhere you've connected your Safe via WalletConnect needs to be reconnected with your new SSP vault address
* **Governance** — if your Safe held governance tokens (UNI, COMP, etc.) and was delegating votes, redelegate from the new vault
* **Off-chain tools** — Safe Transaction Service URLs, custom dashboards, internal tools all need to be repointed
* **Counterparties** — same as Fireblocks: anyone who whitelisted your Safe address needs the new address

## Step 9 — Decommission the Safe

Once funds and approvals are migrated:

1. **Leave the Safe deployed.** Don't try to "delete" it — Safe contracts are immutable. Just empty it.
2. **Document the old address** in your records as deprecated
3. **Remove team members from your Safe interface** (Settings → Owners) so it doesn't appear in their dashboards
4. **Optionally remove your Safe from any tracking tools** like DeBank, Zerion, etc.

## Common gotchas

* **Counterfactual deployment** — newer Safes are sometimes deployed lazily on different chains with the same address. If you've never sent a transaction *from* your Safe on a given chain, the contract may not actually be deployed there. Check before assuming.
* **EIP-712 signatures** — if your Safe relied on EIP-712 off-chain signature flows (signing messages, not transactions), those workflows need re-implementation against SSP's signature API
* **Hardware wallet UX** — signers who used a Ledger with their Safe will find SSP's two-device flow different (no Ledger required, but SSP Wallet + SSP Key both needed). Walk them through it before migration day.
* **Gas estimation differences** — SSP's smart account gas costs may differ from Safe's. Budget accordingly for the first few transactions while you get a feel for it.

## Estimated timeline

| Team profile | Typical duration |
|---|---|
| Single Safe, 3–5 signers, 1 chain | 3–5 days |
| Multiple Safes, 5+ signers, 2–3 chains | 1–2 weeks |
| Treasury with extensive dApp integrations | 3–4 weeks (mostly integration re-wiring) |

## Need help?

For hands-on migration support — including team training and integration audits — email **[tadeas@sspwallet.com](mailto:tadeas@sspwallet.com)** or [book a call](https://calendar.app.google/NZd7n1d6Hjmd7XFD6).
