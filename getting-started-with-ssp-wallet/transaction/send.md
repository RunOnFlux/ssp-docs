---
icon: file-lines
---

# Send

Sending in SSP Wallet v2 is one unified, guided flow for every chain — three steps: **Details → Review → Approve**.

From your Home screen click the "Send" button

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../../.gitbook/assets/image (217).png" alt="" width="308"><figcaption></figcaption></figure></div>

## Step 1 — Details

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../../.gitbook/assets/image (131).png" alt="" width="311"><figcaption></figcaption></figure></div>

Provide values for the following

* Receiver Address - The address of your crypto recipient
* Amount to Send - The amount you will send to your crypto recipient (use **Max** to send everything after fees)

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure></div>

{% hint style="info" %}
Amount to Send should be within the Max limit
{% endhint %}

{% hint style="info" %}
If you already have saved contacts, you may use them from the Receiver Address field — recent recipients are offered too. You can also scan a QR code of the recipient's address.

For creating contacts see [Create New Contact](../contacts/create-new-contact.md).
{% endhint %}

## Step 2 — Review

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../../.gitbook/assets/image (133).png" alt="" width="311"><figcaption></figcaption></figure></div>

The review step shows everything about your transaction before anything is signed:

* **To** — the **full recipient address** is always displayed. Check it character by character; this is your moment to catch a wrong or tampered address.
* **Amount** — with its fiat value.
* **Network Fee** — the fee is set **automatically** by default. You can override it: choose **Slow**, **Automatic**, **Fast**, or **Custom** to set your own (per-chain controls such as sat/vB or gas settings live under Custom).

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../../.gitbook/assets/image (132).png" alt="" width="311"><figcaption></figcaption></figure></div>

When everything checks out, continue to the approval step.

## Step 3 — Approve on your SSP Key

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../../.gitbook/assets/image (135).png" alt="" width="306"><figcaption></figcaption></figure></div>

The wallet shows **"Approve on your SSP Key"** with a live status (*Sent → Awaiting approval → Approved*) while the request travels to your phone. If the request doesn't arrive, use **"Show QR instead"** and scan it with the Key.

On your SSP Key, the transaction is **decoded on the device** and shown in plain language — amount, token, and the full recipient. Verify it matches what you entered, then **slide to approve** and confirm with biometrics or password. See [Approving Requests with SSP Key](../../getting-started-with-ssp-key/approving-requests-with-ssp-key.md).

Once approved, the transaction broadcasts. You can check it in the **Activity** tab (bottom bar).

{% hint style="info" %}
Transaction speed may vary for each chain; you can also visit the chain's block explorer to verify your transactions.
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../../.gitbook/assets/image (136).png" alt="" width="308"><figcaption></figcaption></figure></div>

Congratulations on your first sending crypto transaction!
