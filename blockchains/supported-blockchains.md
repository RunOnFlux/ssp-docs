# Supported Blockchains Overview

## Multi-Chain Architecture

SSP Wallet supports **15 blockchain networks** (plus testnets) with true multisignature capabilities across UTXO and EVM-compatible blockchains. Each network uses the optimal implementation:

- **UTXO Networks**: Native P2SH/P2WSH multisignature addresses
- **EVM Networks**: Standard EOA addresses with Schnorr multisig message signing
- **Account Abstraction**: Optional smart contract wallets on supported EVM chains

All networks maintain the same **2-of-2 multisignature security model** using BIP48 key derivation.

## 🔗 Supported Networks

### Bitcoin-Based (UTXO) Networks

#### Bitcoin (BTC)
- **Network**: Bitcoin Mainnet
- **Address Type**: P2WSH (Pay-to-Witness-Script-Hash) multisignature
- **Derivation Path**: `m/48'/0'/0'/2'/0/0`
- **Script Type**: `p2wsh` (SegWit native)
- **Features**: 
  - Native 2-of-2 multisignature addresses
  - Replace-by-Fee (RBF) support
  - SegWit for lower fees and faster confirmation
  - Bech32 address format (`bc1...`)

```typescript
// Actual Bitcoin configuration from blockchains.ts
const btc = {
  id: 'btc',
  libid: 'bitcoin',
  name: 'Bitcoin',
  symbol: 'BTC',
  decimals: 8,
  slip: 0, // BIP44 coin type
  scriptType: 'p2wsh', // SegWit multisig
  bech32: 'bc1',
  dustLimit: 546, // 546 satoshi minimum
  minFeePerByte: 1,
  feePerByte: 100, // Default fee rate
  rbf: true // Replace-by-Fee enabled
};
```

#### Bitcoin Testnet & Signet
- **Bitcoin Testnet**: Testing network with same features as mainnet
  - **Bech32 Prefix**: `tb1` 
  - **Script Type**: `p2wsh`
  - **Fee Rate**: 5 sat/vbyte (lower for testing)
- **Bitcoin Signet**: Controlled test network
  - **More Reliable**: Consistent block production
  - **Same Configuration**: As testnet but different network

#### Litecoin (LTC)
- **Network**: Litecoin Mainnet  
- **Address Type**: P2WSH multisignature
- **Derivation Path**: `m/48'/2'/0'/2'/0/0`
- **Script Type**: `p2wsh`
- **Features**:
  - 2.5-minute block times (4x faster than Bitcoin)
  - Lower transaction fees
  - Scrypt mining algorithm
  - Bech32 addresses (`ltc1...`)

#### Zcash (ZEC)
- **Network**: Zcash Mainnet
- **Address Type**: P2SH multisignature (transparent addresses)
- **Script Type**: `p2sh`
- **Derivation Path**: `m/48'/133'/0'/2'/0/0`
- **Features**:
  - Transparent transactions only (t-addresses)
  - Transaction expiry (1 hour default)
  - Overwinter and Sapling protocol support

#### Dogecoin (DOGE) 
- **Network**: Dogecoin Mainnet
- **Address Type**: P2SH multisignature
- **Script Type**: `p2sh`
- **Derivation Path**: `m/48'/3'/0'/0'/0/0`
- **Features**:
  - Very low transaction fees (1000+ DOGE typical fee)
  - High transaction throughput
  - 1-minute block times

#### Ravencoin (RVN)
- **Network**: Ravencoin Mainnet
- **Address Type**: P2SH multisignature
- **Script Type**: `p2sh`
- **Derivation Path**: `m/48'/175'/0'/0'/0/0`
- **Features**:
  - Asset creation and transfer capabilities
  - 1-minute block times
  - ASIC-resistant mining (KAWPOW algorithm)
  - Built-in asset layer

#### Flux (FLUX)
- **Network**: Flux Mainnet + Testnet
- **Address Type**: P2SH multisignature
- **Script Type**: `p2sh`
- **Derivation Path**: `m/48'/19167'/0'/0'/0/0` (Flux-specific coin type)
- **Features**:
  - Native integration with Flux decentralized cloud
  - Transaction expiry (30 blocks, ~1 hour)
  - Built-in message capability (80 bytes)
  - Special integration with SSP ecosystem

### Ethereum-Compatible (EVM) Networks

#### Ethereum (ETH)
- **Network**: Ethereum Mainnet
- **Chain ID**: 1 (`0x1`)
- **Address Type**: Standard EOA addresses (0x...) + Schnorr multisig message signing
- **Account Abstraction**: Optional smart contract wallets available
- **Features**:
  - Schnorr multisig message signing for enhanced security
  - EIP-712 structured data signing
  - WalletConnect v2 integration
  - Optional Account Abstraction (ERC-4337)
  - DeFi protocol compatibility

```typescript
// Actual Ethereum configuration from blockchains.ts
const eth = {
  id: 'eth',
  libid: 'mainnet',
  chainId: '1',
  chainType: 'evm',
  symbol: 'ETH',
  decimals: 18,
  backend: 'alchemy',
  // Account Abstraction support
  factoryAddress: '0x3974821943e9cA3549744D910999332eE387Fda4',
  entrypointAddress: '0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789',
  baseFee: 11, // 11 gwei
  priorityFee: 2, // 2 gwei
  gasLimit: 750000
};
```

#### Polygon (POL)
- **Network**: Polygon Mainnet
- **Chain ID**: 137 (`0x89`)
- **Address Type**: Standard EOA + Schnorr multisig message signing
- **Features**:
  - Very low transaction fees (~$0.01)
  - Fast 2-second block times
  - Full Ethereum Virtual Machine compatibility
  - Account Abstraction support
  - Extensive DeFi ecosystem

#### Binance Smart Chain (BSC)
- **Network**: BNB Smart Chain
- **Chain ID**: 56 (`0x38`)
- **Address Type**: Standard EOA + Schnorr multisig message signing
- **Features**:
  - Low fees (typically $0.10-2.00)
  - High transaction throughput
  - 3-second block times
  - Binance ecosystem integration
  - Account Abstraction support

#### Avalanche (AVAX)
- **Network**: Avalanche C-Chain
- **Chain ID**: 43114 (`0xa86a`)
- **Address Type**: Standard EOA + Schnorr multisig message signing
- **Features**:
  - Sub-second transaction finality
  - Low fees (~$0.10-5.00)
  - Unique Avalanche consensus mechanism
  - Account Abstraction support
  - Growing DeFi ecosystem

#### Base
- **Network**: Base Mainnet (Coinbase L2)
- **Chain ID**: 8453 (`0x2105`)
- **Address Type**: Standard EOA + Schnorr multisig message signing
- **Features**:
  - Coinbase-backed Layer 2 solution
  - Very low fees (~$0.01-0.50)
  - Optimistic rollup technology
  - Growing consumer application ecosystem
  - Account Abstraction support


### Testnets

The following testnets are available:

- **Sepolia** - Ethereum testnet (not Goerli)
- **Amoy** - Polygon testnet (not Mumbai)
- **Bitcoin Testnet** - Bitcoin testing network
- **Bitcoin Signet** - Controlled Bitcoin test network
- **Flux Testnet** - Flux testing network

## 🔧 Network-Specific Features

### UTXO Networks (Bitcoin-based)

#### Transaction Construction
```typescript
// UTXO transaction structure
interface UTXOTransaction {
  inputs: Array<{
    txid: string;
    vout: number;
    value: number;
    scriptSig: string;
  }>;
  outputs: Array<{
    address: string;
    value: number;
  }>;
  fee: number;
  rbf: boolean; // Replace-by-Fee
}
```

#### Multi-Signature Implementation
- **2-of-2 P2SH**: Standard multisig with script hash
- **2-of-2 P2WSH**: SegWit wrapped multisig (lower fees)
- **Address Generation**: Both public keys required
- **Signing Process**: Sequential signing by both keys

### EVM Networks (Ethereum-compatible)

#### Transaction Structure
```typescript
// EVM transaction structure
interface EVMTransaction {
  to: string;
  value: string;
  data: string;
  gasLimit: string;
  gasPrice: string; // or maxFeePerGas/maxPriorityFeePerGas for EIP-1559
  nonce: number;
  chainId: number;
}
```

#### Smart Contract Integration
- **Contract Calls**: Direct smart contract interaction
- **Token Transfers**: ERC-20, ERC-721, ERC-1155 support
- **DeFi Protocols**: Uniswap, Aave, Compound integration
- **Account Abstraction**: ERC-4337 compatible wallets

## 🌍 Network Selection & Switching

### Automatic Network Detection
SSP Wallet automatically detects the appropriate network based on:
- Transaction recipient address format
- Token contract address
- User preference settings
- dApp requested network

### Manual Network Switching
```typescript
// Request network switch
await sspProvider.request({
  method: 'wallet_switchEthereumChain',
  params: [{ chainId: '0x89' }] // Polygon
});

// Add custom network
await sspProvider.request({
  method: 'wallet_addEthereumChain',
  params: [{
    chainId: '0x2105', // Base
    chainName: 'Base',
    nativeCurrency: {
      name: 'Ether',
      symbol: 'ETH', 
      decimals: 18
    },
    rpcUrls: ['https://mainnet.base.org'],
    blockExplorerUrls: ['https://basescan.org']
  }]
});
```

## ⚡ Network Performance & Fees

### Transaction Speed Comparison
| Network | Block Time | Typical Confirmation Time | Finality |
|---------|------------|-------------------------|----------|
| **Bitcoin** | ~10 minutes | 30-60 minutes (3-6 blocks) | 6+ blocks |
| **Ethereum** | ~12 seconds | 1-5 minutes (5-25 blocks) | ~15 minutes |
| **Polygon** | ~2 seconds | 30-60 seconds | ~30 seconds |
| **BSC** | ~3 seconds | 30-90 seconds | ~45 seconds |
| **Avalanche** | ~2 seconds | 10-30 seconds | Sub-second |
| **Base** | ~2 seconds | 30-60 seconds | ~2 minutes |

### Fee Comparison (Typical)
| Network | Transfer Fee | Token Transfer | Swap Fee | DeFi Interaction |
|---------|--------------|----------------|----------|------------------|
| **Bitcoin** | $1-10 | N/A | N/A | N/A |
| **Ethereum** | $5-50 | $10-100 | $20-200 | $50-500 |
| **Polygon** | $0.01-1 | $0.02-2 | $0.10-5 | $0.50-10 |
| **BSC** | $0.10-2 | $0.20-3 | $0.50-5 | $1-20 |
| **Avalanche** | $0.10-5 | $0.20-8 | $1-15 | $2-30 |
| **Base** | $0.01-2 | $0.05-5 | $0.20-10 | $1-25 |

*Fees vary based on network congestion and transaction complexity*

## 🔍 Block Explorers

### Mainnet Explorers
- **Bitcoin**: [mempool.space](https://mempool.space), [blockstream.info](https://blockstream.info)
- **Ethereum**: [etherscan.io](https://etherscan.io)
- **Polygon**: [polygonscan.com](https://polygonscan.com)  
- **BSC**: [bscscan.com](https://bscscan.com)
- **Avalanche**: [snowtrace.io](https://snowtrace.io)
- **Base**: [basescan.org](https://basescan.org)
- **Flux**: [explorer.runonflux.io](https://explorer.runonflux.io)

### Testnet Explorers
- **Bitcoin Testnet**: [mempool.space/testnet](https://mempool.space/testnet)
- **Sepolia**: [sepolia.etherscan.io](https://sepolia.etherscan.io)
- **Amoy**: [amoy.polygonscan.com](https://amoy.polygonscan.com)

## 🛠️ Development & Testing

### Testnet Information
For testnet tokens and development resources, consult official network documentation.

### Network Configuration for Development
Development configurations are available in the SSP Wallet source code. Refer to the blockchains.ts file for accurate network parameters.

## 🔮 Networks Under Evaluation

### Research Phase
- **Cardano**: Multi-signature implementation research in progress
- **Cosmos**: Inter-blockchain communication and multisig evaluation
- **TRON**: TRC-20 and native multisig assessment
- **NEAR**: Account model and multisig capabilities evaluation

For detailed information, see [Networks Under Evaluation](networks-under-evaluation.md).

### Community Requests
Submit requests for new network support on [GitHub](https://github.com/RunOnFlux/ssp-wallet/issues).

## 🎯 Network Selection Best Practices

### For Users
1. **Bitcoin**: Long-term storage, high-value transfers
2. **Ethereum**: DeFi, NFTs, complex smart contracts  
3. **Polygon/BSC**: Frequent trading, gaming, low-value transfers
4. **Avalanche/Base**: DeFi with better UX, moderate fees
5. **Testnets**: Always test before mainnet transactions

### For Developers
1. **Test on testnets first** before mainnet deployment
2. **Consider fee structures** for your application's use case
3. **Plan for network congestion** during high-traffic periods
4. **Implement network switching** for better user experience
5. **Monitor network health** and have fallback options

## 📊 Network Health Monitoring

### Real-Time Status
SSP Wallet displays real-time sync status for all supported networks within the application.

### Performance Metrics
- Block height synchronization
- Transaction pool status  
- Network fee recommendations
- Connection health indicators

## Next Steps

- **[Bitcoin & UTXO Management](bitcoin-utxo.md)** - Deep dive into UTXO networks
- **[Ethereum & EVM Networks](ethereum-evm.md)** - EVM-specific features and optimization
- **[Gas Optimization Strategies](gas-optimization.md)** - Reduce transaction costs
- **[Custom Token Management](custom-tokens.md)** - Add and manage custom tokens