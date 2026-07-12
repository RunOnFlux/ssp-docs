---
icon: code
---

# Enterprise API

The SSP Enterprise API gives your own systems programmatic access to your organization's vaults, balances, transactions, proposals, and portfolio analytics. Use it to feed an internal dashboard, reconcile balances, monitor proposal status, or export data into your own tooling — without logging into the app. Read access is read-only; a single optional write scope lets automation **create a proposal** that still requires your normal signatures to execute.

> **A key can never move funds.** The read scopes are `GET`-only. The write scopes only ever queue or configure — never sign, approve, or broadcast: `proposals:write` can **create a pending proposal**, `policies:write` can adjust a vault's spending *rules*, `vaults:write` can create/configure vaults, and `contacts:write` edits a labelling address book. Every proposal, however it was created, still requires the vault's M-of-N device signatures (each signed by both SSP Wallet and SSP Key) to execute. So a leaked key can, at most, *propose* a payment your signers must then approve in the app — it can never move money or change ownership. This is the core safety property of the API, enforced by construction.

## Base URL

```
https://relay.sspwallet.com/v1/api
```

## Authentication

The Enterprise API authenticates with a **bearer API key**. Send it in the `Authorization` header on every request:

```
Authorization: Bearer ssp_live_<your key>
```

A key looks like `ssp_live_` followed by a long random string, for example:

```
ssp_live_a1B2c3D4e5F6g7H8i9J0k1L2m3N4o5P6q7R8s9T0u1V2
```

If the header is missing, malformed, points to a revoked or expired key, or lacks the scope required by the endpoint, the request is rejected — see [Errors](#errors).

### The organization is derived from the key

An API key belongs to exactly **one organization**, and that organization is taken from the key itself — never from the URL or any request parameter. A key issued for organization A can only ever read organization A's data; it can never read, reference, or even discover another organization. There is no organization ID to pass, and no way to widen a key's reach by changing the request.

### Creating a key

API keys are created and managed inside the SSP Enterprise app, under **Settings → Developers → API Keys**.

* Only **owners and admins** of the organization can create or revoke API keys. Members and viewers cannot.
* When you create a key you choose a **name** (for your own reference, e.g. "Grafana read-only") and select which [scopes](#scopes) it should have.
* The full key is **shown only once**, at creation time. SSP stores only a SHA-256 hash of it and can never show it again. Copy it into your secret manager immediately. If you lose it, revoke it and create a new one.
* For identification, the app displays only a short **prefix** of the key (e.g. `ssp_live_a1b2`) alongside its name, scopes, status, and last-used time.

### Revoking a key

From the same **Settings → Developers → API Keys** screen, an owner or admin can revoke any key. Revocation is **immediate** — the key is checked against its status on every request, so the next call made with a revoked key fails with `401`. Revocation cannot be undone; create a new key if you need access again.

Keep your keys secret. Treat `ssp_live_...` like a password: never commit it to source control, embed it in a frontend bundle, or paste it into a browser. A leaked key can read one organization's data and, if it has the `proposals:write` scope, create a *pending* proposal — but it can never sign, move funds, or change settings, because execution always requires your vault's device signatures.

## Scopes

Each key carries a set of **scopes**. Every endpoint requires one specific scope; if the key doesn't have it, the request fails with `403`. Grant a key only the scopes it actually needs.

| Scope | Grants read access to |
|---|---|
| `org:read` | Your organization's profile |
| `vaults:read` | The list of vaults and individual vault details |
| `balances:read` | Per-vault balances |
| `transactions:read` | Per-vault transaction history |
| `proposals:read` | Per-vault transaction proposals |
| `analytics:read` | Aggregated portfolio analytics |
| `contacts:read` | Your organization's saved address book |
| `policies:read` | Vault & org policies, policy rules, approval groups, and policy templates |

| Write scope | Grants |
|---|---|
| `proposals:write` | Create or cancel a **pending** proposal on a vault — never signs or moves funds |
| `contacts:write` | Manage the organization address book (create / update / delete contacts) |
| `policies:write` | Update a vault's spending policy (limits, whitelist, rules) — a control, not a fund movement |
| `vaults:write` | Create and configure vaults, and manage vault tag definitions — never signs or moves funds |

The read scopes are read-only. The write scopes never sign, approve, broadcast, or move funds: `proposals:write` can only create or cancel a pending proposal (which still requires your vault's device signatures to execute), `contacts:write` only edits a labelling address book that has no effect on policy, `policies:write` only tightens or loosens a vault's spending *rules*, and `vaults:write` only creates or configures vaults (a vault created this way holds no funds until you fund it, and still needs its M-of-N devices to spend). None can move money or approve a proposal. A `policies:write` key's creator must be an admin of the vault it targets; `vaults:write` requires an organization admin. Creating a key with any write scope requires the **Pro plan or higher**.

## Response format

Every response uses the standard SSP envelope:

```json
{ "status": "success", "data": { } }
```

On error, `status` is `"error"` and `data` describes the problem — see [Errors](#errors).

## Pagination

List endpoints (vaults, transactions, proposals) use **cursor-based pagination**:

* `limit` — how many items to return per page. The default is 20 and the maximum is **100**; larger values are clamped to 100.
* `cursor` — an opaque token. Omit it on the first request; pass the `nextCursor` from a response to fetch the next page.

Each paginated response includes a `pagination` object:

```json
{
  "status": "success",
  "data": {
    "items": [ ],
    "pagination": {
      "limit": 20,
      "nextCursor": "eyJpZCI6IjY1YS4uLiJ9",
      "hasMore": true
    }
  }
}
```

When `hasMore` is `false`, `nextCursor` is `null` and you've reached the end. Treat the cursor as opaque — its contents may change, so don't parse or construct it yourself.

Example: fetch the next page of transactions:

```bash
curl https://relay.sspwallet.com/v1/api/vaults/VAULT_ID/transactions?limit=50&cursor=eyJpZCI6IjY1YS4uLiJ9 \
  -H "Authorization: Bearer ssp_live_..."
```

## Rate limits

Requests are rate limited **per API key** (with an additional global per-IP limit). The read API allows roughly **60 requests per minute per key**, with a short burst allowance above that. If you exceed the limit, you receive a `429` response; back off and retry after a short delay. Batch and cache where you can rather than polling tightly.

## Endpoints

All endpoints are `GET` and require the `Authorization: Bearer` header. Path parameters such as `:vaultId` and `:proposalId` are the IDs returned by the list endpoints.

---

### Get organization

```
GET /v1/api/org
```

Scope: `org:read`

Returns the profile of the organization the key belongs to.

```bash
curl https://relay.sspwallet.com/v1/api/org \
  -H "Authorization: Bearer ssp_live_..."
```

```json
{
  "status": "success",
  "data": {
    "id": "65a1b2c3d4e5f60718293a4b",
    "name": "Acme Treasury",
    "orgIndex": 1024,
    "createdAt": "2026-01-12T09:30:00Z",
    "memberCount": 6,
    "vaultCount": 3
  }
}
```

---

### List vaults

```
GET /v1/api/vaults
```

Scope: `vaults:read`

Lists the vaults in your organization. Paginated.

```bash
curl https://relay.sspwallet.com/v1/api/vaults?limit=20 \
  -H "Authorization: Bearer ssp_live_..."
```

```json
{
  "status": "success",
  "data": {
    "items": [
      {
        "id": "70c1f2a3b4c5d6e7f8091a2b",
        "name": "Treasury — BTC",
        "chain": "btc",
        "threshold": 2,
        "signerCount": 3,
        "status": "active",
        "createdAt": "2026-02-01T14:00:00Z"
      }
    ],
    "pagination": { "limit": 20, "nextCursor": null, "hasMore": false }
  }
}
```

---

### Get a vault

```
GET /v1/api/vaults/:vaultId
```

Scope: `vaults:read`

Returns full details for a single vault in your organization.

```bash
curl https://relay.sspwallet.com/v1/api/vaults/70c1f2a3b4c5d6e7f8091a2b \
  -H "Authorization: Bearer ssp_live_..."
```

```json
{
  "status": "success",
  "data": {
    "id": "70c1f2a3b4c5d6e7f8091a2b",
    "name": "Treasury — BTC",
    "chain": "btc",
    "threshold": 2,
    "signers": [
      { "wkIdentity": "...", "role": "owner" },
      { "wkIdentity": "...", "role": "admin" },
      { "wkIdentity": "...", "role": "member" }
    ],
    "addressCount": 12,
    "status": "active",
    "createdAt": "2026-02-01T14:00:00Z"
  }
}
```

---

### Get vault balances

```
GET /v1/api/vaults/:vaultId/balances
```

Scope: `balances:read`

Returns the current balances for a vault.

```bash
curl https://relay.sspwallet.com/v1/api/vaults/70c1f2a3b4c5d6e7f8091a2b/balances \
  -H "Authorization: Bearer ssp_live_..."
```

```json
{
  "status": "success",
  "data": {
    "vaultId": "70c1f2a3b4c5d6e7f8091a2b",
    "chain": "btc",
    "balances": [
      {
        "asset": "BTC",
        "amount": "1.42500000",
        "decimals": 8,
        "fiatValue": "98250.00",
        "fiatCurrency": "USD"
      }
    ],
    "totalFiatValue": "98250.00",
    "asOf": "2026-06-24T10:00:00Z"
  }
}
```

---

### List vault transactions

```
GET /v1/api/vaults/:vaultId/transactions
```

Scope: `transactions:read`

Lists the transaction history for a vault, newest first. Paginated.

```bash
curl https://relay.sspwallet.com/v1/api/vaults/70c1f2a3b4c5d6e7f8091a2b/transactions?limit=50 \
  -H "Authorization: Bearer ssp_live_..."
```

```json
{
  "status": "success",
  "data": {
    "items": [
      {
        "txid": "9f8e7d6c5b4a39281706f5e4d3c2b1a0...",
        "chain": "btc",
        "direction": "outgoing",
        "amount": "0.50000000",
        "asset": "BTC",
        "fee": "0.00002100",
        "confirmations": 124,
        "timestamp": "2026-06-20T08:15:00Z",
        "recipients": [
          { "address": "bc1q...", "amount": "0.50000000" }
        ]
      }
    ],
    "pagination": { "limit": 50, "nextCursor": "eyJpZCI6Ii4uLiJ9", "hasMore": true }
  }
}
```

---

### List vault proposals

```
GET /v1/api/vaults/:vaultId/proposals
```

Scope: `proposals:read`

Lists transaction proposals for a vault, newest first. Paginated.

```bash
curl https://relay.sspwallet.com/v1/api/vaults/70c1f2a3b4c5d6e7f8091a2b/proposals?limit=20 \
  -H "Authorization: Bearer ssp_live_..."
```

```json
{
  "status": "success",
  "data": {
    "items": [
      {
        "id": "81d2e3f4a5b6c7d8e9f0a1b2",
        "vaultId": "70c1f2a3b4c5d6e7f8091a2b",
        "chain": "btc",
        "amount": "0.50000000",
        "asset": "BTC",
        "status": "pending_signatures",
        "requiredSignatures": 2,
        "currentSignatures": 1,
        "recipients": [
          { "address": "bc1q...", "amount": "0.50000000" }
        ],
        "createdAt": "2026-06-23T12:00:00Z",
        "expiresAt": "2026-06-30T12:00:00Z"
      }
    ],
    "pagination": { "limit": 20, "nextCursor": null, "hasMore": false }
  }
}
```

---

### Get a proposal

```
GET /v1/api/vaults/:vaultId/proposals/:proposalId
```

Scope: `proposals:read`

Returns full details for a single proposal, including which signers have signed.

```bash
curl https://relay.sspwallet.com/v1/api/vaults/70c1f2a3b4c5d6e7f8091a2b/proposals/81d2e3f4a5b6c7d8e9f0a1b2 \
  -H "Authorization: Bearer ssp_live_..."
```

```json
{
  "status": "success",
  "data": {
    "id": "81d2e3f4a5b6c7d8e9f0a1b2",
    "vaultId": "70c1f2a3b4c5d6e7f8091a2b",
    "chain": "btc",
    "amount": "0.50000000",
    "asset": "BTC",
    "status": "pending_signatures",
    "requiredSignatures": 2,
    "currentSignatures": 1,
    "signatures": [
      { "wkIdentity": "...", "signedAt": "2026-06-23T12:05:00Z" }
    ],
    "recipients": [
      { "address": "bc1q...", "amount": "0.50000000" }
    ],
    "proposedBy": "...",
    "createdAt": "2026-06-23T12:00:00Z",
    "expiresAt": "2026-06-30T12:00:00Z"
  }
}
```

> Proposal details contain only the metadata already visible to your team inside the app. They never include private keys, raw signatures, nonces, xpubs, or witness scripts.

---

### Get portfolio analytics

```
GET /v1/api/analytics/portfolio
```

Scope: `analytics:read`

Returns aggregated portfolio analytics across all vaults in your organization.

```bash
curl https://relay.sspwallet.com/v1/api/analytics/portfolio \
  -H "Authorization: Bearer ssp_live_..."
```

```json
{
  "status": "success",
  "data": {
    "totalFiatValue": "312450.00",
    "fiatCurrency": "USD",
    "vaultCount": 3,
    "byChain": [
      { "chain": "btc", "fiatValue": "98250.00" },
      { "chain": "eth", "fiatValue": "214200.00" }
    ],
    "asOf": "2026-06-24T10:00:00Z"
  }
}
```

---

### Read policy configuration

Read back the policy configuration that the `policies:write` endpoints manage — useful for auditing, drift detection, or syncing config into your own systems.

```
GET /v1/api/vaults/:vaultId/policy         # vault policy         (policies:read)
GET /v1/api/vaults/:vaultId/policy-rules   # vault policy rules   (policies:read)
GET /v1/api/org/policy                     # org policy           (policies:read)
GET /v1/api/org/policy-rules               # org policy rules     (policies:read)
GET /v1/api/approval-groups                # approval groups      (policies:read)
GET /v1/api/policy-templates               # policy template catalog (policies:read)
```

Scope: `policies:read`. Org-scoped (the key determines the organization); no signing material is ever returned. Each mirrors the shape written by the corresponding `policies:write` endpoint, so config round-trips.

```bash
curl https://relay.sspwallet.com/v1/api/vaults/VAULT_ID/policy \
  -H "Authorization: Bearer ssp_live_..."
```

---

### Create a proposal

```
POST /v1/api/vaults/:vaultId/proposals
```

Scope: `proposals:write` · Requires the **Pro plan or higher**.

Creates a **pending** transaction proposal on a vault — the same object the app creates when a signer proposes a payment. The proposal is attributed to the wkIdentity that created the API key, who **must be a signer or admin on the target vault**. It runs through your organization's full policy engine (spending limits, whitelists, approval workflows, IP/geo rules) exactly like an app-created proposal.

This endpoint **only creates a proposal**. It never signs, approves, or broadcasts. The returned proposal has `status: "pending"` and an empty `signatures` list; it executes only once your vault's M-of-N signers approve it in SSP Wallet + SSP Key. A write key can propose a payment, never send one.

Body:

| Field | Type | Notes |
|---|---|---|
| `recipients` | array (required) | One or more `{ "address": "...", "amount": "1.5" }`. `amount` is a decimal string in the chain's main unit. |
| `memo` | string | Optional note stored with the proposal. |
| `fee`, `feePerByte`, `gasPrice`, … | — | Optional chain-specific fee controls (same options as the app). |
| `tokenContract`, `tokenSymbol`, `tokenDecimals` | — | For ERC-20 transfers (EVM only); omit for native currency. |

The vault's chain is taken from the vault itself — you don't pass it.

```bash
curl -X POST https://relay.sspwallet.com/v1/api/vaults/VAULT_ID/proposals \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "recipients": [{ "address": "bc1q...", "amount": "0.25" }], "memo": "Vendor payment" }'
```

```json
{
  "status": "success",
  "data": {
    "success": true,
    "proposal": {
      "id": "665f...",
      "vaultId": "665a...",
      "chain": "btc",
      "status": "pending",
      "requiredSignatures": 2,
      "signatureCount": 0,
      "recipients": [{ "address": "bc1q...", "amount": "0.25" }],
      "createdAt": "2026-07-09T10:00:00Z"
    }
  }
}
```

If the key's creator isn't a vault signer, a policy blocks the transfer, funds are insufficient, or the payload is invalid, the response is still `HTTP 200` but carries `data.success: false` with an `errorCode` such as `NOT_SIGNER`, `POLICY_BLOCKED`, `INSUFFICIENT_FUNDS`, or `INVALID_RECIPIENTS`. Write calls are rate-limited to **30 requests per minute per key**.

---

### Cancel a proposal

```
POST /v1/api/vaults/:vaultId/proposals/:proposalId/cancel
```

Scope: `proposals:write`

Cancels a **pending** proposal — the same as withdrawing it in the app. The key's creator must be the proposer or a vault admin. Cancelling only sets the proposal's status to `cancelled`; it moves no funds and cannot cancel a proposal that has already been signed and broadcast. Returns the updated proposal.

```bash
curl -X POST https://relay.sspwallet.com/v1/api/vaults/VAULT_ID/proposals/PROPOSAL_ID/cancel \
  -H "Authorization: Bearer ssp_live_..."
```

---

### Create a vault

```
POST /v1/api/vaults
```

Scope: `vaults:write` · Requires the **Pro plan or higher**. Creates a new vault in your organization. The key's creator must be an organization **admin or owner**. A vault created here holds no funds until you fund it, and spending always requires its M-of-N device signatures — creating one never moves money. Returns the new vault (same redacted shape as the read API).

```bash
curl -X POST https://relay.sspwallet.com/v1/api/vaults \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "name": "Treasury", "description": "Main treasury vault" }'
```

### Update a vault

```
PUT /v1/api/vaults/:vaultId
```

Scope: `vaults:write`. Updates a vault's configuration (e.g. name, description, tags). Org admin/owner only. Returns the updated vault.

### Vault tags

Reusable tag definitions you can attach to vaults for organization and filtering.

```
POST   /v1/api/vault-tags          # create  (vaults:write)
PUT    /v1/api/vault-tags/:tagId   # update  (vaults:write)
DELETE /v1/api/vault-tags/:tagId   # delete  (vaults:write)
```

Scope: `vaults:write`. Org admin/owner only. Create takes `{ "name": "...", "description": "...", "color": "..." }` (color optional).

```bash
curl -X POST https://relay.sspwallet.com/v1/api/vault-tags \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "name": "Operations", "description": "Day-to-day operational vaults" }'
```

---

### Update a vault policy

```
PUT /v1/api/vaults/:vaultId/policy
```

Scope: `policies:write` · Requires the **Pro plan or higher**.

Updates the vault's spending policy — transaction/aggregate limits, per-token limits, whitelist mode, time-lock, approval overrides, and other policy sections. A policy is a *control*, not a fund movement: changing it never signs, approves, or broadcasts anything, and existing proposals still require the vault's M-of-N device signatures to execute. The key's creator must be an **admin** of the target vault (org owner/admin, or a vault-level admin); otherwise the call returns `INSUFFICIENT_ROLE`.

The request body contains only the policy sections you want to change; omitted sections are left unchanged. Each section is validated server-side (for example, a vault limit may not be looser than the org default). Returns the updated policy.

```bash
curl -X PUT https://relay.sspwallet.com/v1/api/vaults/VAULT_ID/policy \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{
    "aggregateLimits": { "perTransaction": { "usd": 5000 } }
  }'
```

---

### Policy rules

Policy rules are the programmable, ordered side of a vault's spending policy — each rule matches conditions on a proposal and applies an action (allow, block, require approval, time-lock). This is the "policy-as-code" surface: define your controls in your own systems and push them via the API.

```
POST   /v1/api/vaults/:vaultId/policy-rules          # create   (policies:write)
PUT    /v1/api/vaults/:vaultId/policy-rules/:ruleId  # update   (policies:write)
DELETE /v1/api/vaults/:vaultId/policy-rules/:ruleId  # delete   (policies:write)
POST   /v1/api/vaults/:vaultId/policy-rules/reorder  # reorder  (policies:write)
```

Scope: `policies:write` · Requires the **Pro plan or higher**. Rules are a *control*: they gate or route proposals, they never sign or move funds. The key's creator must be an **admin** of the target vault (org owner/admin, or a vault-level admin); otherwise the call returns `INSUFFICIENT_ROLE`. Each rule is validated server-side.

Reorder takes the full ordered list of rule ids: `{ "ruleIds": ["rule_c", "rule_a", "rule_b"] }`.

```bash
# Create a rule
curl -X POST https://relay.sspwallet.com/v1/api/vaults/VAULT_ID/policy-rules \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{
    "name": "High-value approval",
    "conditions": [{ "field": "amountUsd", "operator": "gte", "value": 10000 }],
    "action": { "type": "require_approval" }
  }'

# Reorder rules (priority = array order)
curl -X POST https://relay.sspwallet.com/v1/api/vaults/VAULT_ID/policy-rules/reorder \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "ruleIds": ["RULE_A", "RULE_B"] }'
```

---

### Vault whitelist

A vault's allowed-destination list and its enforcement mode. The whitelist is a control on **where** funds may go; editing it never signs or moves funds.

```
PUT    /v1/api/vaults/:vaultId/policy/whitelist/mode  # set mode   (policies:write)
POST   /v1/api/vaults/:vaultId/policy/whitelist        # add        (policies:write)
DELETE /v1/api/vaults/:vaultId/policy/whitelist        # remove     (policies:write)
```

Scope: `policies:write`. The key's creator must be an admin of the target vault. Mode takes `{ "mode": "disabled" | "warn" | "enforce" }`. Add takes `{ "address": "...", "label": "...", "chain": "..." }` (label/chain optional). Remove takes `{ "address": "..." }`. Each returns the updated policy.

```bash
curl -X POST https://relay.sspwallet.com/v1/api/vaults/VAULT_ID/policy/whitelist \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "address": "0xRECIPIENT", "label": "Treasury", "chain": "eth" }'
```

---

### Update the organization policy

```
PUT /v1/api/org/policy
```

Scope: `policies:write` · Requires the **Pro plan or higher**. Updates the organization-wide default policy that every vault inherits — individual vaults may only make their policy **stricter**, never looser. The key's creator must be an organization **owner or admin**; otherwise the call returns `INSUFFICIENT_ROLE`. Body contains only the sections you want to change; returns the updated org policy.

```bash
curl -X PUT https://relay.sspwallet.com/v1/api/org/policy \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "aggregateLimits": { "perTransaction": { "usd": 10000 } } }'
```

---

### Organization policy rules

The org-wide rule set every vault inherits — same shape as vault policy rules, but applied at the organization level.

```
POST   /v1/api/org/policy-rules          # create   (policies:write)
PUT    /v1/api/org/policy-rules/:ruleId  # update   (policies:write)
DELETE /v1/api/org/policy-rules/:ruleId  # delete   (policies:write)
POST   /v1/api/org/policy-rules/reorder  # reorder  (policies:write)
```

Scope: `policies:write`. The key's creator must be an organization **owner or admin**. Bodies match the vault policy-rule endpoints; reorder takes `{ "ruleIds": [...] }`.

---

### Organization per-chain policy

A stricter policy override for a specific chain (for example, tighter BTC limits than your org default).

```
PUT    /v1/api/org/chain-policy/:chain  # set/update  (policies:write)
DELETE /v1/api/org/chain-policy/:chain  # remove      (policies:write)
```

Scope: `policies:write`. Org owner/admin only. `:chain` is the chain identifier (e.g. `btc`, `eth`). Returns the updated org policy.

```bash
curl -X PUT https://relay.sspwallet.com/v1/api/org/chain-policy/btc \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "aggregateLimits": { "perTransaction": { "usd": 2000 } } }'
```

---

### Approval groups

Named signer quorums (e.g. "Finance — 2 of 3") that policy rules reference to require multi-party approval. Managing a group defines **who** must approve; it never signs or moves funds.

```
POST   /v1/api/approval-groups            # create  (policies:write)
PUT    /v1/api/approval-groups/:groupId   # update  (policies:write)
DELETE /v1/api/approval-groups/:groupId   # delete  (policies:write)
```

Scope: `policies:write`. The key's creator must be an organization **owner or admin**. Body: `{ "name": "...", "description": "...", "members": ["wkIdentity", ...], "requiredApprovals": 2 }` (description optional). Returns the created/updated group. (Listing groups will arrive with a future `policies:read` scope.)

```bash
curl -X POST https://relay.sspwallet.com/v1/api/approval-groups \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "name": "Finance", "members": ["WK_A", "WK_B", "WK_C"], "requiredApprovals": 2 }'
```

---

### Apply a policy template

```
POST /v1/api/vaults/:vaultId/apply-template
```

Scope: `policies:write` · Requires the **Pro plan or higher**. Materializes a built-in policy template (a curated set of rules, and optionally some flat policy fields) onto a vault in one call. The key's creator must be an **admin** of the vault. Body: `{ "templateId": "..." }`. Returns the created rules and any flat fields applied.

```bash
curl -X POST https://relay.sspwallet.com/v1/api/vaults/VAULT_ID/apply-template \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "templateId": "tmpl_high_value_approval" }'
```

---

### Contacts (address book)

Your organization's saved recipient address book. Contacts are a convenience/labelling layer only — they are **not** whitelists and have no effect on policy enforcement or which addresses a proposal may pay. `contacts:read` lists them; `contacts:write` creates, updates, and deletes them.

```
GET    /v1/api/contacts        # list    (contacts:read)
POST   /v1/api/contacts        # create  (contacts:write)
PUT    /v1/api/contacts/:id    # update  (contacts:write)
DELETE /v1/api/contacts/:id    # delete  (contacts:write)
```

The list accepts optional `chain`, `search`, `limit`, and `skip` query parameters. Create body: `{ "chain": "btc", "name": "Vendor Inc", "address": "bc1q...", "notes": "optional" }`; update accepts any of `name`, `address`, `notes`.

```bash
# List
curl "https://relay.sspwallet.com/v1/api/contacts?chain=btc" \
  -H "Authorization: Bearer ssp_live_..."

# Create
curl -X POST https://relay.sspwallet.com/v1/api/contacts \
  -H "Authorization: Bearer ssp_live_..." \
  -H "Content-Type: application/json" \
  -d '{ "chain": "btc", "name": "Vendor Inc", "address": "bc1q..." }'
```

## Errors

Errors use the standard SSP envelope, with a machine-readable `name` and a human-readable `message`:

```json
{
  "status": "error",
  "data": {
    "code": 403,
    "name": "FORBIDDEN",
    "message": "API key is missing the required scope: vaults:read"
  }
}
```

| HTTP status | `name` | Meaning |
|---|---|---|
| `400` | `BAD_REQUEST` | Malformed request — for example an invalid `cursor` or `limit`. |
| `401` | `UNAUTHORIZED` | Missing, malformed, revoked, or expired API key. |
| `403` | `FORBIDDEN` | The key is valid but lacks the scope this endpoint requires. |
| `404` | `NOT_FOUND` | The requested vault or proposal doesn't exist **in your organization**. (A key can only ever see its own organization, so resources in other organizations also appear as `404`.) |
| `429` | `RATE_LIMITED` | You've exceeded the rate limit — back off and retry. |
| `500` | `INTERNAL_ERROR` | Something went wrong on our side. |

## Frequently asked

**Can I create or sign a transaction with an API key?**
A key with the `proposals:write` scope can **create a pending proposal**, but it can never **sign** one. Signing — and therefore actually moving funds — always happens in SSP Wallet + SSP Key with your vault's M-of-N devices. A read-only key can't even create a proposal. So no API key can move funds or change a setting; at most a write key can queue a payment for your signers to approve.

**Can one key read multiple organizations?**
No. A key is bound to a single organization, derived from the key itself. To read another organization, create a key from inside that organization.

**I lost my key — can I recover it?**
No. Keys are shown once and only a hash is stored. Revoke the lost key and create a new one.

**Who can create keys?**
Only owners and admins of the organization, from **Settings → Developers → API Keys**.
