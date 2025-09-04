---
icon: file-lines
---

# Overview

SSP Key is your **two-factor authentication (2FA)** solution for the [**SSP Wallet**](https://sspwallet.io/). It securely holds the **secondary private key** required to construct **2-of-2 multisignature addresses** and sign transactions. With its own independent seed phrase, SSP Key operates on true blockchain protocol standards, providing fully enforced **multisignature, multi-asset accounts**.

This **powerful** tool brings the **simplicity and security of multisignature technology** to average users, making it easier than ever to manage multisignature addresses across multiple assets. SSP Key empowers users with the highest level of protection while maintaining a user-friendly interface, ensuring anyone can benefit from robust, blockchain-based security without complexity.

#### How SSP Key and SSP Wallet Work Together

SSP Key is an essential part of the **SSP Wallet ecosystem**, enabling a **2-of-2 multisignature system** for unmatched security:

* **SSP Wallet** manages one private key.
* **SSP Key** (on your mobile device) holds the second private key.\
  Both keys are required to authorize transactions, ensuring that your funds remain secure even if one device is compromised.

#### **Advanced Encryption**

* **Secure Storage**: User passwords and sensitive data are stored with `react-native-encrypted-storage`, leveraging MMKV with encryption powered by CryptoJS.
* **Randomized Security**: Salts and initialization vectors (IVs) are randomly generated to enhance encryption robustness.
* **Performance Focused**: Avoids underperforming libraries like Cryptr, ensuring optimal functionality in React Native environments.

####
