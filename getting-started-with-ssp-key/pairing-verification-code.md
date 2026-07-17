---
icon: shield-check
---

# Pairing Verification Code

Starting with SSP v2, every time your SSP Wallet and SSP Key sync — first pairing, chain activation, or re-pairing — **both devices show a verification code of 6 words**. Comparing these words is how you personally verify that your pairing was not tampered with. Take a moment to do it every time.

## What it looks like

* After a successful sync, **SSP Wallet** shows: *"Verify this matches your SSP Key"* with 6 words and a QR code.
* **SSP Key** shows: *"Verify this matches your SSP Wallet"* with its own 6 words.

You confirm one of two ways:

1. **Scan to verify (recommended)** — in SSP Key, tap **Scan wallet to verify** and scan the wallet's verification QR code. The Key compares the codes for you and shows either *"Verified — your devices match, safe to continue"* or a loud **MISMATCH** warning.
2. **Compare by eye** — read the 6 words on both screens. They must be **exactly the same words in the same order**. If they match, choose **"They match — continue"** in the wallet.

## Why this matters

SSP Wallet and SSP Key exchange public keys through the SSP Relay server. The relay never sees private keys or seed phrases — but a hostile or compromised relay could, in theory, try to substitute key material during the exchange so that the resulting 2-of-2 wallet is not entirely under your control.

The verification code defeats this:

* Each device **independently derives the 6 words from its own view of the two synced keys** (a cryptographic hash of the key pair, rendered as words).
* The relay never generates or transports the code — it cannot forge it.
* If anyone — including the relay — swapped any key on any chain during sync, the two devices would be hashing **different** key material, and the words would **not** match.

Matching words are proof that both devices hold the same key pair, and therefore that your pairing is exactly the 2-of-2 between your two devices and nothing else.

{% hint style="info" %}
The code covers **all chains synced in that session**: with multi-chain batch sync, one comparison verifies every chain activated in that batch. A tampered key on *any* of them would change the words.
{% endhint %}

## If the words differ

{% hint style="danger" %}
**If the two devices show different words, STOP.**

* Do **not** use the wallet and do **not** send any funds to its addresses.
* Choose **"They don't match"** in SSP Wallet. The affected sync is not activated.
* Remove the pairing and pair again, ideally on a network you trust. If the mismatch repeats, report it immediately via [GitHub Issues](https://github.com/RunOnFlux/ssp-wallet/issues) or [support.runonflux.io](https://support.runonflux.io).

A mismatch does not endanger funds already on previously verified addresses — it means the *new* sync must not be trusted.
{% endhint %}

## Notes

* The verification code is **display-only**. It plays no role in key derivation or signing — it exists purely so you can check the sync.
* The words are drawn from the same word list as seed phrases, but they are **not a seed** and **not secret**. Showing them to someone reveals nothing about your keys.
* Both apps must be on v2 to show the code. If your SSP Key is still on v1, the sync itself works as before — update the Key to get the verification step.
* The manual/offline pairing path (scanning QR codes directly, without the relay) also ends with the verification code on v2 — compare the words there too.
