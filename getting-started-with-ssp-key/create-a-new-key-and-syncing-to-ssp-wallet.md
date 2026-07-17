---
icon: file-lines
---

# Create a New Key and Syncing to SSP Wallet

{% hint style="info" %}
Please make sure SSP Key is installed in your device. For installing SSP Wallet  please use this guide: [https://app.gitbook.com/o/JAOVHXmS0zOqrz6ohvwK/s/OLYjN2kelo3HWVPLgZVU/\~/changes/7/getting-started-with-ssp-key/install-ssp-key](https://app.gitbook.com/o/JAOVHXmS0zOqrz6ohvwK/s/OLYjN2kelo3HWVPLgZVU/~/changes/7/getting-started-with-ssp-key/install-ssp-key)
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (139).png" alt="" width="281"><figcaption></figcaption></figure></div>

Click "Synchronise Key!"

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (140).png" alt="" width="281"><figcaption></figcaption></figure></div>

Please provide values on "Set SSP Key Password" and "Confirm Key Password" then click "Setup Key!"

{% hint style="info" %}
If you already have a key and you want to restore it to your device you will need to click "Restore Key". Please use this guide:
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (141).png" alt="" width="281"><figcaption></figcaption></figure></div>

Click "Show Mnemonic Key Seed Phrase"

{% hint style="info" %}
Please back up this seed phrase offline and store it separately. It backs up **Key #2 only** — one of the two keys in your 2-of-2 wallet — so on its own it cannot move funds; your SSP Wallet holds Key #1, which has its own separate seed phrase. Back up **both** seeds separately: if a key's device **and** its seed phrase are both lost, the 2-of-2 can no longer sign and the funds are frozen. By clicking "Show Mnemonic Wallet Seed Phrase" you will be presented with your own unique seed phrase.
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (142).png" alt="" width="281"><figcaption></figcaption></figure></div>

After you back up your seed phrase, SSP Key v2 verifies your backup with a short **word challenge**: you'll be asked to pick 3 specific words of your phrase (e.g. "Select word #5") from a set of choices. This replaces the old "I have backed it up" switch — you need your written backup at hand to pass it. Once you see "Backup confirmed", click "Setup Key!"

{% hint style="info" %}
Synchronisation to SSP Wallet is needed to continue. SSP Wallet must also be created for this setup, please use this guide: [https://app.gitbook.com/o/JAOVHXmS0zOqrz6ohvwK/s/OLYjN2kelo3HWVPLgZVU/\~/changes/7/getting-started-with-ssp-wallet/quickstart-1](../getting-started-with-ssp-wallet/wallet/quickstart-1.md)
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (143).png" alt="" width="315"><figcaption></figcaption></figure></div>

{% hint style="info" %}
In SSP Wallet creation you need to be in this Sync SSP Key step in order to sync up with SSP Key
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (144).png" alt="" width="290"><figcaption></figcaption></figure></div>

In your SSP Key click "Scan Code" to scan the QR code in your SSP Wallet

{% hint style="info" %}
SSP Key needs to have permission to take picture, it is recommended to select allow while using the app.
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (145).png" alt="" width="290"><figcaption></figcaption></figure></div>

{% hint style="info" %}
For iOS users you can go to Settings > Apps >  SSP Key , then find the camera settings and toggle on
{% endhint %}

After scanning the SSP Wallet QR code, review the synchronisation request on your SSP Key and **slide to approve** it

{% hint style="info" %}
If your SSP Wallet requested several chains at once (multi-chain batch sync), the request names every chain being activated — one approval covers them all, and the Key shows live progress as it prepares keys for each chain.
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (146).png" alt="" width="290"><figcaption></figcaption></figure></div>

Please type your password in the "Confirm Key Password" text box, then click "Confirm"

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (147).png" alt="" width="290"><figcaption></figcaption></figure></div>

{% hint style="info" %}
You can also use the fingerprint scanner or face recognition of your device in syncing
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (148).png" alt="" width="314"><figcaption></figcaption></figure></div>

{% hint style="warning" %}
**Verify the pairing.** After syncing, both devices show the same **6-word verification code**. In SSP Key, tap **"Scan wallet to verify"** and scan the wallet's verification QR — or compare the 6 words on both screens. They must match exactly; if they differ, stop and re-pair. See [Pairing Verification Code](pairing-verification-code.md).
{% endhint %}

Congratulations for creating a new key and synching your SSP Wallet and SSP Key!
