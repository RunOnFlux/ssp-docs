---
icon: shield-halved
---

# Configuring Policy Controls

The **transaction policy engine** lets you constrain what vaults can do beyond the M-of-N signing threshold. Even if signers approve a transaction, the policy engine can block, delay, or escalate it based on rules you've defined.

Policies cascade in three tiers:

```
Organization defaults  →  Vault overrides  →  Per-signer overrides
```

The **stricter rule always wins**. If your organization sets a $50,000 daily limit and a specific vault sets $20,000, the effective limit is $20,000.

## When to use policies

* Limit how much a vault can move per day, week, or month
* Restrict where funds can be sent (whitelist of approved addresses)
* Add a delay between threshold-met and broadcast (lets you cancel suspicious transactions)
* Require an Admin to approve any transaction over a USD threshold
* Cap individual signers — junior staff get tighter limits than executives

## Where policies live

* **Organization → Policies** — defaults that apply to every vault in the org
* **Vault → Policies** — overrides for a single vault
* **Vault → Policies → Per-Signer Limits** — overrides for an individual signer on that vault

## Spending Limits

Set in USD. The platform converts to native token at current exchange rates when evaluating each transaction.

### Aggregate limits

* **Per Transaction** — max value of a single transaction
* **Daily** — rolling 24-hour cap
* **Weekly** — rolling 7-day cap
* **Monthly** — rolling 30-day cap

Leave any field blank to disable that specific limit.

> **Example:** Set Daily = $50,000 and Per Transaction = $25,000 to allow up to two large transactions per day, or many smaller ones up to the daily cap.

### Token-specific limits

In addition to USD aggregate caps, you can set per-token caps:

* Pick a token (BTC, ETH, USDC, etc.)
* Set its own daily / weekly / monthly cap

This is useful when you want different rules per asset (e.g., loose limits on a stablecoin you transact frequently, tight limits on a volatile asset).

### Live usage tracking

Each policy page shows current usage:

* Progress bars for daily / weekly / monthly spend as a percentage of the limit
* `$X of $Y USD spent this [period]`

When a proposal would exceed a limit, it's **blocked** at creation time — no nonces are burned, no signatures collected. The proposer sees a clear error explaining which limit was hit.

## Address Whitelist

The whitelist controls which destinations a vault can send funds to. Three modes:

* **Disabled** — no whitelist; any address can receive funds
* **Warning Mode** — whitelisted addresses are auto-approved; non-whitelisted require Admin approval before broadcast
* **Enabled (Strict)** — only whitelisted addresses can receive funds; everything else is blocked at proposal creation

### Adding addresses

1. Open **Policies → Address Whitelist**
2. Click **+ Add Address**
3. Enter:
   * **Address** (validated against the chain's format)
   * **Label** (optional, e.g., `Treasury Cold Storage`, `Payroll Gnosis Safe`, `Custodian Off-Ramp`)
4. Click **Add to Whitelist**

The address appears in the table with the label, who added it, and when.

### Removing addresses

Click **Delete** on the row, confirm. The whitelist updates immediately.

> **Tip:** Run in **Warning Mode** for the first few weeks to identify which addresses your team actually uses, then promote to **Strict Mode** once your whitelist is comprehensive.

## Time-Lock Delays

A time-lock adds a waiting period between when a proposal reaches its signing threshold and when it can be broadcast.

* **Threshold** (in seconds) — e.g., `3600` = 1 hour, `86400` = 24 hours
* **USD trigger** (optional) — only apply the delay to transactions above a certain amount

During the delay window:

* The proposal status is **Awaiting Time Lock**
* Any vault member can **Cancel** the proposal (no signing required for cancellation during this window)
* When the window closes, the **Broadcast** button becomes available

Use this for:

* High-value transactions where a 24-hour delay gives you time to detect and cancel an attack
* Treasury operations that require off-chain coordination (e.g., notifying a counterparty before broadcasting)

## Admin Approval

Require an Admin to approve any transaction above a configurable USD threshold, in addition to the regular signer threshold.

* **Threshold (USD)** — e.g., $100,000 — set to 0 to require Admin approval on every transaction
* When triggered, the proposal status becomes **Awaiting Admin Approval** after signers reach the M-of-N threshold
* An Admin must explicitly approve in the proposal detail page before broadcast

Use this when:

* You have a large signer set and want a final check from a smaller approval committee
* Your governance requires Board-level sign-off above certain amounts

## Per-Signer Spending Limits (Vault Policies)

On a vault, you can override the spending limits for individual signers. This is useful when:

* Junior team members should only be able to propose smaller transactions
* Executives need higher caps but you don't want to raise the vault-wide limit
* External contractors or partners need extremely tight caps

### Adding a per-signer override

1. Open **Vault → Policies → Per-Signer Limits**
2. Click **+ Add Signer Override**
3. Select the signer
4. Set their daily / weekly / monthly caps in USD
5. Click **Save Signer Policy**

The signer-specific caps override the vault-wide caps for that signer **only**, and only when they are the proposer of a transaction.

> **Important:** Per-signer limits apply to transactions the signer **proposes**, not to transactions they **co-sign**. A junior signer with a $5,000 daily cap can still co-sign a $50,000 transaction proposed by an executive, as long as the executive's cap allows it.

### Removing an override

Click **Delete** on the signer's row. The signer falls back to the vault-wide caps.

## Velocity tracking

The platform automatically tracks **rolling 30-day spending windows** at three levels:

* Per vault
* Per signer (on each vault)
* Per token

This data feeds the spending limit enforcement and is also visible in **Analytics** for your own monitoring.

## Emergency vault freeze

For incident response, Owners and Admins can **freeze** a vault, blocking all proposals until unfrozen.

1. Open **Vault → Settings → Emergency Freeze**
2. Click **Freeze Vault**
3. Sign with **SSP Wallet + SSP Key** (this is a critical action)
4. The vault status changes to **Frozen** — no new proposals can be created, no pending proposals can be signed or broadcast

To unfreeze: same flow, click **Unfreeze Vault** and sign.

Use this when:

* You suspect a signer's device has been compromised
* You're investigating an unusual transaction
* You need to pause all activity during a security review

Existing on-chain balances are unaffected — freeze only blocks the SSP Enterprise interface. The on-chain multisig itself remains intact.

## Policy hierarchy in practice

A worked example. Suppose:

* **Organization** — Daily limit $100,000, Whitelist: Warning Mode
* **Treasury Vault (override)** — Daily limit $50,000, Whitelist: Strict Mode, Admin Approval over $25,000
* **Junior Signer (override on Treasury Vault)** — Daily limit $5,000

When the junior signer proposes a $30,000 transaction from the Treasury Vault:

1. Junior's daily cap ($5,000) is checked first → **blocked at proposal creation**

When the junior signer proposes a $4,000 transaction from the Treasury Vault to a non-whitelisted address:

1. Junior's daily cap allows $4,000 ✅
2. Treasury Vault Strict Whitelist → **blocked at proposal creation** (address not whitelisted)

When an Admin proposes a $30,000 transaction from the Treasury Vault to a whitelisted address:

1. Admin has no per-signer cap, falls back to Treasury Vault $50,000 ✅
2. Whitelisted ✅
3. After signers reach M-of-N → status becomes **Awaiting Admin Approval** (because $30,000 > $25,000)
4. A separate Admin approves → broadcast

The stricter rule always wins, evaluated in order: per-signer → vault → organization.

## Next steps

* **[Inviting Team Members & Roles](inviting-members.md)** — make sure roles are set right before relying on Admin Approval rules
* **[Creating Multisig Vaults](creating-vaults.md)** — apply policies to specific vaults
