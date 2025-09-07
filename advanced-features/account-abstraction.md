# Account Abstraction in SSP Wallet

## Overview

Account Abstraction (AA) is an **optional advanced feature** available in SSP Wallet for Ethereum-compatible networks. While SSP's **primary architecture** uses traditional 2-of-2 multisignature addresses, users can optionally enable Account Abstraction features on EVM chains for enhanced functionality.

**Important**: Most SSP Wallet users use traditional multisig addresses which work on all supported blockchains. Account Abstraction is an advanced option for users who need specific features like gasless transactions or smart contract wallet capabilities.

## 🏗️ SSP's Dual Architecture

### Primary Architecture: Traditional 2-of-2 Multisignature
```
✅ Used by: All SSP Wallet users
✅ Networks: Bitcoin, Litecoin, Dogecoin, Ethereum, Polygon, BSC, etc.
✅ Address Types: P2SH, P2WSH (Bitcoin-like), EOA (Ethereum-like)
✅ Security: True 2-of-2 multisignature protection
```

### Optional Feature: Account Abstraction
```
⚡ Used by: Advanced users who enable AA features
⚡ Networks: Ethereum, Polygon, BSC, Base, Avalanche only
⚡ Address Types: Smart contract wallets
⚡ Enhanced Features: Gasless transactions, batch operations
```

## 🔧 How SSP Implements Account Abstraction

### Blockchain Support with AA
Based on the actual codebase, SSP supports Account Abstraction on:

| Network | Chain ID | Factory Address | Entry Point |
|---------|----------|-----------------|-------------|
| **Ethereum** | 1 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |
| **Polygon** | 137 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |
| **BSC** | 56 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |
| **Base** | 8453 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |
| **Avalanche** | 43114 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |

### Schnorr Multisignature Integration

SSP's unique implementation combines Account Abstraction with Schnorr multisignatures:

```typescript
// From the actual SSP codebase
import * as aaSchnorrMultisig from '@runonflux/aa-schnorr-multisig-sdk';

// Create Schnorr signers for both keys
const signerOne = aaSchnorrMultisig.helpers.SchnorrHelpers.createSchnorrSigner(
  walletPrivateKey
);
const signerTwo = aaSchnorrMultisig.helpers.SchnorrHelpers.createSchnorrSigner(
  mobilePrivateKey
);

// Generate multisig signature
const publicKeys = [signerOne.getPubKey(), signerTwo.getPubKey()];
const publicNonces = [signerOne.getPubNonces(), signerTwo.getPubNonces()];
const combinedPublicKey = aaSchnorrMultisig.signers.Schnorrkel.getCombinedPublicKey(publicKeys);
```

## ⚡ Account Abstraction Features

### When to Use Account Abstraction

**Use traditional multisig for**:
- ✅ Bitcoin and UTXO-based transactions
- ✅ Standard Ethereum transactions
- ✅ Maximum compatibility across all networks
- ✅ Simplest user experience

**Use Account Abstraction for**:
- ⚡ Gasless transactions (someone else pays fees)
- ⚡ Batch multiple operations in one transaction
- ⚡ Smart contract wallet features
- ⚡ Advanced DeFi integrations

### Available AA Features

#### 1. User Operations
Instead of regular transactions, AA uses User Operations:

```typescript
// Example from SSP codebase
const userOp: UserOperationRequest_v6 = {
  sender: smartAccountAddress,
  nonce: await getNonce(),
  callData: encodedTransactionData,
  callGasLimit: '750000',
  verificationGasLimit: '100000',
  preVerificationGas: '21000',
  maxFeePerGas: baseFee.toString(),
  maxPriorityFeePerGas: priorityFee.toString(),
  signature: schnorrMultisigSignature
};
```

#### 2. Smart Account Client
```typescript
// From SSP's constructTx.ts
const smartAccountClient = createSmartAccountClient({
  transport: viemHttp(node),
  chain: viemChain,
  account: {
    address: contractAddress as `0x${string}`,
    async signMessage() {
      // SSP's Schnorr multisig signing
      return await generateSchnorrMultisigSignature();
    }
  }
});
```

## 🔍 Technical Implementation Details

### Gas Configuration
Based on the actual blockchain configurations:

```typescript
// Actual gas settings from SSP codebase
const gasSettings = {
  ethereum: {
    baseFee: 11, // 11 gwei
    priorityFee: 2, // 2 gwei
    gasLimit: 750000
  },
  polygon: {
    baseFee: 50, // 50 gwei
    priorityFee: 5, // 5 gwei
    gasLimit: 750000
  },
  base: {
    baseFee: 0.1, // 0.1 gwei
    priorityFee: 0.01, // 0.01 gwei
    gasLimit: 750000
  }
};
```

### Message Signing with Schnorr

SSP implements advanced message signing using Schnorr multisignatures:

```typescript
// Real implementation from schnorr-multisig-summary.md
const { signature: sigOne, challenge } = signerOne.signMultiSigMsg(
  message, publicKeys, publicNonces
);
const { signature: sigTwo } = signerTwo.signMultiSigMsg(
  message, publicKeys, publicNonces
);
const sSummed = aaSchnorrMultisig.signers.Schnorrkel.sumSigs([sigOne, sigTwo]);

// ABI encode for on-chain verification
const px = ethers.hexlify(combinedPublicKey.buffer.subarray(1, 33));
const parity = combinedPublicKey.buffer[0] - 2 + 27;
const sigData = abiCoder.encode(
  ['bytes32', 'bytes32', 'bytes32', 'uint8'],
  [px, challenge.buffer, sSummed.buffer, parity]
);
```

## 🎯 When to Enable Account Abstraction

### For Regular Users
**Recommendation**: Use traditional multisig addresses (default behavior)
- Works on all networks (Bitcoin, Ethereum, etc.)
- Simpler user experience
- Lower gas costs for basic operations
- Full 2-of-2 multisignature security

### For Advanced Users
**Consider AA when you need**:
- Gasless transactions
- Batch operations
- Smart contract wallet features
- Advanced DeFi integrations
- Custom transaction logic

## 🚀 Enabling Account Abstraction

Account Abstraction features are automatically available on supported EVM networks. Users can access them through:

1. **WalletConnect dApps** that support ERC-4337
2. **Advanced transaction options** in SSP Wallet
3. **Developer integrations** using SSP's AA SDK

## ⚠️ Important Limitations

### Account Abstraction is NOT available on:
- ❌ Bitcoin and UTXO-based networks
- ❌ Networks without AA infrastructure
- ❌ All operations (only specific AA features)

### Traditional Multisig is ALWAYS available:
- ✅ All supported networks
- ✅ All transaction types
- ✅ Standard wallet operations
- ✅ Maximum compatibility

## 📊 Performance Comparison

| Feature | Traditional Multisig | Account Abstraction |
|---------|---------------------|-------------------|
| **Networks** | All 15+ networks | EVM networks only |
| **Gas Cost** | Standard | Higher base cost |
| **Features** | Standard wallet | Advanced features |
| **Complexity** | Simple | More complex |
| **Compatibility** | Universal | AA-enabled dApps |

## 🔗 Integration Examples

### Using Traditional Multisig (Default)
```typescript
// Standard SSP Wallet transaction (works everywhere)
const transaction = {
  to: recipientAddress,
  value: amount,
  data: '0x'
};
// SSP handles 2-of-2 multisig automatically
```

### Using Account Abstraction (Optional)
```typescript
// Only available on EVM networks with AA support
const userOperation = {
  target: contractAddress,
  value: 0,
  data: encodeFunctionData({
    abi: contractAbi,
    functionName: 'batchTransfer',
    args: [recipients, amounts]
  })
};
// Enables advanced features like batching
```

## 📚 Additional Resources

### Documentation
- **[Schnorr Multisig Summary](../../../schnorr-multisig-summary.md)** - Technical implementation details
- **[ERC-4337 Standard](https://eips.ethereum.org/EIPS/eip-4337)** - Account Abstraction specification

### Developer Resources  
- **[SSP AA SDK](https://github.com/RunOnFlux/aa-schnorr-multisig-sdk)** - Schnorr multisig integration
- **[Account Abstraction Repository](https://github.com/RunOnFlux/account-abstraction)** - SSP's AA implementation

## Next Steps

- **[Schnorr Signatures & Message Signing](schnorr-signatures.md)** - Deep dive into SSP's signature system
- **[Smart Contract Interaction](smart-contract-interaction.md)** - Advanced contract integration  
- **[Developer Integration](../developers/getting-started.md)** - Build with SSP's AA features

---

**Account Abstraction is a powerful optional feature in SSP Wallet. Most users will benefit from SSP's secure traditional multisig addresses, while advanced users can enable AA features when needed for specific use cases.**