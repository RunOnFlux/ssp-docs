---
icon: file-signature
---

# Proposing & Signing Transactions

Transactions out of a multisig vault require collective approval. The flow is:

1. **Anyone with proposer rights** (Member, Admin, Owner who is also a vault signer) creates a **proposal**
2. **Designated signers** approve the proposal with their SSP Wallet + SSP Key
3. Once the **signing threshold** is reached (M of N), the transaction broadcasts to the blockchain

This guide walks you through both sides — creating a proposal and signing one.

## Part 1 — Create a Proposal

### Step 1 — Open the vault and click "New Proposal"

Navigate to **Vaults → [your vault]** and click **New Proposal** in the top right.

The Create Proposal page opens.

<!-- SCREENSHOT: vault detail with New Proposal button -->

### Step 2 — Add recipients

For each recipient:

* **Address** — type or paste the destination address. The field auto-suggests vault-owned addresses and saved contacts.
* **Amount** — how much to send (in the chain's native unit or selected token)

Click **+ Add Recipient** to send to multiple destinations in one transaction (useful for batch payouts; saves on fees).

Click the red **Delete** button on a row to remove it.

### Step 3 — Choose the source address

If your vault has multiple receive addresses with balances, select which **Source Address** to spend from. The dropdown shows each address with its label and balance.

For UTXO chains (Bitcoin, Litecoin, etc.), this determines which UTXOs are spent. For EVM chains, this is the smart account that will execute the transfer.

### Step 4 — (EVM only) Select token

If the vault is on an EVM chain (Ethereum, Polygon, BSC, Avalanche, Base), choose what to send:

* **Native token** (ETH, MATIC, BNB, AVAX, ETH on Base)
* Any **ERC-20 token** the vault is watching

The dropdown shows current balance for each option.

### Step 5 — Configure the fee

* **Auto Fee** (default) — the platform estimates the appropriate network fee. You'll see the estimate in native units and USD.
* **Manual Fee** — toggle on to override. For UTXO chains, set sat/byte. For EVM, set gas price and gas limit.
* **Max** — clicking the Max button sets the amount to the entire available balance, automatically subtracting the fee.

### Step 6 — Designate signers (optional)

By default, all vault signers can approve the proposal. If you want only specific signers to be requested:

* Check **Request from specific signers only**
* Select the signers you want notified

This is useful when you know certain signers are unavailable, or when policy requires a specific subset (e.g., always include the CFO on payroll).

The platform burns one nonce per designated signer at proposal creation, so designate carefully — see [Why nonces matter](#why-nonces-matter) below.

### Step 7 — Review

Click **Review Proposal**. A confirmation modal shows:

* All recipients and amounts
* Total in USD (if exchange rate available)
* Estimated fee
* Vault and chain
* Designated signers

Verify everything carefully. Once you create the proposal, **the nonces are committed** — even rejecting or expiring the proposal won't return them.

### Step 8 — Create and sign

Click **Create & Sign**.

* Your **SSP Wallet** prompts you to approve the proposal creation. Click **Approve**.
* Your **SSP Key** prompts on your phone. Approve.

On success, you see **Proposal created** and land on the proposal detail page.

## Part 2 — Sign a Proposal

### Step 1 — Find the proposal

You'll be notified in two ways:

* **In-app notification** — appears in **Notifications → Signing Requests**
* **On the vault detail page** — visible in the Proposals tab with a **Pending Signatures** badge

Click the proposal to open its detail page.

<!-- SCREENSHOT: signing requests notification -->

### Step 2 — Review the details

The proposal detail page shows:

* Vault name and chain
* All recipient addresses and amounts
* Total in USD
* Network fee
* **Signature progress bar** — e.g., `1 of 3 signatures collected`
* Status badge — `Pending Signatures` (orange), `Ready to Broadcast` (green), `Broadcast` (gray)
* List of signers with their signature status

Read carefully. Once you sign, you've cryptographically committed to this exact transaction. You cannot un-sign.

### Step 3 — Click "Sign Transaction"

If you're a designated signer and haven't yet signed, the **Sign Transaction** button is enabled.

* Your **SSP Wallet** opens with the full transaction details. Verify the recipient(s), amount(s), and fee match what's shown on the page. Click **Approve**.
* Your **SSP Key** prompts on your phone. Verify the same details, then approve with PIN/biometric.

On success:

* Your signature is added to the proposal
* The progress bar updates: e.g., `2 of 3 signatures collected`
* You see **Signature added**

### Step 4 — Wait for the threshold (or broadcast)

If the signing threshold is now met (e.g., 3 of 3), the status changes to **Ready to Broadcast** and a **Broadcast** button appears for any vault member. Click it to send the transaction to the blockchain.

If the threshold is not yet met, the proposal remains **Pending Signatures** until the remaining required signers approve.

### Step 5 — Confirmation

Once broadcast:

* The status changes to **Broadcast** then **Confirmed** as block confirmations come in
* A block explorer link appears (e.g., `View on Etherscan`)
* The transaction shows up in the vault's **Transactions** tab
* The vault balance updates

## Rejecting a proposal

If you don't agree with a proposal:

1. Open the proposal detail page
2. Click **Reject**
3. Sign the rejection with SSP Wallet + SSP Key (this is logged for audit)

Rejecting your own signature alone does not cancel the proposal — it just records your refusal. The proposal is cancelled when:

* Enough signers reject that the remaining signers cannot reach the threshold
* The proposer cancels it explicitly (if no one has signed yet)
* It expires (default: 24 hours after creation)

> **Note:** Rejecting or letting a proposal expire **does not** return the burned nonces. If the proposal needs to be re-attempted, the proposer creates a new one and burns fresh nonces.

## Why nonces matter

For EVM chains (Ethereum, Polygon, BSC, Avalanche, Base), every signature requires a fresh cryptographic nonce — a one-time random value. Each signer's SSP Wallet and SSP Key maintain a local pool of pre-generated nonces synced with the platform.

When a proposal is created with designated signers, the platform **immediately burns one wallet nonce + one key nonce** for each designated signer. This is for cryptographic safety — nonces can never be reused without compromising the signature scheme.

Practical implications:

* **Don't designate signers you don't intend to ask** — if you designate 5 signers but only need 3, you've burned 4 extra nonces
* **Failed signing burns nonces** — if a signer's wallet glitches mid-sign, the nonces are still spent
* **Cancelled or expired proposals burn nonces** — the burn happens at creation, not at completion
* **Nonce pools auto-replenish** — your wallet/key sync new nonces in the background; you'll rarely run out

If a signer's nonce pool is empty (rare), they'll see a sync prompt before they can sign. Wait for the pool to refill.

## Critical action signing — explained

Some operations are too sensitive to authorize via session cookies alone. They require **WK re-signing** — proving that you control both your SSP Wallet and SSP Key by signing a server-generated challenge.

Critical actions in SSP Enterprise:

* Transferring organization ownership
* Deleting an organization
* Removing a member
* Archiving a vault
* Changing your linked email

The flow:

1. You initiate the action (e.g., click **Remove Member**)
2. The server generates a challenge message: `<timestamp>:SSP_CRITICAL:<action>:<targetOrgId>:<targetWkIdentity>:<nonce>`
3. Your SSP Wallet and SSP Key prompt you to sign this challenge
4. The server verifies both signatures, confirms they match your WK Identity, and executes the action
5. The attempt (success or failure) is logged permanently in the audit trail

This prevents session hijacking attacks and guarantees that destructive actions are intentional.

## Audit trail

Every significant action — proposal created, signed, rejected, broadcast, member added/removed, vault created/archived, policy changed — is logged permanently. You can review the full history in:

* **Activity** (organization-level)
* **Vault → Activity** (vault-level)

Audit logs are append-only and never deleted, even when the related entity (member, vault) is removed.

## Next steps

* **[Configuring Policy Controls](policies.md)** — add guardrails: spending limits, address whitelists, time-locks, admin approval thresholds
