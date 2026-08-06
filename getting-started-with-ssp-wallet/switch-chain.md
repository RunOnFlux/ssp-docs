---
icon: file-lines
---

# Switch Chain

{% hint style="info" %}
Switch Chain lets you move from one network to another — for example from Bitcoin to Ethereum. Each chain has its own addresses, balances, and activity.
{% endhint %}

## Switching to a chain you already activated

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (219).png" alt="" width="308"><figcaption></figcaption></figure></div>

Tap the **wallet & chain pill** at the top of the wallet — it shows your active wallet's identicon, name, and chain.

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure></div>

The switcher opens: your wallets are listed on top, and the **Networks** section below lists the chains. Use the search box to find a wallet or network quickly.

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (29).png" alt="" width="209"><figcaption></figcaption></figure></div>

Tap the chain you want. If it's already activated, the wallet switches immediately — that's it.

## Activating a new chain

The first time you switch to a chain, your SSP Key must share that chain's public keys with the wallet (no funds move — every future transaction still needs your approval on the Key).

{% hint style="info" %}
Installing SSP Key is needed before you can create a wallet

* iOS: [https://apps.apple.com/us/app/ssp-key/id6463717332](https://apps.apple.com/us/app/ssp-key/id6463717332)&#x20;
* Android: [https://play.google.com/store/apps/details?id=io.runonflux.sspkey](https://play.google.com/store/apps/details?id=io.runonflux.sspkey)

Guide for installing SSP Key: [Install SSP Key](../getting-started-with-ssp-key/install-ssp-key.md)
{% endhint %}

**With SSP Key v2 (one-tap activation):**

1. Select the chain (or several chains) to activate. You can also activate chains from the [Portfolio tab](portfolio-and-activity.md).
2. SSP Wallet sends a **Chain Activation Request** to your paired SSP Key.
3. On your SSP Key, review the request — it names every chain being activated — and approve **once** (slide to approve, then confirm with biometrics or password). The Key shows live progress as it prepares keys for each chain.
4. Both devices then show a **6-word verification code**. Scan the wallet's code with your SSP Key, or compare the words yourself — they must match exactly. See [Pairing Verification Code](../getting-started-with-ssp-key/pairing-verification-code.md).

{% hint style="danger" %}
If the verification words on the two devices **differ**, do not use the newly activated chain — choose "They don't match", and re-pair. Matching words prove no one, including the relay, tampered with the sync.
{% endhint %}

**QR code fallback (works with SSP Key v1 and v2):**

If your SSP Key app hasn't been updated, or the request doesn't arrive, the wallet shows a QR code for the chain instead.

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (84).png" alt="" width="314"><figcaption></figcaption></figure></div>

Scan the QR code using your SSP Key, then approve the synchronisation on the Key.

Alternatively, you can manually input the sync data shown in SSP Wallet into your SSP Key (Manual Input) — the fully offline path.

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (85).png" alt="" width="314"><figcaption></figcaption></figure></div>

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (31).png" alt="" width="311"><figcaption></figcaption></figure></div>

Congratulations, you successfully switched chain!
