# Common Issues & Solutions

## Overview

This guide covers frequently encountered issues with SSP Wallet based on actual user experiences and known technical limitations. All solutions are based on SSP's actual implementation.

## 🔄 Device Synchronization Issues

### Problem: "Cannot sync SSP Wallet and SSP Key"

**SSP's 2-of-2 Architecture Requirements:**
- Browser extension (SSP Wallet) must sync with mobile app (SSP Key)
- Both devices need public key exchange to generate multisig addresses
- Synchronization can be done via QR code or manual input

**Solutions:**

#### **QR Code Sync Method**
1. **SSP Wallet**: Display QR code during sync process
2. **SSP Key**: Use "Scan to Sync" feature to scan QR code
3. **Verify**: Both devices show same wallet addresses after sync

#### **Manual Input Method**
1. **SSP Key**: Go to Settings → Export Public Key
2. **Copy Extended Public Key** from SSP Key
3. **SSP Wallet**: Choose "Manual Input" and paste the key
4. **Complete sync** process

### Problem: "Addresses don't match between devices"

**Root Cause**: Incomplete or failed synchronization

**Solution:**
1. Verify both devices completed sync process
2. Check that both devices show "Connected" or "Synced" status
3. If addresses differ, repeat synchronization process
4. Ensure you're viewing the same blockchain network on both devices

## 🔑 Authentication Issues

### Problem: "Cannot unlock SSP Wallet"

**Password Recovery:**
1. Click "Restore Wallet" option
2. Enter your 12-word seed phrase
3. Set new password
4. Re-sync with SSP Key

### Problem: "Cannot unlock SSP Key"

**PIN/Biometric Recovery:**
- If PIN forgotten: Use seed phrase to restore SSP Key on same or new device
- If biometric fails: Use PIN as fallback
- Complete recovery: Uninstall/reinstall app and restore from seed phrase

## 💸 Transaction Issues

### Problem: "Transaction not completing"

**SSP's 2-of-2 Requirement:**
- Every transaction needs approval from BOTH devices
- SSP Wallet initiates transaction
- SSP Key must approve transaction
- Both signatures required for blockchain broadcast

**Solutions:**
1. Ensure both devices are online and synced
2. Check that SSP Key receives transaction notification
3. Approve transaction on SSP Key mobile app
4. Wait for blockchain confirmation (varies by network)

### Problem: "Transaction stuck or pending"

**Blockchain-Level Issues:**
- Bitcoin: Transaction may be waiting in mempool
- Ethereum: May need higher gas fee for faster confirmation
- Network congestion can delay confirmations

**Solutions:**
1. Wait for network confirmation (can take minutes to hours)
2. Check transaction status on blockchain explorer
3. For Ethereum: Some wallets support transaction speed-up (if implemented)

## 📱 Mobile App Issues

### Problem: "SSP Key app crashes"

**Basic Solutions:**
1. Force close app and restart
2. Restart mobile device  
3. Update to latest version from app store
4. If persistent: Uninstall, reinstall, restore from seed

### Problem: "Not receiving transaction notifications"

**Notification Settings:**
1. Enable notifications for SSP Key in device settings
2. Ensure app has permission to send notifications
3. Check that device is not in Do Not Disturb mode

## 💻 Browser Extension Issues

### Problem: "SSP Wallet extension not working"

**Browser Extension Solutions:**
1. Check extension is enabled in browser settings
2. Update to latest version
3. Try disabling other extensions temporarily
4. Clear browser cache and cookies
5. If corrupted: Remove extension, reinstall, restore from seed

### Problem: "Extension data lost"

**Recovery Process:**
1. Click "Restore Wallet" in extension
2. Enter 12-word seed phrase
3. Set new password
4. Re-sync with SSP Key mobile app

## 🌐 Network Issues

### Problem: "Cannot connect to blockchain networks"

**Network Connectivity:**
- SSP Wallet connects to blockchain RPC endpoints
- Some networks may have intermittent connectivity
- Corporate firewalls may block blockchain connections

**Solutions:**
1. Check internet connection
2. Try different network (mobile data vs WiFi)
3. Contact IT department if using corporate network
4. Wait and retry if blockchain network is experiencing issues

## 🔧 Recovery Scenarios

### Device Loss Recovery

**Lost Computer (SSP Wallet):**
1. Install SSP Wallet on new computer/browser
2. Restore using wallet seed phrase
3. Re-sync with existing SSP Key

**Lost Mobile Device (SSP Key):**
1. Install SSP Key on new mobile device
2. Restore using SSP Key seed phrase  
3. Re-sync with existing SSP Wallet

**Lost Both Devices:**
1. Need BOTH seed phrases for complete recovery
2. Install SSP Wallet and restore with wallet seed phrase
3. Install SSP Key and restore with key seed phrase
4. Sync both restored devices

### Important Recovery Notes
- Each device has its own seed phrase
- Both seed phrases needed for full wallet recovery
- Cannot recover if either seed phrase is lost
- Test recovery process with small amounts

## 📞 Getting Help

### Official Support Channels

**Primary Support:**
- **Support Center**: [sspwallet.io/support](https://sspwallet.io/support) - FAQ and guides
- **Support Tickets**: [support.runonflux.io](https://support.runonflux.io) - Direct support
- **Contact Form**: [sspwallet.io/contact](https://sspwallet.io/contact) - Message support team

**Community Support:**
- **Discord**: [discord.gg/runonflux](https://discord.gg/runonflux) - Community help
- **GitHub Issues**: [github.com/RunOnFlux/ssp-wallet/issues](https://github.com/RunOnFlux/ssp-wallet/issues) - Bug reports

### Before Contacting Support

**Gather This Information:**
- Device types and operating system versions
- SSP Wallet and SSP Key version numbers
- Browser type and version (for SSP Wallet)
- Exact error messages or screenshots
- Steps you took before the issue occurred

### Security Issues
- **Report via**: [GitHub Security](https://github.com/RunOnFlux/ssp-wallet/security)
- **Critical Issues**: Use GitHub's security advisory feature

## Prevention Best Practices

### Regular Maintenance
- Keep both apps updated to latest versions
- Verify seed phrase backups are secure and accessible
- Test sync functionality periodically
- Keep devices charged during transaction operations

### Security Best Practices  
- Store seed phrases securely offline (paper, never digital)
- Use strong passwords/PINs on both devices
- Never share seed phrases with anyone
- Always verify transaction details before approving

---

**Note**: This guide focuses on issues specific to SSP Wallet's 2-of-2 multisignature architecture. For general cryptocurrency or blockchain issues, consult respective network documentation.