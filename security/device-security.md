# Device Security for SSP Wallet

## Overview

SSP Wallet uses a **2-of-2 multisignature system** where both your browser extension (SSP Wallet) and mobile app (SSP Key) must approve transactions. This means protecting both devices is essential for securing your cryptocurrency.

## 🖥️ SSP Wallet (Browser Extension) Security

### Browser Extension Protection

**Essential Browser Settings:**
- ✅ **Pin SSP Wallet extension** to toolbar for easy access
- ✅ **Enable automatic browser updates** 
- ✅ **Use official browser stores only** (Chrome Web Store, Firefox Add-ons)
- ✅ **Verify publisher** is "Run on Flux" before installing
- ❌ **Never install** SSP Wallet from third-party websites

### SSP Wallet Password Security

Your SSP Wallet password protects access to your browser extension:
- 🔐 **Use strong, unique password** (12+ characters)
- 💾 **Store in password manager** 
- 🔄 **Never reuse** passwords from other accounts
- 🚫 **Don't use browser's built-in password saving** for SSP Wallet

### Computer Security for SSP Wallet

**Critical Requirements:**
- 🔒 **Lock your computer** when not in use
- 🔄 **Enable automatic OS updates**
- 🛡️ **Run antivirus software** (Windows Defender is sufficient)
- 🚫 **Avoid using SSP Wallet on shared computers**

## 📱 SSP Key (Mobile App) Security

### Mobile App Protection

**Essential Mobile Settings:**
- 📱 **Enable strong screen lock** (6+ digit PIN or biometric)
- 🔐 **Enable biometric authentication** for SSP Key if available
- 🔄 **Keep mobile OS updated** to latest version
- ✅ **Download only from official app stores** (App Store, Google Play)

### SSP Key PIN Security

Your SSP Key PIN protects transaction approvals:
- 🔢 **Use 6+ digit PIN** (longer is better)
- 🤔 **Choose non-obvious numbers** (avoid birthdays, sequences)
- 🔄 **Change PIN periodically** for high-value wallets
- 🚫 **Never share PIN** with anyone

## 🔐 2-of-2 Security Implications

### Why Both Devices Matter

In SSP's 2-of-2 system:
- 🔑 **Both keys required** - Compromise of one device alone cannot steal funds
- ⚡ **Both devices needed** - Losing access to either device blocks transactions
- 🛡️ **Distributed security** - No single point of failure

### Device Loss Scenarios

**If you lose SSP Wallet device (computer):**
1. 🆕 **Install SSP Wallet** on new computer
2. 🔑 **Restore using seed phrase**
3. 🔄 **Re-sync with existing SSP Key**

**If you lose SSP Key device (mobile):**
1. 🆕 **Install SSP Key** on new mobile device  
2. 🔑 **Restore using SSP Key seed phrase**
3. 🔄 **Re-sync with existing SSP Wallet**

**If you lose both devices:**
1. 🔑 **Need both seed phrases** for complete recovery
2. 🆕 **Install both apps** on new devices
3. 🔄 **Restore and re-sync** both components

## 🚨 SSP-Specific Security Warnings

### Never Do These:
- ❌ **Don't use SSP Wallet on public computers** (libraries, internet cafes)
- ❌ **Don't approve transactions without verifying** details on mobile
- ❌ **Don't store seed phrases digitally** (photos, cloud storage, etc.)
- ❌ **Don't ignore sync errors** - they may indicate security issues
- ❌ **Don't share QR codes** from sync process with others

### Critical Security Practices:
- ✅ **Always verify transaction details** on SSP Key before approving
- ✅ **Keep both devices updated** and secure
- ✅ **Store seed phrases separately** in secure physical locations
- ✅ **Test recovery process** with small amounts first
- ✅ **Monitor transaction history** regularly on both devices

## 🛠️ Secure Usage Patterns

### Daily Usage
- 🔒 **Lock both devices** when not in use
- 📱 **Keep mobile device with you** for transaction approvals
- 👀 **Verify transaction details** match on both screens
- ⏰ **Don't approve transactions** you didn't initiate

### High-Value Transactions
- 🔍 **Double-check recipient addresses** 
- 💰 **Test with small amounts** first
- 🏠 **Use secure network** (avoid public WiFi)
- ⏱️ **Take time to review** - don't rush approvals

### Regular Maintenance
- 🔄 **Update both apps** when new versions available
- 🔍 **Review transaction history** weekly
- 🔑 **Verify seed phrase backups** are secure and accessible
- 📱 **Check device sync status** regularly

## Emergency Response

### If You Suspect Device Compromise

**Immediate Actions:**
1. 🧊 **Don't panic — one device alone cannot move funds.** SSP is a strict 2-of-2 multisig, so every transaction (including a rescue transfer) needs BOTH your SSP Wallet and SSP Key. A single compromised device cannot steal funds on its own.
2. 🆕 **Create a fresh SSP Wallet** with brand-new seed phrases on a clean device.
3. 🔄 **While you still control BOTH keys, use both devices together to move funds** to the new wallet — this co-sign requires both the SSP Wallet and the SSP Key (including the possibly-compromised one).
4. 🚨 **Only after the funds are safely moved, stop using / discard the old setup.** Do not wipe or abandon either old device before the funds have moved, or you may permanently freeze the 2-of-2.

### Getting Help
- 📝 **Report security issues** via [GitHub Issues](https://github.com/RunOnFlux/ssp-wallet/issues)
- 🔍 **Check transaction history** on both devices for unauthorized activity
- 📱 **Contact device manufacturer** for device-specific security concerns

---

**Remember: SSP's 2-of-2 multisignature design means both devices must be secure, but also provides protection against single device compromise.**