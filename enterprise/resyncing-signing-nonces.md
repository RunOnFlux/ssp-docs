---
icon: arrows-rotate
---

# Re-syncing Enterprise Signing Nonces

{% hint style="info" %}
**Signing nonces are one-time cryptographic values your devices use to co-sign EVM vault transactions — not funds.** Re-syncing them simply generates a fresh batch on your SSP Wallet and SSP Key. **No funds are moved.**
{% endhint %}

For EVM vaults, SSP pre-generates a pool of single-use signing nonces on **both** your SSP Wallet (browser extension) and your SSP Key (phone). When you replace a device, that device's nonces are gone, and the pool falls out of sync — so you re-generate them with one click.

## When you need this

* You **got a new phone** and restored your SSP Key on it.
* You **reinstalled or restored the SSP Wallet** extension on a new browser or computer.
* You see a **"Nonce Sync Required"** message, or a nonce error, when creating or signing a vault proposal.

{% hint style="info" %}
This only applies to **EVM vaults** (Ethereum and other EVM chains). **Bitcoin / UTXO** and **Solana** vaults don't use these nonces, so you never need to sync nonces for them.
{% endhint %}

## Before you start

* Your **SSP Wallet** browser extension is **installed, connected, and unlocked** — the Sync Nonces button works through the extension, so it must be open and unlocked.
* Your **SSP Key** phone app is restored and already re-paired with your wallet. If you just got a new phone, do that first: [Got a New Phone? Re-sync SSP Key with SSP Wallet](../troubleshooting/resync-ssp-key-new-phone.md).
* **Both devices online.**

## How to sync your nonces

1. In **SSP Enterprise**, open your **Profile**.
2. Find the **Enterprise Signing Nonces** card and click **Sync Nonces**.
3. The **SSP Wallet** extension opens an **Enterprise Nonce Synchronisation** prompt. Click **Sync Nonces** there to confirm. (It reminds you: *"No funds will be moved."*)
4. The wallet regenerates its own nonces (*"Synchronising wallet nonces…"*), then waits for your phone (*"Waiting for SSP Key to sync nonces…"*).
5. **On your phone, open SSP Key.** A **Nonce Sync Request** appears — tap **Approve Request** and confirm with your password or biometrics.
6. Done. The wallet shows **"Wallet and Key nonces synchronised successfully!"** and SSP Enterprise confirms the sync.

{% hint style="success" %}
**One click syncs both devices.** Pressing **Sync Nonces** refreshes the nonces on your SSP Wallet **and** triggers the matching refresh on your SSP Key — you don't sync them separately.
{% endhint %}

## If a proposal says "Nonce Sync Required"

If signing a vault transaction fails because nonces are out of sync, the proposal page shows a **Nonce Sync Required** notice with a **Sync Nonces** button right there. Click it, approve on your phone (as above), then sign the proposal again.

## Good to know

* **No funds move.** Syncing only regenerates signing material; it never touches balances or sends a transaction.
* **The SSP Wallet extension must be installed and unlocked.** If it isn't detected you'll get a "wallet not detected" message; if it's locked, unlock it and try again.
* **A proposal created before you re-synced may need to be recreated.** If one specific pending proposal still can't be signed after syncing (e.g. *"No reserved nonces found for your identity. This proposal may need to be recreated."*), cancel that proposal, create it again, and sign the new one. This affects only that pending proposal — never your funds.
* **Nonces are single-use and managed for you.** The apps generate and consume them automatically; you never copy, reuse, or share nonce values yourself.

## Related pages

* [Got a New Phone? Re-sync SSP Key with SSP Wallet](../troubleshooting/resync-ssp-key-new-phone.md)
* [Lost or Replaced a Device?](../troubleshooting/lost-or-replaced-device.md)
* [Proposing & Signing Transactions](transactions.md)
* [Common Issues & Solutions](../troubleshooting/common-issues.md)
