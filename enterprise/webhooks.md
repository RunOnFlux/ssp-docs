---
icon: bell
---

# Webhook Notifications

SSP Enterprise can deliver vault-proposal lifecycle events to your own systems as **signed HTTP webhooks**. Use them to drive Slack/PagerDuty alerts, kick off CI pipelines, update an internal dashboard, or trigger your own approval reminders — without polling the app.

A webhook subscription is configured per organization under **Settings → Developers → Webhooks**. You provide an HTTPS URL and pick which event types you care about; SSP generates a **signing secret** (shown once) that you use to verify every delivery is genuinely from SSP and unmodified.

> **Webhook payloads never contain key material.** They carry only proposal metadata that is already visible to your team inside the app — vault name, chain, amount, recipient addresses, signature counts. No private keys, no signatures, no nonces, no xpubs, no witness scripts.

## Event types

Each subscription is fired for the event types you select:

| Event type | When it fires |
|---|---|
| `proposal.created` | A new transaction proposal is created in a vault |
| `proposal.needs_signature` | A designated signer still needs to sign (sent to those not yet signed) |
| `proposal.executed` | The threshold was met and the transaction was broadcast |
| `proposal.rejected` | The proposal was rejected and can no longer reach its threshold |
| `proposal.failed` | Broadcast or execution failed |
| `proposal.expired` | The proposal expired before reaching its threshold |

You can also scope a subscription to specific vaults, or leave it unscoped to receive events for every vault in the organization.

## The event envelope

Every delivery is an HTTP `POST` with a JSON body in this shape:

```json
{
  "id": "evt_a1b2c3d4e5f6",
  "type": "proposal.created",
  "createdAt": "2026-06-23T12:00:00Z",
  "organizationId": "...",
  "data": {
    "vaultId": "...",
    "vaultName": "Treasury — BTC",
    "proposalId": "...",
    "chain": "btc",
    "amount": "0.5 BTC",
    "status": "pending_signatures",
    "requiredSignatures": 2,
    "currentSignatures": 0,
    "recipients": [
      { "address": "bc1q...", "amount": "0.5 BTC" }
    ]
  }
}
```

The exact fields inside `data` vary slightly by event type (for example, `proposal.executed` includes the broadcast transaction id), but the top-level envelope — `id`, `type`, `createdAt`, `organizationId`, `data` — is always present.

## Verifying the signature

Every request carries a signature header so you can confirm it came from SSP and was not tampered with in transit:

```
SSP-Signature: t=<unixSeconds>,v1=<hexHmac>
SSP-Event-Id: evt_a1b2c3d4e5f6
SSP-Delivery-Attempt: 1
```

The `v1` value is computed as:

```
v1 = HMAC-SHA256(signingSecret, `${t}.${rawBody}`)
```

where:

* `signingSecret` is the secret SSP showed you once when you created the subscription,
* `t` is the `t=` value from the `SSP-Signature` header (Unix seconds),
* `rawBody` is the **exact raw request body bytes** — verify before any JSON parsing or re-serialization, since whitespace and key order changes will break the HMAC.

To verify a request:

1. Parse `t` and `v1` out of the `SSP-Signature` header.
2. Recompute `HMAC-SHA256(signingSecret, "${t}.${rawBody}")` and compare it to `v1` using a **constant-time** comparison.
3. Reject the request if `|now - t| > 5 minutes` (replay-tolerance window). This stops an attacker from re-playing an old, validly-signed request.

### Node.js

```js
const crypto = require('crypto');

// Mount this with a raw-body parser, e.g. express.raw({ type: 'application/json' }),
// so `rawBody` is the unmodified bytes SSP signed.
function verifySspWebhook(rawBody, signatureHeader, signingSecret) {
  const parts = Object.fromEntries(
    signatureHeader.split(',').map((kv) => kv.split('='))
  );
  const t = Number(parts.t);
  const v1 = parts.v1;
  if (!t || !v1) return false;

  // Replay tolerance: reject if the timestamp is more than 5 minutes off.
  const nowSecs = Math.floor(Date.now() / 1000);
  if (Math.abs(nowSecs - t) > 5 * 60) return false;

  const expected = crypto
    .createHmac('sha256', signingSecret)
    .update(`${t}.${rawBody}`) // rawBody is a string/Buffer of the exact bytes
    .digest('hex');

  // Constant-time compare.
  const a = Buffer.from(expected, 'hex');
  const b = Buffer.from(v1, 'hex');
  return a.length === b.length && crypto.timingSafeEqual(a, b);
}
```

### Python

```python
import hashlib
import hmac
import time


def verify_ssp_webhook(raw_body: bytes, signature_header: str, signing_secret: str) -> bool:
    parts = dict(kv.split("=", 1) for kv in signature_header.split(","))
    try:
        t = int(parts["t"])
        v1 = parts["v1"]
    except (KeyError, ValueError):
        return False

    # Replay tolerance: reject if the timestamp is more than 5 minutes off.
    if abs(int(time.time()) - t) > 5 * 60:
        return False

    signed_payload = f"{t}.".encode() + raw_body  # raw_body must be the exact bytes
    expected = hmac.new(
        signing_secret.encode(), signed_payload, hashlib.sha256
    ).hexdigest()

    return hmac.compare_digest(expected, v1)
```

> **Use the raw body, not a re-serialized object.** Frameworks that auto-parse JSON will not give you back byte-identical input. Capture the raw bytes before parsing (for example, `express.raw()` in Express, or `request.body` in Flask) and verify against those.

## Delivery, retries, and idempotency

* **Return `2xx` quickly.** SSP treats any `2xx` response as a successful delivery. Acknowledge first, then do slow work asynchronously — your endpoint should respond well within a few seconds.
* **Retries use exponential backoff.** A non-`2xx` response, a timeout, or a connection error is retried with increasing delays. The current attempt number is in the `SSP-Delivery-Attempt` header (starts at `1`).
* **Deliveries are idempotent — deduplicate on `SSP-Event-Id`.** A retry reuses the same `SSP-Event-Id` (also the top-level `id` in the body). Because retries can arrive after a delivery your server actually processed (but failed to acknowledge in time), treat `SSP-Event-Id` as an idempotency key and ignore IDs you have already handled.
* **Redirects are never followed.** SSP posts directly to the URL you registered. A `3xx` response is treated as a failed delivery, not a redirect to follow — register the final URL.
* **Subscriptions auto-disable after sustained failure.** If an endpoint keeps failing across many consecutive deliveries, SSP automatically disables the subscription to stop hammering a dead endpoint. You'll see it as **Auto-disabled** in **Settings → Developers → Webhooks**; fix the endpoint and re-enable it there. The per-subscription **Delivery Log** shows recent attempts, HTTP status, and error reason to help you debug.

## Testing a subscription

After creating a subscription, use **Send test** in **Settings → Developers → Webhooks** to deliver a synthetic event to your endpoint. The test payload is signed with the same scheme as real events, so it's the quickest way to validate your verification code end to end.

## Security notes

* **HTTPS only.** Webhook URLs must be `https://`. Plaintext `http://` endpoints are rejected.
* **Keep the signing secret secret.** It is shown only once at creation. If it leaks, delete the subscription and create a new one — there is no way to recover a lost secret, and rotating means creating a fresh subscription.
* **Always verify before trusting.** Anyone who learns your URL can `POST` to it; only requests with a valid `SSP-Signature` (and a fresh timestamp) are genuinely from SSP.
* **Payloads are metadata-only.** They contain nothing that isn't already visible to your team in the app, so a webhook endpoint never becomes a path to keys or signing material.

## Next steps

* **[Proposing & Signing Transactions](transactions.md)** — the proposal lifecycle that drives these events
* **[Configuring Policy Controls](policies.md)** — guardrails that may add `proposal.needs_signature` (admin approval) steps
</content>
</invoke>
