---
icon: wallet
cover: https://sspwallet.io/logo-light-mode.svg
coverY: 50
layout:
  cover:
    visible: true
    size: hero
---

# Welcome to SSP Wallet

## SSP Wallet

### Secure. Simple. Powerful.

Visit us at:

* [SSP Wallet](https://sspwallet.io)
* [Run on Flux](https://runonflux.com)

#### Download SSP Wallet:

* **Google Chrome Extension:** [SSP Wallet on Chrome Web Store](https://chromewebstore.google.com/u/3/detail/ssp-wallet/mgfbabcnedcejkfibpafadgkhmkifhbd)
* **Mozilla Firefox Extension:** [SSP Wallet on Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/ssp-wallet)

#### Download SSP Key:

* **iOS:** [Download on the App Store](https://apps.apple.com/us/app/ssp-key/id6463717332)
* **Android:** [Download on Google Play](https://play.google.com/store/apps/details?id=io.runonflux.sspkey)

***

### Why SSP Wallet?

SSP Wallet is not just another crypto wallet. It is a **true two-factor authentication wallet** designed with **security** and **self-custody** at its core. Here's how it works:

1. **Two Devices, Two Keys:**
   * Your **SSP Wallet** contains one private key.
   * Your **SSP Key** (on your mobile device) contains a second private key.
2. **2-of-2 Multisignature:**
   * Transactions are constructed and signed by the SSP Wallet and then signed again by SSP Key.
3. **Enhanced Security:**
   * Keys, seeds, and sensitive data are never shared between devices, making it impossible to compromise without access to both devices.

This design ensures that **both devices are required** to authorize any transaction, making the wallet incredibly secure and user-friendly.

***

### Technical Details

#### Key Derivation

* SSP Wallet adheres to the **BIP48 derivation scheme** for generating hierarchical deterministic keys supporting P2SH, P2SH-P2WSH, and P2WSH addresses.
* Example derivation paths for popular chains:
  * **Bitcoin:** `m/48'/0'/0'/2'/0/0`
  * **Flux:** `m/48'/19167'/0'/0'/0/0`
* Extended functionality includes support for additional chains and constructing multiple external addresses per chain as needed.

#### Synchronization Process

**Initial Setup:**

1. **SSP Relay Server:**
   * Simplifies the synchronization process by facilitating communication between SSP Wallet and SSP Key.
   * Synchronization starts when the SSP Key scans a Hardened Extended Public Key QR code from SSP Wallet.
   * A special identity path (`m/48'/0'/0'/2'/10/0`) reserved for SSP Wallet verifies unique wallet instances.
   * [SSP Relay GitHub Repository](https://github.com/RunOnFlux/ssp-relay)
2. **Public Key Exchange:**
   * SSP Key sends its hardened extended public key (e.g., `m/48'/0'/0'/2'`) to the SSP Relay Server along with a constructed 2-of-2 multisignature address.
   * SSP Wallet validates the received address, ensuring integrity.
3. **Validation and Confirmation:**
   * Both SSP Wallet and SSP Key confirm matching derived addresses to finalize synchronization.

**Transaction Signing:**

* Transactions are signed in two steps:
  1. SSP Wallet constructs the transaction and signs it with its private key.
  2. SSP Key receives the partially signed transaction via the relay server, signs it with its private key, and returns the fully signed transaction for broadcast.

**Offline Functionality:**

* Transactions and synchronization can bypass the relay server through manual QR code scanning, maintaining security in environments with restricted connectivity.

#### Encryption and Storage Security

**Sensitive Data:**

1. **Encryption Layers:**
   * PBKDF2-based password derivation generates keys for AES-GCM encryption.
   * Secondary encryption uses device and browser fingerprints to restrict data access to the originating environment.
2. **Local Data Management:**
   * Serialized sensitive data (e.g., keys, seeds) is stored as JSON blobs with base64-encoded fields (`data`, `iv`, and `salt`).
   * This approach prevents brute-force attacks and unauthorized migration between devices.

**Session Management:**

* Encrypted passwords are stored temporarily in session storage, ensuring convenience without compromising security.
* No sensitive data is ever retained in unencrypted form, even within the application’s runtime memory.

**Non-Sensitive Data:**

* Information such as transaction history and balance data is stored using **LocalForge**, prioritizing performance without compromising sensitive details.

#### Attack Mitigation Strategies

* **Anti-Phishing Measures:**
  * The wallet and key validate each other's public keys and derived addresses during setup.
* **Server Security:**
  * SSP Relay Server only facilitates communication and cannot access private keys or sensitive data.
* **Brute Force Protection:**
  * Physical possession of both devices and knowledge of passwords are required to compromise the wallet.

***

### WalletConnect Integration

SSP Wallet supports **WalletConnect v2** (now **Reown**), enabling seamless integration with thousands of decentralized applications (dApps) on EVM-compatible networks.

#### Key Features:
* **EVM dApp Compatibility:** Connect to any WalletConnect-enabled dApp on Ethereum, Polygon, BSC, Avalanche, and Base
* **DeFi & NFT Support:** Full integration with DeFi protocols, NFT marketplaces, and trading platforms
* **Account Abstraction Support:** Native ERC-4337 Account Abstraction on supported EVM chains
* **EIP-712 Message Signing:** Full support for typed structured data signing standard used by modern dApps
* **Secure Transaction Signing:** All transactions still require both SSP Wallet and SSP Key for authorization
* **Real-Time Communication:** Instant connection and transaction requests through secure WebSocket connections

**Note:** WalletConnect is available only for EVM-compatible networks (Ethereum, Polygon, BSC, Avalanche, Base). UTXO networks (Bitcoin, Litecoin, etc.) use native wallet functionality.

***

### Supported Blockchains

SSP Wallet supports **15 blockchain networks** with native multisignature security:

#### UTXO Networks (Native Multisignature)
- **Bitcoin (BTC)** - P2WSH native segwit multisignature
- **Bitcoin Testnet** - P2WSH testing network
- **Bitcoin Signet** - P2WSH controlled test network
- **Litecoin (LTC)** - P2WSH native segwit multisignature
- **Dogecoin (DOGE)** - P2SH multisignature
- **Bitcoin Cash (BCH)** - P2SH multisignature
- **Ravencoin (RVN)** - P2SH multisignature with asset support
- **Zcash (ZEC)** - P2SH multisignature (transparent addresses)
- **Flux (FLUX)** - P2SH multisignature with cloud integration
- **Flux Testnet** - P2SH testing network

#### EVM Networks (Account Abstraction + WalletConnect)
- **Ethereum (ETH)** - Full DeFi ecosystem, ERC-4337 support
- **Polygon (POL)** - Low-cost transactions, gaming dApps
- **Binance Smart Chain (BNB)** - High throughput, diverse DeFi
- **Avalanche (AVAX)** - Fast finality, growing ecosystem  
- **Base (ETH on Base)** - Coinbase Layer 2, consumer apps

#### Testnet Networks
- **Sepolia** - Ethereum testnet
- **Amoy** - Polygon testnet

Each network maintains SSP's **2-of-2 multisignature security** while leveraging blockchain-specific optimizations and features.

***

### Open Source Transparency

SSP Wallet is fully open source, ensuring transparency and community trust. Review and contribute to the project here:\
[SSP Wallet GitHub Repository](https://github.com/RunOnFlux/ssp-wallet)

***

### Documentation

SSP Wallet has a comprehensive documentation available at with many guides, FAQs, API references and more:\
[SSP Wallet Documentation](https://docs.sspwallet.io/)

***

### SSP Assets

Integrated Blockchains, Assets - Coins, Tokens in SSP Wallet are available at:\
[SSP Assets](https://docs.google.com/spreadsheets/d/1GUqGeV4hCwjKlxazY1vPY52owrEqXQ1UTchOKfkyS7c). SSP Supports custom ERC20 token imports on Ethereum, Polygon, BSC, Avalanche, and Base networks.

***

### Translation

SSP Wallet supports multiple languages! Help us make it accessible to everyone by contributing to translations at:\
[Translate SSP Wallet](https://translate.sspwallet.io), [Translate SSP Key](https://translatekey.sspwallet.io)

***

### Additional Repositories

* **SSP Key Repository:** [SSP Key GitHub Repository](https://github.com/RunOnFlux/ssp-key)
* **SSP Relay Repository:** [SSP Relay GitHub Repository](https://github.com/RunOnFlux/ssp-relay)
* **Account Abstraction Repository:** [Account Abstraction GitHub Repository](https://github.com/RunOnFlux/account-abstraction)

***

### Disclaimer

By using SSP Wallet, you agree to the terms outlined in the [SSP Disclaimer](https://github.com/RunOnFlux/ssp-wallet/blob/master/DISCLAIMER.md).

[![Crowdin](https://badges.crowdin.net/sspwallet/localized.svg)](https://crowdin.com/project/sspwallet)

***

### Developer Information

* **Built With:** React 19, TypeScript, Vite
* **Node Version:** 22+
* **SSP Wallet Version:** 1.26.0
* **SSP Key Version:** 1.15.0
* **SSP Relay Version:** 1.9.0
* **Run Development Mode:** `yarn dev`

#### Key Development Features:

1. **Modular Codebase:**
   * Separation of concerns for wallet UI, cryptographic operations, and relay server communication.
2. **Strong Typing:**
   * TypeScript ensures type safety and prevents runtime errors.
3. **Test Coverage:**
   * Unit tests of library ensures reliability of critical functions.

Join us in building a secure, simple, and powerful wallet for the crypto community!

***

### 🔒 Security Audits

Our security is a top priority. All critical components of the SSP ecosystem have undergone rigorous security audits by [Halborn](https://halborn.com/), ensuring the highest standards of protection.

* **SSP Wallet, SSP Key, and SSP Relay** were thoroughly audited, with the final report completed in **March 2025**.
* **Shnorr Multisig Account Abstraction Smart Contracts and SDK** underwent a comprehensive audit, finalized in **February 2025**.

#### 📜 Audit Reports

📄 **SSP Wallet, SSP Key, SSP Relay Audit**

* [**Halborn Audit Report – SSP Wallet, Key, Relay**](https://github.com/RunOnFlux/ssp-wallet/blob/master/SSP_Security_Audit_HALBORN_2025.pdf) (GitHub)
* [**Halborn Public Report – SSP Wallet, Key, Relay**](https://www.halborn.com/audits/influx-technologies/ssp-wallet-relay-and-key) (Halborn)

📄 **Smart Contracts Audit**

* [**Halborn Audit Report – Smart Contracts**](https://github.com/RunOnFlux/account-abstraction/blob/main/Account_Abstraction_Schnorr_MultiSig_SmartContracts_SecAudit_HALBORN_2025.pdf) (GitHub)
* [**Halborn Public Report – Smart Contracts**](https://www.halborn.com/audits/influx-technologies/account-abstraction-schnorr-multisig) (Halborn)

📄 **SDK Audit**

* [**Halborn Audit Report – SDK**](https://github.com/RunOnFlux/account-abstraction/blob/main/Account_Abstraction_Schnorr_MultiSig_SDK_SecAudit_HALBORN_2025.pdf) (GitHub)
* [**Halborn Public Report – SDK**](https://www.halborn.com/audits/influx-technologies/account-abstraction-schnorr-signatures-sdk) (Halborn)
