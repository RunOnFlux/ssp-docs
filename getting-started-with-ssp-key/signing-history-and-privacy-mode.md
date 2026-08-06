---
icon: clock-rotate-left
---

# Signing History & Privacy Mode

## Signing History

SSP Key v2 keeps a **Signing History** — a log of everything this Key has co-signed: transactions, EVM messages, identity messages, vault transactions, shared public nonces, and other signed actions, each with its time and details.

Its privacy properties are strict:

* **Local only** — the log is stored **only on your device** and is never sent anywhere, not even to the SSP Relay.
* **Encrypted** — stored with the same encryption that protects the rest of your Key's data.
* **Biometric-gated** — opening the history requires your biometric or password, just like signing.

You can permanently delete the log at any time with **Clear history**. This cannot be undone and affects only the local record — it has no effect on the blockchain or your wallet.

{% hint style="info" %}
The history is a convenience record for you. It plays no role in signing, and losing it (e.g., restoring the Key on a new phone) loses nothing but the log itself.
{% endhint %}

## Privacy Mode

**Privacy Mode** masks wallet identities and transaction references shown in the signing history — useful if someone glances at your phone.

{% hint style="warning" %}
Details **inside signing requests are never masked** — when you approve something, you must always see exactly what you are signing. Privacy Mode only affects the history view.
{% endhint %}

The wallet side has its own privacy mode too: tap your balance in SSP Wallet to blur every amount. See [Navigating SSP Wallet](../getting-started-with-ssp-wallet/navigating-ssp-wallet.md).
