---
icon: mobile-screen-button
---

# Lost or Replaced a Device?

{% hint style="info" %}
**Your funds live on the blockchain, not on your devices.** Your devices hold the keys that authorize spending. As long as you have your **seed phrases** backed up, you can restore your keys on a new device and keep using your funds.
{% endhint %}

## How SSP's two-device model works

SSP is a **true 2-of-2 multisignature** system. There are two independent keys:

* **SSP Wallet** — the browser extension. Holds the **first** key.
* **SSP Key** — the mobile app. Holds the **second** key.

Every transaction needs **both** keys to sign. This is the security guarantee: a single lost, stolen, or compromised device **cannot move your funds on its own**.

Each device has its **own seed phrase**:

* The **wallet seed phrase** restores the SSP Wallet (first) key.
* The **key seed phrase** restores the SSP Key (second) key.

> **The two seed phrases are different and equally important.** Back up both, store them separately, and never store them on the same device or in the same place. You need both to fully restore your wallet.

## What is — and isn't — recoverable

**The rule:** every transaction needs **both** keys. Each key can be provided either by its **working device** *or* by restoring it from **its own seed phrase**. So a key is safe only if it has a seed backup — and if either key is lost with **no** backup (its device *and* its seed both gone), the 2-of-2 can no longer sign and the funds are frozen. In short: **keep a seed-phrase backup for _both_ keys.**

| Situation | Recoverable? | Notes |
| --- | --- | --- |
| Lost / broke your **phone** (SSP Key) | ✅ Yes | Restore from your **key** seed phrase |
| Lost / reset your **browser / computer** (SSP Wallet) | ✅ Yes | Restore from your **wallet** seed phrase |
| Lost **both** devices, but kept **both** seed phrases | ✅ Yes | Restore both keys from their seeds |
| Lost a **seed phrase**, but **both devices still work** | ⚠️ Act now | You can still spend — move funds to a fresh, fully-backed-up wallet before that device ever fails (see below) |
| A key's **device _and_ its seed** are both gone | ❌ No | You'd hold only 1 of the 2 required keys; a 2-of-2 cannot sign |
| Lost **both** seed phrases | ❌ No | No one can rebuild your keys — if a device then fails, the funds freeze |

{% hint style="danger" %}
**No one can recover your seed phrases for you.** SSP is self-custody and zero-knowledge — the SSP Relay never sees your private keys or seed phrases. If both seed phrases are lost, the funds cannot be recovered by anyone, including SSP.
{% endhint %}

## Replacing a lost or broken phone (SSP Key)

If your phone is lost, stolen, or broken but you still have your **key seed phrase**:

1. Install **SSP Key** on the new phone:
   * iOS: [App Store](https://apps.apple.com/us/app/ssp-key/id6463717332)
   * Android: [Google Play](https://play.google.com/store/apps/details?id=io.runonflux.sspkey)
2. Choose **Restore Key** and enter your backed-up **key seed phrase**, then set a new key password.
3. Re-sync the restored SSP Key with your SSP Wallet (scan the QR code in the wallet, or paste the extended public key). For the full step-by-step, see [Got a New Phone? Re-sync SSP Key with SSP Wallet](resync-ssp-key-new-phone.md) — it also covers re-syncing each additional chain. For the screenshot walkthrough, see [Restore Key Using Seed Phrase and Syncing to SSP Wallet](../getting-started-with-ssp-key/restore-key-using-seed-phrase-and-syncing-to-ssp-wallet.md).

Because the restored key is derived from the **same seed phrase**, it produces the **same public key** — so your multisig addresses stay exactly the same. Your funds were never on the phone; they remain on-chain and become spendable again as soon as both devices are back in sync.

## Replacing a browser or computer (SSP Wallet)

If you reset your browser, switched computers, or lost access to the extension but still have your **wallet seed phrase**:

1. Install the **SSP Wallet** browser extension on the new browser ([download page](https://sspwallet.io/download)).
2. Choose **Forgot Password? Restore**, enter your backed-up **wallet seed phrase**, and set a new password. See [Restore Wallet Using Seed Phrase](../getting-started-with-ssp-wallet/wallet/quickstart-2.md).
3. Re-sync with your SSP Key (scan the QR code, or input the SSP Key extended public key).

As with the key, the restored wallet derives the **same** first key from the same seed, so all addresses match and balances reappear after syncing.

## If you've lost one seed phrase (but both devices still work) — act now

This is the time-sensitive case. Because **both devices still work**, both keys can still sign, so **you can still move funds right now**. The problem is that the key whose seed you lost no longer has a backup: if that device is ever wiped, updated badly, or breaks, that key is gone — and a 2-of-2 with only one surviving key can **never sign again**.

So while both devices are still working, migrate everything to a freshly backed-up wallet:

1. **Create a brand-new SSP Wallet + SSP Key** with two new seed phrases, and back up **both** — stored separately.
2. **Move all funds** from the old wallet to the new one. You can still sign because both old devices work.
3. **Stop using the old wallet** and continue on the new, fully-backed-up one.

{% hint style="warning" %}
Don't wait. The same urgency applies if you've lost **both** seed backups but your devices still work. The moment an un-backed-up device fails, that key is unrecoverable and its funds are frozen for good.
{% endhint %}

## If you've lost an entire key (a device and its seed)

If one key is gone with **no** backup — for example your phone is lost or broken **and** its **key** seed phrase was never saved (or was also lost) — then you hold only **one** of the two keys SSP requires. Every transaction needs both, so the funds on that wallet **can no longer be moved**. The same is true if **both** seed phrases are lost.

There is no workaround: SSP is a true 2-of-2 with no third key and no recovery service — the SSP Relay never holds your keys or seeds. This is exactly why you must back up **both** seed phrases, separately, the moment you set up your wallet.

## Best practices to avoid lockout

* **Back up both seed phrases** the moment you create the wallet and the key — write them down offline.
* **Store the two phrases separately** (different physical locations), so a single incident can't take out both.
* **Never** store a seed phrase as a screenshot, in cloud notes, or in a password field synced across devices.
* **Test your restore** with a small amount before relying on it for large holdings.
* Keep both apps updated, and keep device passwords/PINs and biometrics enabled.

## Enterprise: replacing a vault signer

For **SSP Enterprise** vaults, recovery for an **individual signer** works the same way: that signer restores their SSP Wallet and SSP Key from **their own** seed phrases on new devices, and their access — and ability to sign — returns automatically, because their identity is derived from their seeds, not from the specific hardware.

What is **different** for enterprise is changing **who** is on a vault:

{% hint style="warning" %}
A vault's **signer set and threshold are fixed at creation** and cannot be edited afterwards (see [Creating Multisig Vaults](../enterprise/creating-vaults.md#critical-signers-and-threshold-are-permanent)). There is no in-place "rotate signer" operation that swaps one signer's keys for another's on an existing vault.
{% endhint %}

So if a signer leaves the organization, or you need to permanently replace one person on a vault (rather than just help an existing signer onto new devices), the supported path is:

1. **Create a new vault** on the same chain with the new signer set and threshold.
2. **Migrate funds** from the old vault to the new one using the old vault's current signers.
3. Retire the old vault.

Removing a member from the organization revokes their access to the SSP Enterprise interface, but does **not** remove them from a vault's on-chain multisig — the signer set was finalized when the vault was created. For sensitive departures, plan to migrate to a new vault that excludes that person. See [Inviting Team Members & Roles](../enterprise/inviting-members.md#removing-members) for the member-removal details.

## Related pages

* [Got a New Phone? Re-sync SSP Key with SSP Wallet](resync-ssp-key-new-phone.md)
* [Restore Wallet Using Seed Phrase](../getting-started-with-ssp-wallet/wallet/quickstart-2.md)
* [Restore Key Using Seed Phrase and Syncing to SSP Wallet](../getting-started-with-ssp-key/restore-key-using-seed-phrase-and-syncing-to-ssp-wallet.md)
* [Security Overview](../security/security-overview.md)
* [Device Security Guidelines](../security/device-security.md)
* [Common Issues & Solutions](common-issues.md)
* [Creating Multisig Vaults](../enterprise/creating-vaults.md)
