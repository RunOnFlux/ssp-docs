# Account Abstraction in SSP Wallet

## Overview

On every supported **EVM** chain, SSP funds are held in **ERC-4337 smart-contract accounts** secured by a **Schnorr 2-of-2 multisignature** — this is SSP's native architecture for those chains, **not** an optional feature you turn on. There is no plain EOA (externally owned account) option and no single importable private key for an SSP EVM address; every spend is submitted as an ERC-4337 UserOperation signed by **both** keys (SSP Wallet + SSP Key).

Account Abstraction applies to EVM chains only. Other chain families use their own native 2-of-2 multisig:

- **EVM** (Ethereum, Polygon, BSC, Base, Avalanche, …): ERC-4337 smart accounts, Schnorr 2-of-2.
- **UTXO** (Bitcoin, Litecoin, Dogecoin, …): native P2SH / P2WSH 2-of-2 multisig.
- **Solana**: PDA vault multisig (the SSP Relay paymaster signs the fee-payer slot).

In all cases the security model is the same strict **2-of-2**: both keys are required for every transaction. Account Abstraction changes *how* an EVM address is constructed and how transactions are submitted — it does not add a third key or weaken the multisig.

## How SSP implements Account Abstraction on EVM

Each EVM address is a **CREATE2-deterministic** ERC-4337 smart account, derived from the chain's factory + entrypoint, the two combined Schnorr public keys, and a fixed account salt. Because it is derived from *both* keys, no single seed or private key reproduces the address.

| Network | Chain ID | Factory Address | Entry Point |
|---------|----------|-----------------|-------------|
| **Ethereum** | 1 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |
| **Polygon** | 137 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |
| **BSC** | 56 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |
| **Base** | 8453 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |
| **Avalanche** | 43114 | `0x3974821943e9cA3549744D910999332eE387Fda4` | `0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789` |

### Schnorr multisignature integration

SSP combines Account Abstraction with Schnorr multisignatures — both the SSP Wallet key and the SSP Key contribute a signature that is aggregated for on-chain verification:

```typescript
import * as aaSchnorrMultisig from '@runonflux/aa-schnorr-multisig-sdk';

// Schnorr signers for both keys
const signerOne = aaSchnorrMultisig.helpers.SchnorrHelpers.createSchnorrSigner(walletPrivateKey);
const signerTwo = aaSchnorrMultisig.helpers.SchnorrHelpers.createSchnorrSigner(mobilePrivateKey);

const publicKeys = [signerOne.getPubKey(), signerTwo.getPubKey()];
const publicNonces = [signerOne.getPubNonces(), signerTwo.getPubNonces()];
const combinedPublicKey = aaSchnorrMultisig.signers.Schnorrkel.getCombinedPublicKey(publicKeys);
```

### Transactions are UserOperations

EVM spends are built as ERC-4337 UserOperations rather than plain transactions:

```typescript
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

```typescript
// From SSP's constructTx.ts
const smartAccountClient = createSmartAccountClient({
  transport: viemHttp(node),
  chain: viemChain,
  account: {
    address: contractAddress as `0x${string}`,
    async signMessage() {
      return await generateSchnorrMultisigSignature(); // SSP's Schnorr 2-of-2 signing
    }
  }
});
```

### Message signing with Schnorr

```typescript
const { signature: sigOne, challenge } = signerOne.signMultiSigMsg(message, publicKeys, publicNonces);
const { signature: sigTwo } = signerTwo.signMultiSigMsg(message, publicKeys, publicNonces);
const sSummed = aaSchnorrMultisig.signers.Schnorrkel.sumSigs([sigOne, sigTwo]);

// ABI encode for on-chain verification
const px = ethers.hexlify(combinedPublicKey.buffer.subarray(1, 33));
const parity = combinedPublicKey.buffer[0] - 2 + 27;
const sigData = abiCoder.encode(
  ['bytes32', 'bytes32', 'bytes32', 'uint8'],
  [px, challenge.buffer, sSummed.buffer, parity]
);
```

## What this means for you

- **It's automatic.** You don't enable or configure Account Abstraction — when you use an EVM chain in SSP, your address is already an ERC-4337 smart account. Sending works the same way as any other chain in the UI.
- **No EOA / no single private key.** An SSP EVM address is not a MetaMask-style EOA; you cannot export a single private key that controls it, and importing one of the seeds elsewhere yields a different, empty account. Access is only ever via the 2-of-2 (both keys), restorable from the two seed phrases.
- **First send deploys the account.** Like all ERC-4337 accounts, the smart account is deployed on-chain with its first UserOperation; SSP handles this for you.
- **Recovery is by seed phrase only.** There is no social recovery, guardians, or third key. Back up **both** seed phrases separately. See [Lost or Replaced a Device?](../troubleshooting/lost-or-replaced-device.md).

## Additional resources

- **[Account Abstraction Implementation](../architecture/account-abstraction.md)** — deeper technical reference
- **[Schnorr Multisig Summary](../../../schnorr-multisig-summary.md)** — signature implementation details
- **[ERC-4337 Standard](https://eips.ethereum.org/EIPS/eip-4337)** — Account Abstraction specification
- **[SSP AA SDK](https://github.com/RunOnFlux/aa-schnorr-multisig-sdk)** — Schnorr multisig integration
- **[Account Abstraction Repository](https://github.com/RunOnFlux/account-abstraction)** — SSP's AA implementation

---

**On EVM chains, Account Abstraction is SSP's built-in architecture — ERC-4337 smart accounts secured by a Schnorr 2-of-2 multisignature, with the same both-keys-required security as SSP's UTXO and Solana multisig.**
