# Security Overview

## SSP's Security-First Approach

SSP Wallet is designed with **security as the foundation**, not an afterthought. Every component, communication, and operation is built to protect your cryptocurrency assets using industry-leading security practices.

## 🛡️ Core Security Principles

### 1. **True 2-of-2 Multisignature**
- **Both keys required** for every transaction
- **No single point of failure** in the system
- **Cryptographically enforced** security at the blockchain level

### 2. **Zero-Knowledge Architecture**
- Private keys **never leave your devices**
- SSP Relay **cannot access** sensitive data
- **End-to-end encryption** for all communications

### 3. **Self-Custody First**
- **You control** all private keys
- **No third-party custody** of funds
- **Full ownership** of your cryptocurrency

### 4. **Defense in Depth**
- **Multiple security layers** working together
- **Redundant protection** mechanisms
- **Graceful failure modes** that prioritize security

## 🔒 Security Layers

### Layer 1: Runtime Security
```
┌─────────────────────────────────────────┐
│          Runtime Security              │
├─────────────────────────────────────────┤
│ • LavaMoat Compartmentalization        │
│ • SES Lockdown Protection              │
│ • Supply Chain Attack Prevention       │
│ • Module Isolation (70+ packages)      │
│ • User-Verifiable Security Tests       │
└─────────────────────────────────────────┘
```

> **Try it yourself!** Click the version number in the wallet footer 5 times rapidly to access the built-in security test page and verify all protections are active.

### Layer 2: Device Security
```
┌─────────────────────────────────────────┐
│           Device Security               │
├─────────────────────────────────────────┤
│ • Password/PIN Protection               │
│ • Device Fingerprinting                 │
│ • Secure Storage (Keychain/MMKV)       │
│ • Biometric Authentication (Optional)   │
└─────────────────────────────────────────┘
```

### Layer 3: Cryptographic Security
```
┌─────────────────────────────────────────┐
│        Cryptographic Security          │
├─────────────────────────────────────────┤
│ • AES-256-GCM Encryption               │
│ • PBKDF2 Key Derivation                │
│ • Schnorr/ECDSA Signatures             │
│ • BIP48 HD Wallet Standard             │
└─────────────────────────────────────────┘
```

### Layer 4: Communication Security
```
┌─────────────────────────────────────────┐
│       Communication Security           │
├─────────────────────────────────────────┤
│ • TLS 1.3 Encryption                   │
│ • Message Authentication Codes         │
│ • Replay Attack Protection             │
│ • Certificate Pinning                  │
└─────────────────────────────────────────┘
```

### Layer 5: Network Security
```
┌─────────────────────────────────────────┐
│          Network Security              │
├─────────────────────────────────────────┤
│ • Secure Blockchain RPC Endpoints      │
│ • Transaction Verification             │
│ • Double-Spend Protection              │
│ • Network Fee Validation               │
└─────────────────────────────────────────┘
```

## 🔐 Encryption Implementation

### Private Key Encryption
```javascript
// Simplified encryption flow
const encryptPrivateKey = (privateKey, password, deviceFingerprint) => {
  // 1. Generate random salt and IV
  const salt = crypto.getRandomValues(new Uint8Array(32));
  const iv = crypto.getRandomValues(new Uint8Array(16));
  
  // 2. Derive encryption key using PBKDF2
  const derivedKey = PBKDF2(password + deviceFingerprint, salt, 100000);
  
  // 3. Encrypt using AES-256-GCM
  const encrypted = AES_GCM.encrypt(privateKey, derivedKey, iv);
  
  return {
    data: base64(encrypted),
    salt: base64(salt),
    iv: base64(iv)
  };
};
```

### Key Derivation (BIP48)
```
Master Seed
    ↓
m/48'/cointype'/account'/script_type'/change/index
    ↓
Example Bitcoin: m/48'/0'/0'/2'/0/0
Example Ethereum: m/48'/60'/0'/0'/0/0
```

## 🛡️ Attack Resistance

### Protected Against
✅ **Single Device Compromise**: 2-of-2 multisig protects your funds
✅ **Phishing Attacks**: Address validation and device verification
✅ **Man-in-the-Middle**: End-to-end encryption and certificate pinning
✅ **Brute Force**: Strong encryption and device fingerprinting
✅ **Server Compromise**: Zero-knowledge architecture protects keys
✅ **Replay Attacks**: Nonce-based message authentication
✅ **Social Engineering**: Multi-device approval required
✅ **Supply Chain Attacks**: LavaMoat compartmentalization and policies
✅ **Code Injection**: SES lockdown blocks eval and dynamic code execution
✅ **Prototype Pollution**: Immutable prototypes prevent tampering

### Potential Risks (Mitigated)
⚠️ **Both Device Loss**: Mitigated by seed phrase backup
⚠️ **User Error**: Mitigated by clear UI and confirmation steps
⚠️ **Physical Attack**: Mitigated by password protection
⚠️ **Compromised Dependencies**: Mitigated by LavaMoat runtime protection  

## 🔍 Security Audits

### Professional Security Audits
SSP has undergone comprehensive security audits by [**Halborn Security**](https://halborn.com/):

#### 📄 **SSP Wallet, Key, and Relay Audit (2025)**
- **Scope**: Browser extension, mobile apps, and relay server
- **Focus**: Cross-device security, encryption, and communication protocols
- **Result**: Comprehensive security validation completed
- **[View Public Report](https://www.halborn.com/audits/influx-technologies/ssp-wallet-relay-and-key)**

#### 📄 **Smart Contracts Audit (2025)**
- **Scope**: Account Abstraction smart contracts with Schnorr multisig
- **Focus**: ERC-4337 implementation and cryptographic security
- **Result**: Smart contract security validated
- **[View Public Report](https://www.halborn.com/audits/influx-technologies/account-abstraction-schnorr-multisig)**

#### 📄 **SDK Security Audit (2025)**
- **Scope**: Account Abstraction SDK with Schnorr signatures
- **Focus**: Developer integration security and cryptographic implementation
- **Result**: SDK security practices validated
- **[View Public Report](https://www.halborn.com/audits/influx-technologies/account-abstraction-schnorr-signatures-sdk)**

## 🚨 Security Best Practices for Users

### Essential Practices
1. **📱 Keep Both Devices Secure**
   - Use strong passwords/PINs
   - Enable biometric authentication when available
   - Keep software updated

2. **🔄 Backup Your Seed Phrases**
   - Store both wallet and key seed phrases separately
   - Use secure, offline storage methods
   - Test recovery procedures

3. **🔍 Verify Transactions**
   - Always verify recipient addresses
   - Check transaction amounts carefully
   - Confirm network fees are reasonable

4. **🛡️ Stay Alert**
   - Be suspicious of unsolicited communications
   - Verify website URLs carefully
   - Never share seed phrases or private keys

### Advanced Security
1. **🔐 Multi-Location Backup**
   - Store seed phrases in different physical locations
   - Consider safety deposit boxes for large holdings
   - Use fire-proof and water-proof storage

2. **📊 Regular Wallet Review**
   - Review transaction history regularly
   - Verify both devices sync properly
   - Keep backup seed phrases secure and accessible

## 🆘 Security Incident Response

### If You Suspect Compromise
1. **🚨 Immediate Actions**
   - Stop all transactions immediately
   - Disconnect from networks if possible
   - Document any suspicious activity

2. **🔍 Assessment**
   - Check transaction history for unauthorized activity
   - Verify both devices are still secure
   - Review recent account access

3. **🔄 Recovery**
   - If one device is compromised, immediately move funds using the other
   - Create new wallet with fresh seed phrases
   - Report incident to security team

### Report Security Issues
- **GitHub Security**: [Report security vulnerabilities](https://github.com/RunOnFlux/ssp-wallet/security)
- **GitHub Issues**: [General security questions](https://github.com/RunOnFlux/ssp-wallet/issues)
- **Responsible Disclosure**: Use GitHub's security reporting features

## 🔬 Technical Security Details

### Cryptographic Specifications
- **Encryption**: AES-256-GCM with random IVs
- **Key Derivation**: PBKDF2 with 100,000+ iterations
- **Signatures**: ECDSA (Bitcoin/Ethereum), Schnorr (Account Abstraction)
- **Hashing**: SHA-256 for Bitcoin, Keccak-256 for Ethereum
- **HD Wallets**: BIP48 derivation for multisignature wallets

### Security Architecture
- **Open Source**: Transparent code for community security review
- **Deterministic Builds**: Verifiable build process with SHA256 checksums
- **Runtime Protection**: LavaMoat compartmentalization and SES lockdown
- **Regular Updates**: Continuous security improvements
- **Community Security**: GitHub-based security reporting

## 🏗️ Deterministic Builds

SSP Wallet implements **deterministic builds** to ensure complete transparency and verifiability of browser extension packages. This means anyone can rebuild the exact same packages and verify the integrity of what's submitted to browser stores.

### Why This Matters for Security

```
┌──────────────────────────────────────────┐
│    Deterministic Build Protection        │
├──────────────────────────────────────────┤
│ ✅ Verifiable Integrity                  │
│ ✅ No Hidden Code Injection              │
│ ✅ Reproducible Across Machines          │
│ ✅ Browser Store Verification            │
│ ✅ Build-Time Supply Chain Protection    │
└──────────────────────────────────────────┘
```

The same source code **always** produces byte-identical binaries, making it impossible to inject malicious code during the build process without detection.

### Quick Verification

```bash
# Clone the repository
git clone https://github.com/RunOnFlux/ssp-wallet.git
cd ssp-wallet

# Build using Docker for reproducibility
npm run build:deterministic

# Verify SHA256 hashes match published version
sha256sum -c SHA256SUMS
```

### Build Environment

- **Docker Image**: `node:22.17.1-alpine` (SHA-pinned)
- **Fixed Timestamp**: 2025-01-01 00:00:00 UTC (eliminates variation)
- **Frozen Dependencies**: `yarn.lock` ensures exact versions
- **Isolated Environment**: No host dependencies affect output
- **Output**: SHA256-verified Chrome and Firefox packages

### Security Guarantees

**Protected Against:**
- ❌ Build environment attacks (Docker isolation)
- ❌ Supply chain substitution (frozen dependencies)
- ❌ Timestamp manipulation (fixed SOURCE_DATE_EPOCH)
- ❌ Hidden build steps (complete reproducibility)

**Verified By:**
- ✅ Security auditors can rebuild and verify
- ✅ Browser store reviewers can validate submissions
- ✅ Community members can independently verify
- ✅ SHA256 checksums provide cryptographic proof

For complete technical details, build instructions, troubleshooting, and Docker configuration, see the **[Deterministic Builds Developer Guide](../getting-started-with-ssp-wallet-development/deterministic-builds.md)**.

## Next Steps

- **[Runtime Security with LavaMoat](runtime-security.md)** - Deep dive into runtime protection and supply chain security
- **[Deterministic Builds Guide](../getting-started-with-ssp-wallet-development/deterministic-builds.md)** - Complete guide to verifying and reproducing builds
- **[Halborn Security Audit](halborn-audit-2025.md)** - Complete audit results and findings
- **[Device Security Guidelines](device-security.md)** - Secure your SSP Wallet devices
- **[GitHub Security](https://github.com/RunOnFlux/ssp-wallet/security)** - Report security issues
- **[Interactive Setup Tutorial](https://sspwallet.io/guide)** - Secure wallet setup guide