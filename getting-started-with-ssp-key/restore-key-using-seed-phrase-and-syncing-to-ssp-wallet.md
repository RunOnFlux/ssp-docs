---
icon: file-lines
---

# Restore Key Using Seed Phrase and Syncing to SSP Wallet

{% hint style="info" %}
Restoring Key using Seed Phrase is a way to restore your key using your backed up seed phrase when you created your key.
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (139).png" alt="" width="281"><figcaption></figcaption></figure></div>

Click "Restore Key"

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (149).png" alt="" width="284"><figcaption></figcaption></figure></div>

Provide your backed up seed phrase on "Input your Mnemonic Key Seed Phrase" text area. Also, provide password value on "Set SSP Key Password" and "Confirm Key Password".

Click "Import Key" button

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (150).png" alt="" width="284"><figcaption></figcaption></figure></div>

Click "Show Mnemonic Key Seed Phrase"

{% hint style="info" %}
Please back up this seed phrase offline and store it separately. It backs up **Key #2 only** — one of the two keys in your 2-of-2 wallet — so on its own it cannot move funds; your SSP Wallet holds Key #1, which has its own separate seed phrase. Back up **both** seeds separately: if a key's device **and** its seed phrase are both lost, the 2-of-2 can no longer sign and the funds are frozen. By clicking "Show Mnemonic Wallet Seed Phrase" you will be presented with your own unique seed phrase.
{% endhint %}

After you backup your seed phrase toggle "I have backed up my keys seed phrase in a secure location." then click "Setup Key!"

In your SSP Wallet, open the **Menu** tab (the ☰ icon in the bottom bar)

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image.png" alt="" width="304"><figcaption></figcaption></figure></div>

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (158).png" alt=""><figcaption></figcaption></figure></div>

Click "SSP Wallet Details"

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (1).png" alt="" width="179"><figcaption></figcaption></figure></div>

Input your password on "Confirm with Password" , then click "Grant Access" button

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (160).png" alt="" width="308"><figcaption></figcaption></figure></div>

Scroll down and you will find SSP Sync with SSP Key

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (161).png" alt="" width="308"><figcaption></figcaption></figure></div>

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure></div>

Click unhide icon

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (163).png" alt="" width="306"><figcaption></figcaption></figure></div>

In your SSP Key click "Scan Code" to scan the QR code in your SSP Wallet

{% hint style="info" %}
SSP Key needs to have permission to take picture, it is recommended to select allow while using the app.
{% endhint %}

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (153).png" alt="" width="301"><figcaption></figcaption></figure></div>

{% hint style="info" %}
For iOS users you can go to Settings > Apps >  SSP Key , then find the camera settings and toggle on
{% endhint %}

After scanning the SSP Wallet QR code, review the synchronisation request on your SSP Key and **slide to approve** it

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (154).png" alt="" width="301"><figcaption></figcaption></figure></div>

Please type your password in the "Confirm Key Password" text box, then click "Confirm"

<!-- TODO(v2): replace screenshot -->

<div align="left"><figure><img src="../.gitbook/assets/image (155).png" alt="" width="301"><figcaption></figcaption></figure></div>

{% hint style="info" %}
You can also use the fingerprint scanner or face recognition of your device in syncing
{% endhint %}

{% hint style="warning" %}
**Verify the pairing.** After syncing, both devices show the same **6-word verification code**. In SSP Key, tap **"Scan wallet to verify"** and scan the wallet's verification QR — or compare the 6 words on both screens. They must match exactly; if they differ, stop and re-pair. See [Pairing Verification Code](pairing-verification-code.md).
{% endhint %}

Congratulations for restoring your key and synching your SSP Wallet and SSP Key!
