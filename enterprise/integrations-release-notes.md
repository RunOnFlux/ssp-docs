---
icon: rocket
---

# Integrations Release Notes (Notifications + API)

This release adds the **Integrations** capability to SSP Enterprise: per-user email
notification preferences, Slack and signed-webhook delivery of vault-proposal events,
and a read-only **Enterprise API** with scoped API keys. It also changes which
proposal emails free-tier organizations receive. Read the
[Behavior change](#behavior-change-free-tier-proposal-emails) section before upgrading.

## What's new

### Per-user email notification preferences

Each member can now control which proposal emails they receive from
**Settings → Profile → Notifications**. Categories include proposal created,
executed, rejected, failed, and expired. The **action-required** category
(`proposal.needs_signature` — "a signer still needs to sign") is treated specially:
it is always delivered so a pending signature is never silently dropped.

### Slack notifications

Organizations on a qualifying plan can connect a Slack **incoming webhook** under
**Settings → Developers → Notifications**. Proposal lifecycle events are posted to
the chosen channel. The Slack URL is itself a secret and is stored encrypted; it is
never shown again after creation and never returned by the admin dashboard.

### Signed webhooks

Organizations can register an HTTPS endpoint and receive **signed** webhook
deliveries for the event types they select. Each delivery carries an `SSP-Signature`
header computed from a signing secret shown **once** at creation. Payloads are
metadata-only — they never contain keys, signatures, nonces, xpubs, or witness
scripts. See [Webhook Notifications](webhooks.md) for the signing scheme and
verification examples.

### Read-only Enterprise API + API keys

A new **read-only** API exposes orgs, vaults, balances, proposals, transactions, and
portfolio analytics under `https://relay.sspwallet.com/v1/api`. Keys are minted,
listed, and revoked from **Settings → Developers → API Keys**, and the full secret is
shown only once at creation. Every endpoint is a `GET`; there is no way to move funds
or change settings through an API key. See the [Enterprise API](api.md) reference.

## Entitlement: Integrations (Pro and above)

Slack, webhooks, and the Enterprise API are gated by the **`integrations`**
entitlement.

| Tier | Integrations |
|---|---|
| Free | Off |
| Starter | Off |
| Pro | **On** |
| Business | **On** |

The entitlement is enforced both when a key or subscription is **created** and when a
key is **used** / a notification is **dispatched**, so access tracks the org's current
plan. If an organization downgrades below Pro, its existing keys and outbound
subscriptions stop working.

## Behavior change: free-tier proposal emails

Previously, proposal emails were sent to every signer and member regardless of plan.
With this release, **non-action-required proposal emails (created, executed, rejected,
failed, expired) require the org's `emailNotifications` entitlement.**

* **Free-tier organizations will stop receiving** the informational proposal emails
  (created / executed / rejected / failed / expired).
* **Action-required emails stay free.** `proposal.needs_signature` is never gated — a
  signer is always emailed when their signature is needed, on every tier.

Communicate this to free-tier customers before rollout so the drop in informational
emails is expected. Upgrading to Pro (or above) restores the full set, and individual
members can further tune categories from their profile.

## Required ops step: `NOTIFICATION_ENC_KEY`

Slack URLs and webhook signing secrets are stored **encrypted at rest** (AES-256-GCM).
The relay-enterprise service requires the following environment variable to be set in
every environment that runs notification dispatch:

```
NOTIFICATION_ENC_KEY=<64 hex chars = 32 random bytes>
```

* The value must be a **stable 32-byte key**, hex-encoded (64 characters). Generate one
  with `openssl rand -hex 32`.
* **Do not lose or change this key.** Rotating `NOTIFICATION_ENC_KEY` makes every
  previously stored Slack URL and webhook signing secret undecryptable: affected
  subscriptions will fail delivery and eventually auto-disable. Treat rotation as a
  migration that re-encrypts existing secrets, not a hot swap.
* Keep it out of source control and managed as a secret alongside the database
  credentials.

## Admin dashboard

The admin dashboard gains two views under **Enterprise → Integrations**:

* **Notification deliveries / subscriptions** — delivery health, top failing
  subscriptions, retry a delivery, and globally enable/disable a subscription.
  Global enable/disable overrides are recorded in the organization audit trail.
* **API keys** — per-org keys with status, scopes, last-used time, and best-effort
  request counts; a single global revoke action. Stored secret material (key hashes,
  encrypted webhook/Slack secrets) is never returned to the dashboard client.

## Next steps

* **[Webhook Notifications](webhooks.md)** — signing scheme, event types, verification
* **[Enterprise API](api.md)** — endpoints, authentication, scopes
</content>
</invoke>
