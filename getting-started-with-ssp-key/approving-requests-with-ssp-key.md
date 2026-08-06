---
icon: hand
---

# Approving Requests with SSP Key

SSP Key v2 rebuilds the approval moment from the ground up. Every request — transactions, chain activations, message signing, dApp requests — shares one clean layout, and every approval is a deliberate gesture.

## Decoded on your device, in plain language

When a request arrives, SSP Key **decodes it on your phone** and presents a plain-language summary — e.g. the amount, the token, and the recipient of a send. The details shown are taken from the raw data being signed, **never trusted from the network**: what you read on the Key's screen is what you sign.

Always verify on the SSP Key screen before approving:

* The **action** — what the transaction actually does
* The **full recipient address** — compare it with what you entered in SSP Wallet
* The **amount** and **network fee**

Advanced users can expand the raw data behind the summary.

## Token approval warnings

Some dApp transactions don't move funds directly — they grant a contract permission to spend your tokens later. SSP Key v2 calls these out loudly:

{% hint style="warning" %}
* **Unlimited allowances** — a request to "Allow UNLIMITED spending" grants the named spender the right to spend an unbounded amount of that token. Only approve for contracts you fully trust, and prefer dApps that request limited amounts.
* **NFT collection-wide operator grants** — a "set approval for all" request lets the named operator transfer **every token you own in that contract**. Treat it with the same caution as an unlimited allowance.
{% endhint %}

In both cases the Key names the spender/operator address so you can verify it.

## Slide to approve

Approving is a slide gesture: **slide the control all the way to the right** to approve. Release early and nothing happens — an accidental tap can never sign anything. After the slide, your **biometric or password confirmation** is still required, exactly as before.

Rejecting is always a plain, immediately reachable button — declining a request never requires extra steps.

{% hint style="info" %}
Nothing in SSP ever auto-approves. Every signature requires the slide gesture plus your biometric/password on the Key. That friction is deliberate — it is your second factor doing its job.
{% endhint %}

## After approving

The signed response returns to your SSP Wallet through the relay (or via QR in offline mode), the wallet combines both signatures, and the transaction broadcasts. On the wallet side you'll see the live 2-of-2 handshake screen progress from *Sent* → *Awaiting approval* → *Approved*.

Every action you co-sign is recorded in your Key's local [Signing History](signing-history-and-privacy-mode.md).
