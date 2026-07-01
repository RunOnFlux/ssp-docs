---
icon: code
---

# Enterprise API

The SSP Enterprise API gives your own systems programmatic, **read-only** access to your organization's vaults, balances, transactions, proposals, and portfolio analytics. Use it to feed an internal dashboard, reconcile balances, monitor proposal status, or export data into your own tooling — without logging into the app.

> **The API is strictly read-only.** Every endpoint is a `GET`. There is **no** way to create a proposal, sign a transaction, move funds, or change any organization setting through an API key. Money can only ever move through the normal two-party signing flow in SSP Wallet and SSP Key. This is the core safety property of the API, enforced by construction.

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

Keep your keys secret. Treat `ssp_live_...` like a password: never commit it to source control, embed it in a frontend bundle, or paste it into a browser. A leaked key can read one organization's data, but — because the API is read-only — it can never move funds or change anything.

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

All scopes are read-only. There is no scope that grants the ability to write, sign, or move funds.

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
        "expiresAt": "2026-06-24T12:00:00Z"
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
    "expiresAt": "2026-06-24T12:00:00Z"
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
No. The API is read-only by construction. Transactions are proposed in the app and signed with your SSP Wallet and SSP Key. No API key can move funds or change any setting.

**Can one key read multiple organizations?**
No. A key is bound to a single organization, derived from the key itself. To read another organization, create a key from inside that organization.

**I lost my key — can I recover it?**
No. Keys are shown once and only a hash is stored. Revoke the lost key and create a new one.

**Who can create keys?**
Only owners and admins of the organization, from **Settings → Developers → API Keys**.
