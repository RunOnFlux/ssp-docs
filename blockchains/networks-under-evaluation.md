# Networks Under Evaluation

SSP Wallet continuously evaluates new blockchain networks for integration, focusing on those that can support secure multisignature implementations while maintaining the core security principles of the SSP ecosystem.

## Current Evaluation Status

The following networks are actively being researched and evaluated for future SSP Wallet support:

### 🌐 Solana (SOL) — Live on Mainnet
**Status**: Live on **mainnet** (program `SSPWVu7dtTDkZYmDx73StqV46PioSmdiNE7igpjHK1r`) and **devnet** (program `CisPSFTQoTnEqn5cUi1pgpfPp2xiTVRkK7eD5jBevxdX`). Solana has graduated from evaluation to a shipped integration.

The two clusters run **separate deployments of the same program**, each with its own keypair and upgrade authority, so the program IDs deliberately differ. The mainnet build is reproducible and verified against the public source with `solana-verify`.

Solana uses **SSP's own self-initiating multisig program** — **not** Squads, and with no separate bootloader. The multisig account is a Program-Derived Address deterministically derived from `sha256(sorted_members) + threshold`, so registration is permissionless and spends are gated purely by the threshold check (Bitcoin-P2WSH-style).

#### Delivered properties
- **Trustless initialization**: No single "creator" signer — anyone can register the deterministic multisig.
- **Front-running resistance**: The config is bound to the member/threshold hash.
- **Immutable configuration**: Members and threshold are fixed at creation (to "rotate," migrate funds to a new multisig).
- **PDA vault model**: A separate SystemProgram-owned vault holds funds; the program authorizes spends via `invoke_signed`.
- **Verifiable builds**: The deployed mainnet bytecode is reproducible from source and independently checkable with `solana-verify verify-from-repo`.

See [Supported Blockchains](supported-blockchains.md) for live integration details.

---

### ⚡ TRON (TRX) — Supported
**Status**: Supported on **mainnet** and the **Nile** testnet, for TRX and TRC-20 tokens including USDT. TRON has graduated from evaluation to a shipped integration. The vault factory, vault implementation and fee sponsor contract addresses are published at launch.

TRON uses **SSP's own smart-contract vault**, not TRON's native account-permission multisig. Every vault is an immutable contract clone at a **CREATE2 address computed from the members' keys and the threshold** — the same trust property as a Bitcoin P2WSH address or SSP's Solana PDA.

#### Why not TRON's native multisig
TRON's built-in account permissions were evaluated and rejected:
- A single **creator key** always exists, and creating the account and setting its permissions can never happen in one atomic step.
- Each permission is capped at **5 keys**.
- The permission-update transaction is exactly what TRON's most common multisig scam asks victims to sign.

#### Delivered properties
- **Trustless initialization**: No creator and no setup transaction — the address is a pure function of the member keys and the threshold, and it can receive TRX, TRC-10 and TRC-20 tokens before it exists on-chain.
- **Front-running resistance**: Deployment is permissionless and only fixes code the address already committed to, so nobody can deploy a different owner set at your address.
- **Immutable configuration**: No admin, no owner, no upgrade path. Members and threshold are fixed (to "rotate," migrate funds to a new vault).
- **Sponsored fees**: SSP covers the network resources, and the vault pays a signed fee in the same transaction — TRX by default, USDT if the vault holds no TRX.
- **Escape hatch**: Any TRON account can submit a fully signed operation directly to the vault, without SSP. See [Moving TRON Vault Funds Without SSP](tron-without-ssp.md).

See [Supported Blockchains](supported-blockchains.md#tron-network) for live integration details.

---

The following networks are being researched and evaluated for future SSP Wallet support:

### 🔄 Cardano (ADA)
**Status**: Research Phase  
**Evaluation Focus**: Multi-signature implementation research in progress

#### Technical Considerations
- **UTXO Model**: Similar to Bitcoin but with enhanced scripting capabilities
- **Native Scripts**: Built-in multisignature support through native scripts
- **Plutus Smart Contracts**: Advanced scripting for complex multisig logic
- **Transaction Model**: UTXO-based with deterministic fee calculation

#### Integration Challenges
- **Key Derivation**: Adapting BIP48 to Cardano's HD wallet structure
- **Address Format**: Supporting both Byron and Shelley address formats
- **Metadata Handling**: Transaction metadata for SSP communication protocol
- **Staking Integration**: Considerations for delegation and rewards

#### Potential Features
- **Native Multisig**: Leverage Cardano's built-in multisignature capabilities
- **Smart Contract Integration**: Advanced features through Plutus contracts  
- **Staking Support**: Delegate ADA while maintaining multisig security
- **DeFi Integration**: Connect with Cardano DeFi protocols

---

### 🌌 Cosmos (ATOM)
**Status**: Inter-blockchain communication and multisig evaluation

#### Technical Considerations
- **Cosmos SDK**: Modular blockchain framework with built-in multisig
- **IBC Protocol**: Inter-blockchain communication capabilities
- **Account Model**: Secp256k1 signatures compatible with existing SSP infrastructure
- **Gas Model**: Predictable fee structure similar to Ethereum

#### Integration Challenges
- **Multi-Chain Architecture**: Supporting multiple Cosmos chains (Osmosis, Juno, etc.)
- **IBC Integration**: Cross-chain transaction coordination
- **Staking Mechanics**: Validator delegation and slashing considerations
- **Governance Integration**: Participating in on-chain governance proposals

#### Potential Features
- **IBC Transactions**: Cross-chain transfers within Cosmos ecosystem
- **Multi-Chain Support**: Single interface for multiple Cosmos chains
- **Staking Integration**: Secure staking with multisig protection
- **Governance Participation**: Vote on proposals with multisig security

---

### 🌈 NEAR Protocol (NEAR)
**Status**: Account model and multisig capabilities evaluation

#### Technical Considerations
- **Account Model**: Human-readable account names and subaccounts
- **Access Keys**: Flexible key management system with different permission levels
- **Sharding**: Nightshade sharding for scalability
- **Storage Model**: State rent and storage staking considerations

#### Integration Challenges
- **Account Structure**: Adapting to NEAR's unique account model
- **Key Management**: Integrating with NEAR's access key system
- **Cross-Shard Transactions**: Handling sharded execution
- **Storage Costs**: Managing state rent and storage requirements

#### Potential Features
- **Subaccount Management**: Create and manage subaccounts with multisig
- **Access Key Control**: Fine-grained permission management
- **DeFi Integration**: Connect with NEAR DeFi ecosystem
- **Low-Cost Transactions**: Benefit from NEAR's scalable architecture

## Evaluation Criteria

### Technical Requirements
- **Multisignature Support**: Native or smart contract-based multisig capabilities
- **Key Compatibility**: Compatibility with secp256k1 or ability to adapt existing key management
- **Transaction Model**: Clear transaction structure for SSP communication protocol
- **Network Reliability**: Stable network with consistent block production

### Security Standards
- **Cryptographic Strength**: Strong cryptographic foundations
- **Audit History**: Network security audits and track record
- **Decentralization**: Sufficient validator/node distribution
- **Upgrade Safety**: Safe network upgrade mechanisms

### Ecosystem Maturity
- **Developer Tools**: Mature development ecosystem and tooling
- **DeFi Integration**: Active DeFi protocols for user utility
- **Community Support**: Strong community and institutional adoption
- **Regulatory Clarity**: Clear regulatory environment for integration

### User Experience
- **Transaction Speed**: Reasonable confirmation times
- **Fee Structure**: Predictable and reasonable transaction costs
- **Mobile Compatibility**: Ability to integrate with SSP Key mobile app
- **Error Recovery**: Robust error handling and recovery mechanisms

## Integration Timeline

### Phase 1: Technical Research (Ongoing)
- **Blockchain Analysis**: Deep dive into network architecture and capabilities
- **Multisig Design**: Design multisignature implementation approach
- **Compatibility Assessment**: Evaluate integration complexity
- **Security Review**: Assess security implications and requirements

### Phase 2: Proof of Concept (TBD)
- **Prototype Development**: Build basic integration prototype
- **Security Testing**: Comprehensive security testing and review
- **Performance Evaluation**: Test transaction throughput and user experience
- **Community Feedback**: Gather feedback from users and stakeholders

### Phase 3: Production Integration (TBD)
- **Full Implementation**: Complete integration with all SSP Wallet features
- **Audit and Testing**: Professional security audits and extensive testing
- **User Interface**: Seamless integration with existing SSP Wallet UI
- **Documentation**: Complete user guides and developer documentation

## Community Input

### Feedback Channels
We encourage community feedback on network priorities and integration approaches:

- **GitHub Discussions**: [SSP Wallet Discussions](https://github.com/RunOnFlux/ssp-wallet/discussions)
- **Community Discord**: Join discussions about network integration priorities
- **Feature Requests**: Submit specific network integration requests
- **Technical Input**: Share expertise about specific blockchain architectures

### Voting and Prioritization
- **Community Polls**: Regular polls on network integration priorities
- **Usage Analytics**: Consider user demand and ecosystem growth
- **Technical Feasibility**: Balance community requests with technical complexity
- **Security First**: Prioritize networks that maintain SSP's security standards

## Development Resources

### Research Documentation
- **Technical Analysis**: Detailed analysis of each network's capabilities
- **Integration Proposals**: Specific approaches for multisig implementation
- **Security Assessments**: Security considerations for each network
- **Timeline Estimates**: Projected development and integration timelines

### Contributing
Developers and researchers interested in contributing to network evaluation:

- **Research Contributions**: Help analyze network capabilities and requirements
- **Prototype Development**: Assist with proof-of-concept implementations
- **Testing Support**: Help with security testing and validation
- **Documentation**: Contribute to technical documentation and guides

## Current Status Summary

| Network | Status | Key Focus | Timeline |
|---------|--------|-----------|----------|
| **Cardano** | Research | Multi-signature native scripts | Under evaluation |
| **Cosmos** | Research | IBC and multi-chain support | Under evaluation |
| **NEAR** | Research | Account model adaptation | Under evaluation |

**Note**: Integration timelines depend on technical complexity, security requirements, community demand, and development resources. Networks that demonstrate strong multisignature capabilities and align with SSP's security principles will be prioritized for integration.

The evaluation process ensures that any new network integration maintains the high security standards and user experience that define the SSP Wallet ecosystem.