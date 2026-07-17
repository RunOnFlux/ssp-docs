# First-Time Setup Guide

## Welcome to SSP Wallet!

This guide will walk you through setting up your **SSP Wallet 2-of-2 multisignature system** in about **10 minutes**. You'll need two devices: a computer (or mobile browser) and a smartphone.

## 📋 What You'll Need

### Required Devices
- **🖥️ Computer/Laptop** with Chrome, Firefox, or Brave browser
- **📱 Smartphone** (iOS 15.1+ or Android 7+)
- **🌐 Internet Connection** on both devices
- **✏️ Pen and Paper** for seed phrase backup (physical backup only!)

### Estimated Time
- **Setup**: 10 minutes
- **First Transaction**: 2 minutes
- **Complete Tutorial**: 15 minutes total

## 🚀 Step 1: Install SSP Wallet (Browser Extension)

### Chrome/Brave Installation
1. **Visit Chrome Web Store**
   ```
   🔗 https://chromewebstore.google.com/detail/ssp-wallet/mgfbabcnedcejkfibpafadgkhmkifhbd
   ```
2. **Click "Add to Chrome"**
3. **Pin Extension** to toolbar for easy access
4. **Click SSP Wallet icon** to begin setup

### Firefox Installation
1. **Visit Firefox Add-ons**
   ```
   🔗 https://addons.mozilla.org/firefox/addon/ssp-wallet/
   ```
2. **Click "Add to Firefox"**
3. **Pin to toolbar** and **click icon** to start

## 📱 Step 2: Install SSP Key (Mobile App)

### iOS Installation
1. **Open App Store**
2. **Search "SSP Key"** or visit:
   ```
   🔗 https://apps.apple.com/app/ssp-key/id6463717332
   ```
3. **Install and open** the app

### Android Installation  
1. **Open Google Play Store**
2. **Search "SSP Key"** or visit:
   ```
   🔗 https://play.google.com/store/apps/details?id=io.runonflux.sspkey
   ```
3. **Install and open** the app

## 🔐 Step 3: Create Your First Wallet

### On SSP Wallet (Browser)
1. **Click "Create New Wallet"**
   
2. **Set Strong Password**
   ```
   ✅ At least 12 characters
   ✅ Mix of letters, numbers, symbols
   ✅ Unique to SSP Wallet
   ✅ Store in password manager
   ```

3. **Write Down Seed Phrase**
   ```
   ⚠️  CRITICAL SECURITY STEP
   ✅ Write on paper (not digitally)
   ✅ Double-check spelling
   ✅ Store in secure location
   ✅ Make multiple copies
   ```

4. **Pass the Word Challenge**
   - The wallet asks you to pick specific words of your phrase (e.g., "Word #2") from a set of choices — this verifies your backup is real

5. **Make It Yours**
   - Give your wallet a name and accent color so you can spot it at a glance

6. **Wallet Created Successfully!**
   - You'll see your empty wallet dashboard

### Important: First Device Complete ✅
Your browser extension now holds **Key #1** of your 2-of-2 multisig system.

## 📲 Step 4: Set Up SSP Key (Mobile)

### On SSP Key App
1. **Open SSP Key App**

2. **Choose "Create New Key"**

3. **Set PIN or Biometric**
   ```
   ✅ Use 6+ digit PIN
   ✅ Enable Face ID/Touch ID if available
   ✅ Choose secure but memorable PIN
   ```

4. **Write Down Mobile Seed Phrase**
   ```
   ⚠️  SECOND CRITICAL STEP
   ✅ This is DIFFERENT from wallet seed
   ✅ Write on paper separately
   ✅ Store in different location from wallet seed
   ✅ Both seeds needed for full recovery
   ```

5. **Confirm Mobile Seed Phrase**
   - Pass the 3-word challenge: pick the requested words of your phrase from the choices shown (this replaces the old "I backed it up" switch)

6. **Key Created Successfully!**
   - You'll see "Ready to Sync" screen

### Important: Second Device Complete ✅
Your mobile app now holds **Key #2** of your 2-of-2 multisig system.

## 🔄 Step 5: Synchronize Your Devices

This step connects your two keys to create secure multisig addresses.

### Start Synchronization
1. **On SSP Wallet (Browser)**
   ```
   1. The pairing screen shows a large QR code
   2. Optional: select extra chains to activate along with Bitcoin
      (one approval on the Key activates them all)
   3. Keep this screen open — it shows live pairing status
   ```

2. **On SSP Key (Mobile)**
   ```
   1. Tap "Scan Code"
   2. Point camera at QR code on computer screen
   3. Slide to approve the synchronisation,
      then confirm with biometrics or password
   ```

### Automatic Sync Process
```
📱 SSP Key scans QR code
    ↓
🔄 Devices exchange public keys securely
    ↓  
🔐 Multisig addresses generated
    ↓
🔑 Both devices show the same 6-word verification code
    ↓
✅ Sync Complete!
```

### Verify Synchronization (Important!)
Both devices show a **6-word verification code**, each derived independently from the synced keys:

- 📷 In SSP Key, tap **"Scan wallet to verify"** and scan the wallet's verification QR — or compare the 6 words by eye
- ✅ **Words match** → your pairing is genuine; no one (not even the relay) tampered with it. Click "They match — continue"
- 🚨 **Words differ** → STOP. Do not use the wallet — choose "They don't match" and re-pair. See [Pairing Verification Code](../getting-started-with-ssp-key/pairing-verification-code.md)

Both devices should then show:
- ✅ **"Synchronization Successful"**  
- ✅ **Same wallet addresses** displayed

## 🎉 Step 6: Your First Transaction Test

Let's verify everything works with a small test transaction.

### Get Your Wallet Address
1. **Click "Receive" in SSP Wallet**
2. **Copy your Bitcoin address** (starts with 'bc1')
3. **Share this address** to receive funds

### Send a Small Test Amount
1. **Get small amount of Bitcoin** (from exchange/friend)
2. **Send to your SSP address** 
3. **Wait for confirmation** (usually 10-60 minutes)

### Test Sending
1. **Click "Send" in SSP Wallet** — sending is a guided 3-step flow (Details → Review → Approve)
2. **Enter recipient address** — an address you control (e.g. your exchange deposit address or a second wallet you own). Do **not** enter a testnet or faucet address: on mainnet a testnet address (starting `tb1`) is invalid and will be rejected, and a faucet is where you *obtain* testnet coins, not a destination to send to.
3. **Enter small amount** ($1-5 worth)
4. **Review transaction details** — the review step always shows the full recipient address; the fee is automatic (override it if you like)
5. **Continue to approval**

### Mobile Approval Process
```
🖥️  SSP Wallet creates transaction
    ↓
📱 SSP Key receives notification and decodes it on-device
    ↓
👤 You verify the plain-language summary, then slide to approve
    ↓
✅ Transaction broadcasts to network
```

### Success! 🚀
If the test transaction completes, your SSP Wallet setup is **perfect**!

## 🛡️ Critical Security Steps

### Secure Your Seed Phrases
```
⚠️  NEVER store seed phrases digitally
✅ Write on physical paper
✅ Store in different secure locations  
✅ Consider safety deposit box for large amounts
✅ Make multiple copies
❌ No photos, no cloud storage, no password managers
```

### Device Security
```
✅ Enable screen locks on both devices
✅ Use strong, unique passwords
✅ Enable automatic updates
✅ Install reputable antivirus software
✅ Never lend devices to others for crypto operations
```

### Backup Verification
**Test your backups NOW while setup is fresh in memory:**

1. **Record your current wallet addresses**
2. **Reset SSP Wallet** (only do this once your seed phrases are safely backed up)
3. **Restore from your wallet seed phrase**
4. **Re-sync with SSP Key** — scan the wallet's QR code. This is required first: after a restore the wallet holds only Key #1, so it **cannot show any multisig addresses until it re-syncs** with your SSP Key (Key #2)
5. **Verify the same addresses generate** — once re-synced, the addresses should match those you recorded in step 1

## 🚨 Emergency Information

### If Something Goes Wrong
- **Lost Device**: Use seed phrase to restore on new device
- **Forgotten Password**: Restore wallet using seed phrase
- **Sync Problems**: Try manual QR code sync or restart both apps
- **Technical Issues**: [Report via GitHub Issues](https://github.com/RunOnFlux/ssp-wallet/issues)

### Recovery Process Overview
```
🔑 Wallet Seed Phrase → Restores SSP Wallet
    +
🔑 Key Seed Phrase → Restores SSP Key
    =
🔄 Re-sync → Full wallet recovery
```

## ✅ Setup Complete Checklist

Congratulations! Verify you've completed everything:

- [ ] ✅ SSP Wallet installed and working
- [ ] ✅ SSP Key installed and working  
- [ ] ✅ Strong passwords/PINs set
- [ ] ✅ Both seed phrases written down and stored securely
- [ ] ✅ Devices successfully synchronized
- [ ] ✅ Test transaction completed
- [ ] ✅ Backup recovery process understood
- [ ] ✅ Security best practices reviewed

## 🎓 Next Steps

### Learn More
- **[Interactive Website Tutorial](https://sspwallet.io/guide)** - Complete video setup guide with detailed instructions
- **In-App Tutorial** - Available within SSP Wallet for interactive guidance
- **[Send Your First Transaction](../getting-started-with-ssp-wallet/transaction/send.md)** - Detailed transaction guide
- **[Security Best Practices](../security/security-overview.md)** - Comprehensive security guide

### Start Using SSP Wallet
- **Add More Cryptocurrencies** - Bitcoin, Ethereum, and 12+ blockchains supported
- **Connect to dApps** - Use WalletConnect for DeFi and NFT platforms
- **Set Up Contacts** - Save frequently used addresses
- **Explore Advanced Features** - Account abstraction, batch transactions

### Get Additional Help
- **Interactive Website Tutorial**: [Complete Setup Guide](https://sspwallet.io/guide) - Video walkthrough with step-by-step instructions
- **In-App Tutorial**: Available within SSP Wallet after installation - Interactive setup guidance
- **GitHub Support**: [Report Issues](https://github.com/RunOnFlux/ssp-wallet/issues) for technical support

## 🎯 Pro Tips for New Users

### Optimization Tips
- **Pin browser extension** to toolbar for quick access
- **Enable mobile notifications** for transaction approvals
- **Use contacts feature** for frequently sent addresses
- **Set up price alerts** for your favorite cryptocurrencies

### Common Beginner Mistakes to Avoid
- ❌ Don't store seed phrases in password managers
- ❌ Don't use public WiFi for large transactions
- ❌ Don't skip address verification when sending
- ❌ Don't lose patience with blockchain confirmation times
- ❌ Don't forget to test small amounts first

---

**🎉 Welcome to the most secure crypto wallet experience! You're now part of the SSP community with true 2-of-2 multisignature security.**

Need help? Visit our **[Interactive Website Tutorial](https://sspwallet.io/guide)** for video guidance, use the **in-app tutorial** within SSP Wallet, or **[report issues on GitHub](https://github.com/RunOnFlux/ssp-wallet/issues)** for technical support.