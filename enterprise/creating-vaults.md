---
icon: vault
---

# Creating Multisig Vaults

A **vault** is an M-of-N multisig wallet on a single blockchain. Your organization can have many vaults across many chains — for example, an Ethereum operations vault (2-of-3), a Bitcoin reserve vault (3-of-5), and a Polygon payroll vault (2-of-4). Each vault has its own signers, threshold, balances, transaction history, and policies.

This guide walks you through the 5-step Create Vault wizard.

## Before you begin

* You must be an **Owner** or **Admin** of the organization
* The members you want as signers must already be in the organization with their SSP Wallet linked (they need to have logged in at least once)
* Decide:
  * Which **chain** the vault will operate on
  * Who the **signers** will be (this is permanent)
  * What the **signing threshold** should be (this is permanent)

## Critical: signers and threshold are permanent

Once a vault is created, you **cannot change**:

* The blockchain
* The signer set
* The signing threshold

You can change:

* The vault name and description
* Policies (spending limits, whitelists, etc.)

If your team changes (someone leaves, someone joins) and you need them on the vault's signer set, you'll need to **create a new vault** and migrate funds. Plan accordingly.

## Step 1 — Open Vaults and click "Create Vault"

From your organization, navigate to **Vaults** in the main nav, then click **Create Vault** in the top right.

The Create Vault wizard opens with 5 steps shown in a progress indicator: **Chain → Name → Signers → Threshold → Review**.

<!-- SCREENSHOT: Vaults page with Create Vault button -->

## Step 2 — Choose the chain

Subtitle: *Choose the blockchain network for this vault.*

You'll see a grid of supported chains:

**Mainnet:** Bitcoin, Ethereum, Litecoin, Dogecoin, Flux, Bitcoin Cash, Zcash, Ravencoin, BNB Smart Chain, Avalanche, Polygon, Base.

**Testnet:** Bitcoin Testnet, Bitcoin Signet, Sepolia (Ethereum testnet), Amoy (Polygon testnet), Flux Testnet — useful for trying things out without real funds.

Click a chain card to select it (it highlights in blue), then click **Next →**.

> **Tip:** Test on a testnet vault first if this is your first time. Bitcoin Signet and Sepolia are free, fast, and behave identically to mainnet.

## Step 3 — Name the vault

Subtitle: *Give your vault a descriptive name to identify it within your organization.*

* **Name** (required, 2–100 characters): e.g., `Treasury`, `Operations Fund`, `Q2 Payroll`
* **Description** (optional, up to 500 characters): explain what the vault is for

Click **Next →**.

## Step 4 — Select signers

Subtitle: *X of Y organization members are eligible (have their SSP Wallet linked).*

Only members who have logged in to SSP Enterprise at least once (so their SSP Wallet is linked) appear here. If a member you want isn't shown, ask them to log in first.

### Signing mode (if available)

Depending on your plan and the chain, you may see a signing mode selector:

* **Dual Signing** — each signer uses both their SSP Wallet and SSP Key (2 keys per signer). The default and most secure option.
* **Key Only Signing** — each signer uses only their SSP Key (1 key per signer). Allows more signers per vault.
* **Wallet Only Signing** — each signer uses only their SSP Wallet (1 key per signer). Allows more signers per vault.

Each mode shows the maximum number of signers allowed.

### Selecting signers

From the **Signers** dropdown, multi-select the members who will be on this vault. Each option shows their name, email (if available), truncated SSP Identity, and role badge.

You'll see live counters: e.g., **Signers (3/8)** — you've selected 3, max is 8.

> ⚠️ **Signers are permanent.** You cannot change the signer set after the vault is created. Make sure to include all members who should be able to sign transactions for this vault. Err on the side of including more — you can always set a higher threshold later (e.g., 3-of-5 instead of 3-of-3) if you want flexibility.

Click **Next →**.

## Step 5 — Set the signing threshold

Subtitle explains: *Set the number of signers required to approve each transaction.*

You'll see preset cards based on your signer count, plus a custom input.

### Common presets

* **Any signer (1-of-N)** — *Either member can authorize on their own. Fast but less secure.* Useful for low-value operational vaults where speed matters more than security.
* **Majority (e.g., 2-of-3, 3-of-5)** — *More than half must approve. The industry standard — secure yet tolerates [N - majority] unavailable signers.* This is usually the right choice. Often marked **Recommended**.
* **All required (N-of-N)** — *Every signer must approve. If even one key is lost, funds will be permanently locked.* Strong security but **fragile**: any unavailable signer means the vault is frozen until they return.

For 3-of-3 or higher N-of-N setups, you'll see a red warning:

> **Risk of permanent fund loss.** With an N-of-N setup, every single signer must be available for every transaction. If even one member loses access to their key, all funds in this vault will be permanently unrecoverable. Consider using a majority threshold instead.

### Custom threshold

You can also type a custom number in the **Required Signers** field (between 1 and your signer count).

> ⚠️ **Threshold is permanent.** Choose carefully. The most common mistake is choosing a threshold so high that the vault becomes operationally fragile.

Click **Next →**.

## Step 6 — Review and create

The review screen shows:

* **Chain:** logo, name, symbol
* **Name:** what you entered
* **Multisig:** e.g., `2-of-3 signers (4-of-6 keys with Dual Signing)`
* **Signing Mode:** if not Dual
* **Signers:** bulleted list with name, identity, and role

A red warning reminds you:

> These settings are permanent. Once created, the blockchain, signers, and signing threshold cannot be modified. The vault name can be updated later.

If everything looks right, click **Create Vault**.

## What happens on create

The platform now coordinates with each designated signer to collect their **extended public key (xpub)** for the vault:

1. Each signer receives a notification on their SSP Wallet and SSP Key
2. Each signer approves the xpub generation request — their wallet and key derive a fresh xpub specifically for this vault (using the org index, vault index, and chain)
3. The platform combines all signers' xpubs into the multisig script
4. The vault address is derived deterministically from the combined xpubs

You'll see the vault status as **Pending Setup** during this process. Once all signers have contributed their xpub, the status changes to **Active** and the first vault address is displayed.

> **What this means cryptographically:** the vault address is a function of all signers' xpubs. No single signer (or even the platform) can derive the address alone. This is true self-custody — even if SSP Enterprise went offline tomorrow, your signers could recover the vault using their seed phrases and the public derivation paths.

## Receiving funds

Once the vault is **Active**:

1. Open the vault detail page
2. Click **Receive** (or navigate to **Addresses**)
3. Copy the vault's deposit address
4. Send funds to it from any external source

You can generate multiple receive addresses per vault (each one is also derived from all signers' xpubs).

## Archiving a vault

If you no longer need a vault:

1. Open **Vault → Settings → Archive Vault**
2. The action requires **WK re-signing** (a critical action)
3. Once archived, the vault is read-only — no new proposals, no new addresses
4. Existing balances and transaction history are preserved
5. Archived vaults don't count against your plan's vault limit

You cannot un-archive from the UI today; archiving is intended as a soft-delete for vaults that have been emptied and decommissioned.

## Next steps

* **[Proposing & Signing Transactions](transactions.md)** — move funds out of your new vault
* **[Configuring Policy Controls](policies.md)** — set spending limits and whitelists for the vault
