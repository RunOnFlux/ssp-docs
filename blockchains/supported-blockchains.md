# Supported Blockchains Overview

## Multi-Chain Architecture

SSP Wallet supports **15 mainnet blockchain networks** (plus testnets, Solana devnet and TRON Nile) with true multisignature capabilities across UTXO, EVM-compatible, Solana and TRON blockchains. Each network uses the optimal implementation:

- **UTXO Networks**: Native P2SH/P2WSH multisignature addresses
- **EVM Networks**: ERC-4337 smart-contract accounts (CREATE2-derived), controlled by a 2-of-2 Schnorr multisig — not EOAs
- **Account Abstraction**: The native and only EVM mode; every EVM address is an ERC-4337 smart account signed by both keys
- **Solana**: SSP's own self-initiating on-chain multisig program; funds live in a Program-Derived Address vault
- **TRON**: SSP's own smart-contract vault at a CREATE2 address computed from the members' keys and threshold — no creator, no admin, immutable; network fees sponsored by SSP

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
- **Derivation Path**: `m/48'/133'/0'/0'/0/0`
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
  - Very low transaction fees (typically well under 1 DOGE; auto-computed from transaction size at ~1100 sat/byte and hard-capped at 100 DOGE by config)
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

#### Bitcoin Cash (BCH)
- **Network**: Bitcoin Cash Mainnet
- **Address Type**: P2SH multisignature
- **Script Type**: `p2sh`
- **Derivation Path**: `m/48'/145'/0'/0'/0/0`
- **Features**:
  - CashAddr address format (`bitcoincash:` prefix)
  - Low fees and fast confirmations

#### Kaspa (KAS)
- **Network**: Kaspa Mainnet
- **Address Type**: P2SH multisignature (`kaspa:p…` addresses)
- **Script Type**: `p2sh`
- **Derivation Path**: `m/48'/111111'/0'/0'/0/0`
- **Multisig**: 2-of-2 over the sorted x-only (32-byte) Schnorr keys of SSP Wallet and SSP Key: `OP_2 <key> <key> OP_2 OP_CHECKMULTISIG`, hashed with BLAKE2b into the P2SH script
- **Features**:
  - Blocks every ~100 ms; transactions are confirmed once accepted by the virtual chain, usually within a few seconds
  - Fees are based on transaction *mass* (sompi per gram), including KIP-9 storage mass, which makes very small change outputs expensive — SSP folds tiny change into the fee instead of creating it
  - The transaction ID is known before signing, so both devices can show and track it from the start
  - Built and signed with [`@runonflux/kaspa-core`](https://github.com/RunOnFlux/kaspa-core), an open-source, audited pure-TypeScript library verified against the reference node

### Ethereum-Compatible (EVM) Networks

#### Ethereum (ETH)
- **Network**: Ethereum Mainnet
- **Chain ID**: 1 (`0x1`)
- **Address Type**: ERC-4337 smart-contract account (0x…), CREATE2-derived and controlled by a 2-of-2 Schnorr multisig — not a plain EOA
- **Account Abstraction**: Native and only EVM mode (no EOA option)
- **Features**:
  - Schnorr multisig message signing for enhanced security
  - EIP-712 structured data signing
  - WalletConnect v2 integration
  - Account Abstraction (ERC-4337) — native and mandatory for all EVM addresses (no EOA option)
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
- **Address Type**: ERC-4337 smart-contract account (0x…), CREATE2-derived and controlled by a 2-of-2 Schnorr multisig — not a plain EOA
- **Features**:
  - Very low transaction fees (~$0.01)
  - Fast 2-second block times
  - Full Ethereum Virtual Machine compatibility
  - Account Abstraction support
  - Extensive DeFi ecosystem

#### Binance Smart Chain (BSC)
- **Network**: BNB Smart Chain
- **Chain ID**: 56 (`0x38`)
- **Address Type**: ERC-4337 smart-contract account (0x…), CREATE2-derived and controlled by a 2-of-2 Schnorr multisig — not a plain EOA
- **Features**:
  - Low fees (typically $0.10-2.00)
  - High transaction throughput
  - 3-second block times
  - Binance ecosystem integration
  - Account Abstraction support

#### Avalanche (AVAX)
- **Network**: Avalanche C-Chain
- **Chain ID**: 43114 (`0xa86a`)
- **Address Type**: ERC-4337 smart-contract account (0x…), CREATE2-derived and controlled by a 2-of-2 Schnorr multisig — not a plain EOA
- **Features**:
  - Sub-second transaction finality
  - Low fees (~$0.10-5.00)
  - Unique Avalanche consensus mechanism
  - Account Abstraction support
  - Growing DeFi ecosystem

#### Base
- **Network**: Base Mainnet (Coinbase L2)
- **Chain ID**: 8453 (`0x2105`)
- **Address Type**: ERC-4337 smart-contract account (0x…), CREATE2-derived and controlled by a 2-of-2 Schnorr multisig — not a plain EOA
- **Features**:
  - Coinbase-backed Layer 2 solution
  - Very low fees (~$0.01-0.50)
  - Optimistic rollup technology
  - Growing consumer application ecosystem
  - Account Abstraction support


### Solana Network

#### Solana (SOL)
- **Network**: Solana Mainnet Beta
- **Address Type**: Program-Derived Address (PDA) vault, owned by SystemProgram
- **Multisig Program**: `SSPWVu7dtTDkZYmDx73StqV46PioSmdiNE7igpjHK1r`
- **Derivation**: The multisig PDA is derived from `sha256(sorted_members) + threshold`, so its address is knowable — and fundable — before any on-chain action
- **Features**:
  - SSP's own self-initiating multisig program (not Squads)
  - Permissionless registration; spends gated purely by the M-of-N threshold check
  - Immutable member set and threshold (migrate funds to rotate)
  - SPL token support, including USDC
  - Reproducible builds verifiable against the public source via `solana-verify`

> Devnet runs a **separate deployment** of the same program
> (`CisPSFTQoTnEqn5cUi1pgpfPp2xiTVRkK7eD5jBevxdX`) under its own keypair and
> upgrade authority. The program IDs differ by design — a vault address is
> derived from the program ID, so the two clusters never share addresses.

### TRON Network

#### TRON (TRX)
- **Network**: TRON Mainnet
- **Assets**: TRX and TRC-20 tokens, including USDT (`TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t`)
- **Address Type**: Smart-contract vault (`T…`, 34 characters) at a CREATE2 address computed from the members' keys and the threshold
- **Derivation Path**: `m/48'/195'/0'/0'/0/0` — SSP Wallet and SSP Key each derive their own secp256k1 key at this path; the two resulting TRON addresses, sorted, plus the threshold (2) define the vault
- **Multisig**: 2-of-2 in SSP Wallet; M-of-N (up to 40 keys) for [SSP Enterprise](../enterprise/creating-vaults.md) vaults. Each key signs the operation's TIP-712 typed-data digest with secp256k1 ECDSA, independently and in any order
- **Contracts**: vault factory, vault implementation and fee sponsor addresses are published at launch
- **Features**:
  - No creator, no admin key and no upgrade path: the vault contract is immutable, and only M of its members can move funds
  - Receives TRX, TRC-10 and TRC-20 tokens **before first use** — the address works from the moment SSP shows it, with no setup transaction
  - Deployed automatically inside your first send
  - Network fee sponsored by SSP and paid by the vault in the same transaction: SSP covers the TRON energy and bandwidth, and the vault pays a fee that both keys sign as part of the transaction — in **TRX by default**, or in **USDT if the vault holds no TRX**
  - One transaction can pay **up to 16 recipients**; the fee is charged once per transaction, not once per recipient
  - Never depends on SSP to move funds: anyone holding a fully signed operation can submit it from any funded TRON account — see [Moving TRON Vault Funds Without SSP](tron-without-ssp.md)
  - Built on [`@runonflux/tron-multisig`](https://github.com/RunOnFlux/tron-multisig): open-source contracts and a pure-TypeScript SDK

> **Tip: keep some TRX in the vault.** While the vault holds TRX, the network
> fee is paid in TRX, which is cheaper than paying it in USDT. Around 20 TRX
> covers a few sends.

#### TRON network fees

Each send gets a live quote. SSP Wallet shows the fee before you sign, and SSP Key shows it again before it co-signs. Typical amounts for a USDT transfer:

| Send | Fee paid in TRX (default) | Fee paid in USDT (vault holds no TRX) |
|------|---------------------------|---------------------------------------|
| From a vault that has already sent once | ~6–7 TRX | ~3.5 USDT |
| First send (also deploys the vault) | ~10 TRX | ~4.6 USDT |
| Recipient has never held the token | + ~3.5 TRX | + ~1.2 USDT |

- **Batch payments**: up to 16 recipients share one transaction and one fee, sized to the transaction's total cost, so each extra recipient costs much less than a separate send.
- **Hard ceiling**: SSP Key refuses any sponsored fee above **30 TRX** or **8 USDT**, and any fee that isn't payable to SSP's fee address.
- **Pay network fee myself (advanced)**: turn off sponsorship for a send and SSP Key submits it from its own TRON account instead (the app shows that account's address and balance). No fee goes to SSP; that account burns TRX for the energy — around 10 TRX for a USDT send, more for the first send, which also deploys the vault.
- **Sending max**: when the fee is paid in the token you are sending, SSP subtracts it from the maximum automatically.

#### Receiving on TRON

- **USDT and other TRC-20 tokens** can be sent to the vault at any time, including before its first send.
- **TRX** can be sent at any time too. The first TRX that reaches a brand-new address pays TRON's standard account-activation fee (about 1 TRX), charged to the **sender**; exchanges usually absorb it.
- The vault is a smart-contract address. If you are withdrawing from an exchange to it for the first time, send a small test amount first.
- TRON addresses look the same (`T…`) on mainnet and Nile, so an address alone never tells you which network it is on. Always check the selected chain before sending.

> **TRON Nile** is a separate network with its own contracts and its own
> TIP-712 chain ID, so a signature made for Nile can never be replayed on
> mainnet, and vice versa.

### Testnets

The following testnets are available:

- **Sepolia** - Ethereum testnet (not Goerli)
- **Amoy** - Polygon testnet (not Mumbai)
- **Bitcoin Testnet** - Bitcoin testing network
- **Bitcoin Signet** - Controlled Bitcoin test network
- **Flux Testnet** - Flux testing network
- **Solana Devnet** - Solana testing network
- **TRON Nile** - TRON testing network (TEST-TRX and test USDT)

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

### TRON Vault Operations

#### Operation Structure
```typescript
// A TRON vault operation, signed by M of the vault's keys as TIP-712 typed data
interface TronVaultOp {
  calls: Array<{        // 1 to 16 transfers, executed together or not at all
    to: string;         // recipient, or the TRC-20 token contract
    value: string;      // TRX in sun (1 TRX = 1,000,000 sun)
    data: string;       // TRC-20 transfer calldata, empty for TRX
    tokenId: string;    // TRC-10 token ID (0 otherwise)
    tokenValue: string; // TRC-10 amount (0 otherwise)
  }>;
  nonce: string;        // any unused number; nonces need not be in order
  deadline: string;     // unix seconds; the operation is invalid afterwards
  fee: {                // paid by the vault in the same transaction
    token: string;      // TRX, or USDT when the vault holds no TRX
    amount: string;     // 0 when you pay the network fee yourself
    recipient: string;
  };
}
```

#### Vault Multisig Model
- **Vault**: an immutable contract clone at a CREATE2 address that commits to the sorted signer addresses and the threshold
- **Signing**: every key signs the same TIP-712 digest on its own; there is no signing order and no nonce exchange between devices
- **Replay protection**: the digest binds the vault address and the TRON chain ID, and each nonce can be used once
- **Submission**: SSP's sponsor submits it by default; any TRON account can submit it directly

## 🌍 Network Selection & Switching

### Network Selection

SSP Wallet does not infer the network from a recipient address or token contract. The active network is selected explicitly:

- **User selection**: You pick the active chain in the wallet's chain selector; each chain has its own wallet and send view.
- **dApp request**: A connected dApp can request a switch via WalletConnect's `wallet_switchEthereumChain`, which SSP maps to a matching configured EVM chain and surfaces for your approval.

When you paste a recipient address, SSP validates it against the currently selected chain and warns you only if the address looks like it belongs to a different chain *type* (EVM / Solana / TRON / UTXO). Because all EVM chains share the same `0x` address format, SSP cannot detect which EVM network an address belongs to; likewise, TRON mainnet and Nile addresses look identical — always confirm you are on the intended network before sending, as transactions are irreversible.

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
| **TRON** | ~3 seconds | ~1 minute (SSP waits until the block is irreversible, ~19 blocks) | ~1 minute |

### Fee Comparison (Typical)
| Network | Transfer Fee | Token Transfer | Swap Fee | DeFi Interaction |
|---------|--------------|----------------|----------|------------------|
| **Bitcoin** | $1-10 | N/A | N/A | N/A |
| **Ethereum** | $5-50 | $10-100 | $20-200 | $50-500 |
| **Polygon** | $0.01-1 | $0.02-2 | $0.10-5 | $0.50-10 |
| **BSC** | $0.10-2 | $0.20-3 | $0.50-5 | $1-20 |
| **Avalanche** | $0.10-5 | $0.20-8 | $1-15 | $2-30 |
| **Base** | $0.01-2 | $0.05-5 | $0.20-10 | $1-25 |
| **TRON** | $1-2.50 (sponsored) | $2-3.50 (sponsored, USDT) | N/A | N/A |

*Fees vary based on network congestion and transaction complexity. TRON fees are the sponsored-send fee paid by the vault; see [TRON network fees](#tron-network-fees).*

## 🔍 Block Explorers

### Mainnet Explorers
- **Bitcoin**: [mempool.space](https://mempool.space), [blockstream.info](https://blockstream.info)
- **Ethereum**: [etherscan.io](https://etherscan.io)
- **Polygon**: [polygonscan.com](https://polygonscan.com)  
- **BSC**: [bscscan.com](https://bscscan.com)
- **Avalanche**: [snowtrace.io](https://snowtrace.io)
- **Base**: [basescan.org](https://basescan.org)
- **Flux**: [explorer.runonflux.io](https://explorer.runonflux.io)
- **Solana**: [explorer.solana.com](https://explorer.solana.com)
- **Kaspa**: [explorer.kaspa.org](https://explorer.kaspa.org)
- **TRON**: [tronscan.org](https://tronscan.org)

### Testnet Explorers
- **Bitcoin Testnet**: [mempool.space/testnet](https://mempool.space/testnet)
- **Sepolia**: [sepolia.etherscan.io](https://sepolia.etherscan.io)
- **Amoy**: [amoy.polygonscan.com](https://amoy.polygonscan.com)
- **Solana Devnet**: [explorer.solana.com?cluster=devnet](https://explorer.solana.com?cluster=devnet)
- **TRON Nile**: [nile.tronscan.org](https://nile.tronscan.org)

## 🛠️ Development & Testing

### Testnet Information
For testnet tokens and development resources, consult official network documentation.

### Network Configuration for Development
Development configurations are available in the SSP Wallet source code. Refer to the blockchains.ts file for accurate network parameters.

## 🔮 Networks Under Evaluation

### Research Phase
- **Cardano**: Multi-signature implementation research in progress
- **Cosmos**: Inter-blockchain communication and multisig evaluation
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
5. **TRON**: USDT (TRC-20) payments and batch payouts, with network fees sponsored by SSP
6. **Testnets**: Always test before mainnet transactions

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