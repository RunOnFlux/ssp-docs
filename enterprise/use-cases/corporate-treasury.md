---
icon: building-columns
---

# Corporate Treasury Workflow

This guide covers a recommended SSP Enterprise setup for a corporate treasury — company-owned crypto holdings managed by a finance team with executive sign-off, vendor payments, and clear segregation between operational and reserve funds.

## Who this is for

* Companies holding crypto on the balance sheet (treasury reserves, like MicroStrategy's BTC strategy)
* Crypto-native companies (exchanges, infrastructure providers, protocol companies) managing their own funds
* SaaS / fintech companies accepting crypto payments and managing the proceeds
* Family offices with crypto allocations
* Any business with corporate funds in crypto requiring CFO/CEO oversight

## Why corporates benefit from SSP Enterprise

* **Self-custody** — eliminate counterparty risk to custodians. The Q4 2022 custody failures (FTX, Genesis, BlockFi exposure) made self-custody a board-level discussion.
* **Audit trail** — every action logged with the signer's identity. Critical for SOC 2, ISO 27001, financial audits, and SOX-style internal controls.
* **Segregation of duties** — multi-signer requirements naturally enforce that no single employee (including the CFO) can move funds unilaterally.
* **Multi-chain** — corporates often hold BTC for treasury, ETH for protocol participation, stablecoins for vendor payments. One platform, many chains.
* **Cost** — significantly cheaper than enterprise custody platforms, freeing budget for security audits, insurance, or hiring.

## Recommended structure

### Vaults — segregate by purpose

Recommended starting structure for a mid-size corporate treasury:

| Vault | Purpose | Typical balance | Threshold |
|---|---|---|---|
| **Cold Reserve** | Long-term holdings, rarely touched | 70–90% of total | 3-of-4 (executives only) |
| **Operating** | Vendor payments, payroll, OTC | 5–20% of total | 2-of-3 (finance team) |
| **Hot / Petty** | Small recurring payments, dApp gas | 1–5% of total | 2-of-3 (finance team, lower limits) |

This mirrors traditional treasury practice: a vault structure analogous to bank reserve accounts vs operating checking accounts.

If you're a smaller company, start with **Cold Reserve + Operating** (two vaults). The Hot vault is useful when you have frequent small expenses (dApp interactions, infra payments) that you want kept away from the main operating funds.

### Signers

**Cold Reserve vault:**
* CEO
* CFO
* Board Chair (or independent director)
* Head of Finance / Treasury
* (Optionally) external signer — auditor, legal counsel, lead investor

**Operating vault:**
* CFO
* Head of Finance / Treasury
* Senior Accountant or AP Manager
* (Optionally) CEO as backup

**Hot vault:**
* Head of Finance / Treasury
* Senior Accountant
* Operations Manager (someone who actually executes day-to-day)

### Threshold choice

* **Cold Reserve: 3-of-4 or 3-of-5** — supermajority of executives required. Slow, deliberate, intentional.
* **Operating: 2-of-3** — majority of finance team. Practical for daily operations.
* **Hot: 2-of-3** — same threshold but tighter policy limits

Avoid **N-of-N** for any vault — it creates single-person bottlenecks (any vacation or sick day freezes operations).

## Recommended policies

### Cold reserve vault

```
Daily Limit:        None (you don't move cold reserve daily)
Per-Tx Limit:       None
Whitelist:          Strict Mode — only approved destinations
                    (custodian for BTC sweeps, OTC desk for sales,
                    operating vault for transfers)
Time-Lock:          24 hours, ALL transactions
Admin Approval:     Required, ALL transactions
```

This is your "vault inside the vault." Anything moving from cold reserve should require a Board-level decision and have ample time for the team to flag if something looks wrong.

### Operating vault

```
Daily Limit:        $250,000 USD (or 1 month of typical spend)
Per-Tx Limit:       $50,000 USD
Whitelist:          Warning Mode — auto-approve known vendors,
                    require admin approval for new addresses
Time-Lock:          4 hours for transactions over $25,000
Admin Approval:     Required for transactions over $50,000
```

Rationale: enough flexibility for daily ops (vendor payments, payroll) without exposing the company to a single-transaction wipeout. New vendors go through admin approval before they're whitelisted permanently.

### Hot vault

```
Daily Limit:        $10,000 USD
Per-Tx Limit:       $2,000 USD
Whitelist:          Disabled (small frequent payments to varied recipients)
Time-Lock:          None (defeats the purpose)
Admin Approval:     None
```

Limit blast radius via small balances and tight per-tx caps rather than time-locks (which would slow this vault's intended purpose).

## Operating rhythm

### Daily

* **Operating vault** processes routine vendor payments, payroll runs, OTC sweeps. Threshold met within hours, broadcast same day.
* **Hot vault** handles dApp interactions, gas payments, small recurring expenses.

### Weekly

* Finance team reviews **Operating vault audit log**: every transaction, who proposed, who signed
* Reconcile against ERP / accounting system (Cryptio, Bitwave, NetSuite, QuickBooks)

### Monthly

* **Treasury committee meeting** (CFO + relevant signers): review balances, flag anomalies, approve any cold reserve movements
* **Sweep**: move excess operating funds to cold reserve to maintain target balance ratios
* **Policy review**: any limits being hit? Any whitelist additions to formalize?

### Quarterly

* **Internal audit**: pull the full SSP Enterprise audit log, reconcile against on-chain state, present findings to Audit Committee
* **Signer review**: any role changes? Departures? Plan vault migrations if signer composition changes substantively

### Annually

* **External audit**: provide auditors with read-only Viewer access to your SSP Enterprise organization. They can see all vaults, balances, transactions, and audit logs without being able to sign.
* **Disaster recovery drill**: practice the recovery process with each signer's seed phrase backups. Confirms backups are valid and team knows the procedure.

## Compliance and reporting

### SOC 2 / ISO 27001

* SSP Enterprise's audit log provides the immutable, attributable record auditors require for "segregation of duties" controls
* Two-device per signer satisfies "two-factor authentication" requirements
* Open-source codebase is verifiable, satisfying "third-party security review" requirements

### SOX-style internal controls

* M-of-N multisig is a stronger version of "two signatures required for material disbursements"
* Per-signer limits enforce "delegation of authority" matrices
* Time-locks provide "review window" controls

### Financial audit (Big 4 or boutique)

* Provide auditors **Viewer role** access — they see everything, can change nothing
* Export audit logs for the audit period
* Reconcile on-chain state to GL via accounting integrations

### Insurance

* Self-custody insurance is available from specialty providers (Evertas, Lloyd's syndicates, others)
* Document your SSP Enterprise setup, signer composition, and policies as part of the underwriting process
* Insurance carriers generally view two-device M-of-N favorably vs single-key custody

## Common corporate-specific concerns

### "What if our CFO is hit by a bus?"

* M-of-N means the CFO alone cannot move funds, AND their absence doesn't freeze operations (as long as threshold can be met without them)
* Each signer's SSP Wallet seed phrase + SSP Key seed phrase together can recover their position if their devices are lost
* Backup procedures should be documented and tested annually

### "What if the company is acquired or restructured?"

* SSP Enterprise vault control transfers cleanly: change the **Owner** (transfer ownership critical action), update **signers** by creating new vaults under new control, migrate funds
* The on-chain multisig itself is unchanged by corporate actions — only the SSP Enterprise account control changes

### "What about sanctions / OFAC compliance?"

* You control the address whitelist — implement OFAC SDN screening on every new whitelisted address
* SSP Enterprise's audit log demonstrates that whitelisted addresses were screened and approved by named individuals
* For high-volume operations, consider integrating screening tools (Chainalysis, TRM, Elliptic) into your address-approval workflow

### "What about employee turnover?"

* When an employee with signer access leaves: immediately remove them from the SSP Enterprise organization (critical action, requires WK signing)
* For vaults where they were a designated signer: **create a new vault** without them and migrate funds. (You cannot remove them from an existing vault's signer set — this is intentional cryptographic constraint.)
* Plan for this: rotate operating vaults annually as a hygiene practice, even if no specific departure triggers it.

## Example: $50M corporate treasury, 25-employee company

* **Vaults**: Cold Reserve (3-of-4: CEO, CFO, Board Chair, Head of Treasury), Operating (2-of-3: CFO, Head of Treasury, Senior Accountant), Hot (2-of-3: Head of Treasury, Senior Accountant, Operations Manager)
* **Balance allocation**: 80% Cold Reserve, 18% Operating, 2% Hot
* **Policies**: as recommended above
* **Cadence**: daily ops, weekly reconciliation, monthly committee meeting, quarterly audit, annual external audit + DR drill
* **Tooling**: SSP Enterprise + Cryptio for accounting + Chainalysis for AML screening

## Next steps

* **[Creating Multisig Vaults](../creating-vaults.md)** — set up your treasury vaults
* **[Configuring Policy Controls](../policies.md)** — apply the recommended policies
* **[Inviting Team Members & Roles](../inviting-members.md)** — onboard your finance team and executives
