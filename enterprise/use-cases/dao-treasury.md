---
icon: people-group
---

# DAO Treasury Management

This guide covers a recommended SSP Enterprise setup for a DAO treasury — community-governed funds with multi-signer council approval, on-chain transparency, and clear separation between operational spending and long-term reserves.

## Who this is for

* DAOs with a designated treasury council (typically 3–9 elected members)
* Protocols with foundation treasuries managed by core contributors
* Sub-DAOs spending allocated budgets within a larger DAO
* Any community-governed pool where decisions are made by a multisig council on behalf of token holders

## Why DAOs benefit from SSP Enterprise

* **True self-custody** — no custodian, no MPC vendor sitting between the DAO and its funds. Aligns with the decentralization ethos.
* **Audit trail** — every proposal, approval, and rejection is logged with the signer's identity and timestamp. Meets community demand for transparency.
* **Multi-chain treasury** — protocol may be on Ethereum but treasury holds BTC, USDC, native token across chains. Manage from one place.
* **Policy enforcement** — spending limits prevent runaway proposals; whitelists prevent accidental sends to wrong addresses; time-locks give the community time to react to suspicious activity.

## Recommended structure

### Vaults

Most DAOs benefit from at least three vaults, separated by purpose:

| Vault | Purpose | Threshold |
|---|---|---|
| **Cold Reserve** | Long-term holdings, rarely touched | 4-of-7 (highest threshold) |
| **Operations** | Recurring expenses: contributor pay, infra, tooling | 3-of-5 |
| **Grants & Initiatives** | Discretionary spending on community proposals | 3-of-5 |

This separation means:
* The cold reserve is rarely accessed → can use a higher threshold without ops friction
* Operations runs frequently → middle threshold
* Grants are visible per-vault → community can audit spending by category

If you're a smaller DAO, start with two vaults (Reserve + Operations) and add Grants later.

### Signers

Recommended council composition:

* **5 signers** for sub-$10M treasuries
* **7 signers** for $10M–$100M treasuries
* **9 signers** for $100M+ treasuries (gets unwieldy beyond this)

Keep the signer count **odd** — avoids tied votes if you ever do informal off-chain coordination.

Signer composition should mix:
* **Core contributors** — high engagement, available for fast approvals
* **Community-elected representatives** — legitimacy with token holders
* **Independent advisors** — checks and balances; less likely to collude with insiders

Avoid having all signers from the same legal entity, geography, or social group — diversification reduces risk of coordinated compromise or jurisdictional capture.

### Threshold choice

* **Operations vault: 3-of-5 or 3-of-7** — majority approval, tolerates 2 absences
* **Cold reserve: 5-of-7 or 6-of-9** — supermajority, very deliberate moves only

Avoid:
* **N-of-N** — one offline signer freezes the vault. Catastrophic for a DAO with global signers.
* **2-of-N** — too easy to collude. Below the bar of "majority approval" community expects.

## Recommended policies

### Operations vault

```
Daily Limit:        $50,000 USD
Per-Tx Limit:       $20,000 USD
Whitelist:          Warning Mode (admin approval for new addresses)
Time-Lock:          1 hour for transactions over $10,000
Admin Approval:     None (operations should move fast)
```

Rationale: keep operations nimble for routine payments but cap exposure. Time-lock catches mistakes; warning-mode whitelist tolerates new contributors without blocking them entirely.

### Cold reserve vault

```
Daily Limit:        None (rarely used; if you're hitting a daily limit, something's wrong)
Per-Tx Limit:       None
Whitelist:          Strict Mode (only approved destinations)
Time-Lock:          24 hours, ALL transactions
Admin Approval:     Required, ALL transactions
```

Rationale: deliberate, observable, reversible-in-window. The 24-hour time-lock means the community has a full day to react if a transaction looks wrong. Strict whitelist prevents any "send to attacker" scenario from being viable.

### Grants vault

```
Daily Limit:        $100,000 USD
Per-Tx Limit:       $50,000 USD
Whitelist:          Disabled (grants go to many one-time recipients)
Time-Lock:          4 hours for transactions over $25,000
Admin Approval:     Required for transactions over $25,000
```

Rationale: grants need flexibility (every recipient is different), but high-value grants get extra scrutiny.

## Operating rhythm

### Day-to-day

* **Routine payments** (contributor pay, infra costs): proposed by treasurer, approved by 3-of-5 council members within 24 hours
* **New addresses on operations vault**: any new vendor goes through warning-mode approval — gives community a chance to flag

### Weekly

* **Treasury review**: review the past week's transactions in the SSP Enterprise audit log; share a summary with the community (often as a forum post or Discord update)

### Monthly

* **Policy review**: review spending against limits; adjust if you're consistently hitting them or way under
* **Reserve rebalancing** (if applicable): move funds between cold reserve and operations to maintain target balances

### Quarterly

* **Council rotation review**: per your DAO governance, signers may rotate. When a signer leaves, **create a new vault** with the updated signer set and migrate funds. (Vault signer sets cannot be edited after creation — this is intentional for security but operationally relevant.)

## On-chain transparency

A common DAO requirement: the community should be able to see the treasury independently of trusting the council.

SSP Enterprise vaults are **standard on-chain multisig addresses** — fully visible on block explorers. Your community can:

* Watch the address on Etherscan / mempool.space / etc.
* Use third-party trackers (DeBank, Zerion, DefiLlama Treasury) to see balances and history
* Verify the multisig configuration (signers, threshold) on-chain

You can also publish your vault addresses in your DAO's docs or on a transparency page. SSP Enterprise's audit log adds context (who proposed, who signed, when, why) on top of what's already public on-chain.

## Common pitfalls

* **Signer offboarding without a plan** — a council member leaves, but nobody migrates the vault. Months later, you can't reach quorum. **Always migrate to a new vault when council composition changes substantively.**
* **Letting the operations vault accumulate** — operations should be a small float, not the bulk of the treasury. Sweep excess to cold reserve regularly.
* **No policy on "emergency" transactions** — what happens if you need to move $5M in 30 minutes during a market crisis? Define this *before* it happens. Some DAOs have a designated "emergency vault" with a low threshold (e.g., 2-of-5) for time-sensitive moves, with a small balance only.
* **Single-jurisdiction signers** — if all your signers are in one country and that country imposes sanctions or asset freezes, your treasury is at risk. Geographic diversity matters.
* **Skipping testnet** — DAOs often skip testnet validation because "we'll just be careful." A signer fumbling their first real signing on a $1M proposal is a community-trust event. Test first.

## Recommended SSP Enterprise role mapping

* **Owner**: a long-tenured core contributor or foundation lead — should be someone whose departure would be a major event
* **Admins**: 2–3 senior contributors who handle vault setup and policy changes
* **Members**: the council signers
* **Viewers**: anyone who needs visibility (community managers, finance team) but shouldn't be able to sign

## Example: 7-signer DAO with $25M treasury

* **Vaults**: Cold Reserve (5-of-7), Operations (3-of-5 from a subset of council), Grants (3-of-5 from a subset of council)
* **Signers**: 4 core contributors + 2 community-elected delegates + 1 independent advisor
* **Policies**: as above for each vault
* **Cadence**: daily ops, weekly community report, monthly policy review, quarterly council review

This structure has been operationally validated by multiple SSP Enterprise customers including the Flux Foundation, who use it to manage the Fusion bridge and Foundation resources.

## Next steps

* **[Creating Multisig Vaults](../creating-vaults.md)** — set up your first DAO vault
* **[Configuring Policy Controls](../policies.md)** — apply the policies above
* **[Inviting Team Members & Roles](../inviting-members.md)** — bring your council in
