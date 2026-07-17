---
icon: arrows-rotate
---

# Got a New Phone? Re-sync SSP Key with SSP Wallet

{% hint style="info" %}
**Your funds are safe and nothing needs to be recreated.** Your coins live on the blockchain, not on your phone. As long as you have your **SSP Key seed phrase**, you can install SSP Key on the new phone, restore it, and re-pair it with your existing SSP Wallet. Your addresses and balances stay exactly the same.
{% endhint %}

This guide is for the common case: **you replaced your phone** (lost, broken, or upgraded), you still use the **same SSP Wallet browser extension** on your computer, and you have your **SSP Key recovery seed phrase** backed up. By the end, your new phone will sign transactions again, just like the old one.

If you have lost a seed phrase, or also need to restore the browser wallet, start with [Lost or Replaced a Device?](lost-or-replaced-device.md) instead.

## Before you start

You will need:

* ✅ Your backed-up **SSP Key seed phrase** — the mobile key's own phrase. **This is different from your SSP Wallet seed phrase.**
* ✅ Your **existing SSP Wallet** browser extension, installed and unlockable, with your **wallet password**.
* ✅ **Both devices online** — re-pairing is coordinated through the SSP Relay over the internet.
* ✅ **Camera access** on the new phone (to scan the wallet's QR code).
* ✅ A list of **which chains you use** (Bitcoin, Ethereum, Solana, Flux, etc.) — re-syncing is done **per chain**.

## How this works (and why it's safe)

SSP is a **2-of-2 multisignature** wallet: the **SSP Wallet** browser extension holds the first key and the **SSP Key** mobile app holds the second. Every address is derived deterministically from **both** keys' public keys.

Three things make a new-phone re-sync safe and simple:

* **Same seed → same key → same addresses.** Restoring SSP Key from the same seed phrase reproduces the identical key, so all your multisig addresses — and the funds on them — are unchanged.
* **Your wallet already knows your key.** The browser extension still has your SSP Key's public key stored, so your balances are visible the whole time and the wallet **does not need to be restored or recreated**.
* **Re-pairing only re-shares public data.** The handshake exchanges public keys (never seed phrases or private keys) so the new phone can rebuild the same multisig and sign again.

{% hint style="warning" %}
Because the wallet already has your key on file, it **will not pop up a sync prompt by itself** after you restore the phone. You re-share the sync QR manually from the wallet's Menu tab — see Step 2. This is expected, not a sign that something is wrong.
{% endhint %}

## Step 1 — Install and restore SSP Key on the new phone

1. Install **SSP Key** from the official store and open it:
   * iOS: [App Store](https://apps.apple.com/us/app/ssp-key/id6463717332)
   * Android: [Google Play](https://play.google.com/store/apps/details?id=io.runonflux.sspkey)
2. On the welcome screen, tap **Restore Key**. (Do **not** tap "Synchronise Key!" — that generates a brand-new key.)
3. In **Input your Mnemonic Key Seed Phrase**, enter your backed-up **SSP Key** seed phrase, then set and confirm a new app password under **Set SSP Key Password** / **Confirm Key Password**. Tap **Import Key**.
4. Tap **Show Mnemonic Key Seed Phrase** to reveal the phrase, confirm your backup is correct, toggle **I have backed up my keys seed phrase in a secure location.**, then tap **Setup Key!**

{% hint style="danger" %}
**Restore with the SSP Key seed phrase — never the SSP Wallet seed phrase.** The two are different. Entering the wrong phrase produces a different key, the addresses won't match your wallet, and the new phone won't be able to co-sign. (Your funds are not lost — they stay under the original keys — but you'd need to restore again with the correct SSP Key phrase.)
{% endhint %}

After setup, SSP Key opens the **Home** screen and automatically shows a **Synchronisation Needed** prompt — that's your cue to move to Step 2.

For the full screenshot-by-screenshot version of this restore flow, see [Restore Key Using Seed Phrase and Syncing to SSP Wallet](../getting-started-with-ssp-key/restore-key-using-seed-phrase-and-syncing-to-ssp-wallet.md).

## Step 2 — Re-pair with your SSP Wallet (Bitcoin / identity)

This first pairing restores your core identity and your Bitcoin chain.

1. On your computer, open the **SSP Wallet** browser extension and **unlock** it with your wallet password.
2. Open the **Menu** tab (the ☰ icon in the bottom bar) and click **SSP Wallet Details**.
3. In **Confirm with Password**, enter your wallet password and click **Grant Access**.
4. Scroll to the **SSP Sync with SSP Key** section and click the **eye / unhide icon** to reveal its **QR code**. (This section also syncs your Bitcoin chain automatically.)
5. On the new phone, in the **Synchronisation Needed** prompt, tap **Scan Code** and scan the QR shown in the wallet. (Grant camera permission if asked.)
6. SSP Key shows a **Synchronisation Request** — **slide to approve**, then confirm with your key password or biometrics.
7. Both devices then display the same **6-word verification code** — scan the wallet's verification QR with the Key (**Scan wallet to verify**) or compare the words. **They must match**; if they differ, stop and re-pair. See [Pairing Verification Code](../getting-started-with-ssp-key/pairing-verification-code.md).

{% hint style="info" %}
**Can't scan the QR?** In SSP Key tap **Manual Input** instead and paste the wallet's extended public key. Make sure both the phone and the extension have an internet connection — re-pairing is coordinated through the SSP Relay.
{% endhint %}

## Step 3 — Re-sync your other chains

{% hint style="warning" %}
**Re-syncing is per chain.** Step 2 restores your identity and **Bitcoin** only. Every **other** chain you use (each EVM chain, Solana, Flux, other UTXO coins, etc.) must be re-synced individually, or that chain won't be signable on the new phone.
{% endhint %}

For **each** additional chain you use:

1. In SSP Wallet, switch the **active chain** (tap the wallet & chain pill and pick the chain in the Networks section).
2. With both apps on v2, the wallet can send a **one-tap Chain Activation Request** straight to the phone — review and slide to approve on the Key. Otherwise, open **Menu → SSP Wallet Details** and reveal the **`<chain>` Sync with SSP Key** section to show that chain's QR code.
3. If using the QR: on the phone, tap **Scan Code**, scan it, and slide to approve.
4. Compare the **verification code** shown on both devices after each sync.

Repeat until every chain you actively use has been re-synced and approved.

## Step 4 — Verify everything matches

1. After each approval, SSP Key shows **Synchronisation Successful!** with the generated address. **Confirm this address matches the one shown for the same chain in your SSP Wallet.** A match means the correct key was restored and paired.
2. Back in SSP Wallet, confirm your balances are visible (they should never have disappeared).
3. As a final check, **send a small test transaction** and approve it on the new phone. If it signs and broadcasts, your 2-of-2 is fully restored.

{% hint style="success" %}
Once verified, you're done — the new phone behaves exactly like the old one. There is nothing to migrate and no new addresses to share with anyone.
{% endhint %}

## Troubleshooting

| Symptom | What it means / what to do |
| --- | --- |
| The wallet never prompted me to re-sync | Expected. The wallet already has your key stored, so it won't prompt. Re-share the QR manually via **Menu → SSP Wallet Details** (Step 2). |
| One chain works but another says it needs syncing or can't sign | You haven't re-synced that chain yet. Repeat Step 3 for it. Re-sync is **per chain**. |
| The address on **Synchronisation Successful!** doesn't match the wallet | You restored SSP Key with the **wrong seed** (often the wallet seed instead of the key seed). Restore SSP Key again using the correct **SSP Key** phrase. Funds are not lost. |
| New phone doesn't receive transaction requests / no push notifications | Re-pairing re-registers the phone for push. Make sure notifications are allowed for SSP Key and reopen the app to reconnect; requests sent while the app was fully closed may not have pushed. |
| "Synchronisation failed. Try again later." | One of the devices is offline, or the pairing window timed out. Put both devices online and re-reveal the QR — the sync data is short-lived, so don't leave it sitting between scanning and approving. |
| The QR code won't scan | Grant camera permission (iOS: **Settings → Apps → SSP Key → Camera**), improve lighting, or use **Manual Input** to paste the extended public key. |

## SSP Enterprise vaults

If you sign for **SSP Enterprise** vaults, re-pairing above restores your normal signing. For EVM vaults you also need to refresh your signing nonces on the new phone — open **SSP Enterprise → Profile → Sync Nonces** and approve the **Nonce Sync Request** that appears in SSP Key (it moves **no funds**). Full steps: [Re-syncing Enterprise Signing Nonces](../enterprise/resyncing-signing-nonces.md).

Note that a vault's **signer set is fixed at creation**: restoring your own devices from your own seeds returns your access automatically, but you cannot swap one signer for another on an existing vault. See the [enterprise section of Lost or Replaced a Device?](lost-or-replaced-device.md#enterprise-replacing-a-vault-signer) for details.

## Related pages

* [Lost or Replaced a Device?](lost-or-replaced-device.md)
* [Restore Key Using Seed Phrase and Syncing to SSP Wallet](../getting-started-with-ssp-key/restore-key-using-seed-phrase-and-syncing-to-ssp-wallet.md)
* [Retrieving Your SSP Key Seed Phrase](../getting-started-with-ssp-key/retrieving-your-ssp-key-seed-phrase.md)
* [Common Issues & Solutions](common-issues.md)
* [Security Overview](../security/security-overview.md)
