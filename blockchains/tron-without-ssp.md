---
icon: life-ring
---

# Moving TRON Vault Funds Without SSP

{% hint style="info" %}
**Your TRON vault never depends on SSP.** The vault contract accepts a correctly signed operation from **any** TRON account. If SSP Relay, SSP's fee sponsor or the SSP apps are unavailable, you can still move your funds with signatures from enough of the vault's keys and any TRON account holding a little TRX to pay for energy.
{% endhint %}

## When to use this

* SSP Relay or SSP's sponsor is unavailable and you need to move funds.
* You want to check for yourself that SSP can't hold your funds hostage.

For everyday sends, use the app. If sponsorship is ever unavailable, choose **Pay network fee myself** on the send screen: SSP Key then submits the same kind of signed operation from its own TRON account, with no fee to SSP.

## How it works

Every TRON vault is an immutable contract at a CREATE2 address computed from its signers and threshold. It has two entry points that anyone can call:

1. **`deploy(configHash)`** on the vault factory. This creates the vault contract at its predetermined address. It is only needed if the vault has never sent before, because SSP deploys a vault inside its first send. It is permissionless and idempotent: it can't deploy a different signer set at your address, and calling it again just returns the existing vault.
2. **`execute(signers, threshold, op, signatures)`** on the vault itself. The vault checks that the signers and threshold hash to its own configuration, that the deadline hasn't passed, that the operation's number is unused, and that the signatures are valid. Then it makes the transfers.

The account that calls these pays the TRON energy. Nothing else is required: SSP's sponsor is only a convenience that pays the energy for you.

### What you need

* **A signed operation**: the transfers you want, signed by at least the threshold number of the vault's keys (both keys for an SSP Wallet vault), in the signed-operation file format below.
* **A TRON account with TRX** to submit it. It doesn't have to be one of the vault's keys; a separate account holding just enough TRX is best. Budget around **10 TRX** for a USDT transfer, plus around **6 TRX** if the vault has never sent before (the vault is deployed first).
* **The contract addresses** for the network:

| Contract | Mainnet | Nile |
|----------|---------|------|
| Vault factory | Published at launch | Published at launch |
| Vault implementation | Published at launch | Published at launch |

The addresses are also pinned in the open-source SDK, [`@runonflux/tron-multisig`](https://github.com/RunOnFlux/tron-multisig), which the tools below use.

## The signed-operation file

A signed operation is shared as a JSON file in the SDK's `ssp-tron-signed-op` format, version 1:

```json
{
  "format": "ssp-tron-signed-op",
  "version": 1,
  "network": "mainnet",
  "chainId": "728126428",
  "vault": "T…",
  "signers": ["T…", "T…"],
  "threshold": 2,
  "op": {
    "calls": [
      {
        "to": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t",
        "value": "0",
        "data": "0xa9059cbb…",
        "tokenId": "0",
        "tokenValue": "0"
      }
    ],
    "nonce": "1790000000",
    "deadline": "1790003600",
    "fee": {
      "token": "T9yD14Nj9j7xAB4dbGeiX9h8unkKHxuWwb",
      "amount": "0",
      "recipient": "T9yD14Nj9j7xAB4dbGeiX9h8unkKHxuWwb"
    }
  },
  "signatures": ["0x…", "0x…"]
}
```

| Field | Meaning |
|-------|---------|
| `format`, `version` | Always `"ssp-tron-signed-op"` and `1` |
| `network`, `chainId` | `"mainnet"` with `"728126428"`, or `"nile"` with `"3448148188"` (the TIP-712 chain ID the signatures are bound to) |
| `vault` | The vault address. The tools re-derive it from `signers` and `threshold` and refuse the file if it doesn't match. |
| `signers` | Every signer address of the vault, sorted in ascending order |
| `threshold` | How many signatures the vault requires |
| `op.calls` | 1 to 16 transfers. TRX: `to` is the recipient and `value` is the amount in sun (1 TRX = 1,000,000 sun). TRC-20: `to` is the token contract and `data` is its `transfer(recipient, amount)` calldata. TRC-10: `tokenId` and `tokenValue`. |
| `op.nonce` | A one-time number. Any number the vault hasn't used works; they need not be in order. |
| `op.deadline` | Unix time in seconds after which the operation can never execute |
| `op.fee` | The fee the vault pays in the same transaction. When you submit yourself, use amount `"0"`; the token and recipient are then the zero address shown above. |
| `signatures` | At least `threshold` 65-byte signatures (`0x` hex) over the operation's TIP-712 digest, in any order |

Things to know:

* **The fee is part of what was signed.** An operation signed for a sponsored send includes SSP's fee, and the vault still pays it if you submit that operation yourself. Operations you sign for this purpose should use a zero fee.
* **Deadlines are short in SSP Wallet.** Operations signed in SSP Wallet are valid for at most 2 hours. Fully signed SSP Enterprise proposals are valid until the proposal expires.
* **Each number executes once.** If the vault has already used the operation's nonce (the operation ran, or was invalidated on-chain), it is refused.

### Signing an operation yourself

If the SSP apps aren't available, you can create a signed operation with the SDK and the seed phrases of the signing keys.

{% hint style="danger" %}
**This step handles your seed phrases.** Do it only on a computer you trust, disconnected from the network while the seed phrases are in use. Never type a seed phrase into a website. SSP will never ask you for your seed phrases.
{% endhint %}

Where the keys are:

* **SSP Wallet vault (2-of-2)**: each device's key is at `m/48'/195'/0'/0'/0/<address index>` of **its own** seed phrase: the SSP Wallet seed phrase for one key, the SSP Key seed phrase for the other. Your first TRON address is address index `0`.
* **SSP Enterprise vault**: each signer's keys are at `m/48'/195'/<org index>'/0'/<vault index>/<address index>`. You need as many keys as the vault's on-chain threshold. In Dual Signing mode that threshold is twice the vault's signer threshold (2M), and any of the members' SSP Wallet or SSP Key keys count toward it, for example both seed phrases of M members. Build the configuration from all signers' addresses with `buildConfig(signerAddresses, threshold)` instead of `buildConsumerConfig`.

Install the SDK and two standard BIP-39/BIP-32 libraries (Node.js 20.19 or later) while online, then disconnect:

```bash
yarn add @runonflux/tron-multisig @scure/bip39 @scure/bip32
```

Save this as `sign-op.mjs` and fill in the recipient and amount:

```javascript
import { mnemonicToSeedSync } from '@scure/bip39';
import { HDKey } from '@scure/bip32';
import {
  NETWORKS,
  buildConsumerConfig,
  deriveVault,
  buildOp,
  trc20TransferCall,
  noFee,
  opDigest,
  localSigner,
  stringifySignedOp,
} from '@runonflux/tron-multisig';

const PATH = "m/48'/195'/0'/0'/0/0"; // SSP Wallet vault, first TRON address
const leaf = (words) => HDKey.fromMasterSeed(mnemonicToSeedSync(words)).derive(PATH);
const walletKey = leaf(process.env.WALLET_WORDS); // SSP Wallet seed phrase
const keyKey = leaf(process.env.KEY_WORDS); // SSP Key seed phrase

const config = buildConsumerConfig(walletKey.publicKey, keyKey.publicKey);
const vault = deriveVault('mainnet', config);
console.error('vault', vault.address); // must be your TRON address in SSP Wallet

const now = BigInt(Math.floor(Date.now() / 1000));
const op = buildOp({
  // 25 USDT (6 decimals) to the destination. Use trxTransferCall(to, sun) for TRX.
  calls: [trc20TransferCall(NETWORKS.mainnet.usdt, 'T_DESTINATION_ADDRESS', 25_000_000n)],
  nonce: now, // any number the vault hasn't used
  deadline: now + 3600n, // valid for one hour
  fee: noFee(), // you pay the energy yourself; nothing goes to SSP
});

const digest = opDigest(NETWORKS.mainnet.chainId, vault.address, op);
const signatures = [walletKey, keyKey].map((k) => localSigner(k.privateKey).signDigest(digest));

console.log(
  stringifySignedOp({
    network: 'mainnet',
    chainId: NETWORKS.mainnet.chainId,
    vault: vault.address,
    config: vault.config,
    op,
    signatures,
  })
);
```

Run it without leaving the seed phrases in your shell history:

```bash
read -rs WALLET_WORDS && export WALLET_WORDS
read -rs KEY_WORDS && export KEY_WORDS
node sign-op.mjs > op.json
unset WALLET_WORDS KEY_WORDS
```

Check that the printed vault address is your SSP TRON address before going further. If it isn't, the path, the address index or a seed phrase is wrong, and the vault would refuse the signatures anyway.

## Option 1: Submit with the command-line tool

The SDK includes a command-line tool, `tron-multisig`, that runs with Node.js 20.19 or later. It needs no SSP service: it talks directly to a TRON full node.

**1. Check the vault address (optional).** `predict` computes a vault's address from its signers and threshold:

```bash
npx @runonflux/tron-multisig predict --network mainnet \
  --signers T_WALLET_SIGNER,T_KEY_SIGNER --threshold 2
```

**2. Put the submitting account's private key in an environment variable.** It must be the 64-character hex private key of the account that pays the energy. The tool reads it from the variable you name and never prints it.

```bash
read -rs SUBMITTER_KEY && export SUBMITTER_KEY
```

**3. Do a dry run.** This checks everything and simulates the operation, but broadcasts nothing:

```bash
npx @runonflux/tron-multisig submit --network mainnet --rpc https://api.trongrid.io \
  --signed-op op.json --key-env SUBMITTER_KEY --dry-run
```

**4. Submit.** Run the same command without `--dry-run`:

```bash
npx @runonflux/tron-multisig submit --network mainnet --rpc https://api.trongrid.io \
  --signed-op op.json --key-env SUBMITTER_KEY
unset SUBMITTER_KEY
```

`submit` then:

1. verifies the file offline: it re-derives the vault from the signers and threshold, checks the network and chain ID, and checks that enough distinct signers signed;
2. checks that the submitting account exists on-chain;
3. deploys the vault through the factory if it has no code yet, and waits for that transaction;
4. refuses an operation whose nonce is already used;
5. simulates `execute` and refuses to broadcast if the simulation fails;
6. signs with the submitting key, broadcasts, and waits for the receipt.

Each step prints one line of JSON (`verified`, `deploy-needed`, `deployed`, `simulated`, `execute-broadcast`, `executed`), including transaction IDs you can open on [TronScan](https://tronscan.org).

| Option | Meaning |
|--------|---------|
| `--network` | `mainnet` or `nile` |
| `--rpc` | A TRON full-node URL, for example `https://api.trongrid.io` (mainnet) or `https://nile.trongrid.io` (Nile) |
| `--signed-op` | The signed-operation file |
| `--key-env` | Name of the environment variable holding the submitter's hex private key |
| `--fee-limit` | The most TRX, in sun, each transaction may burn for energy. Default `300000000` (300 TRX); you pay only what is used. |
| `--dry-run` | Verify and simulate only. If the vault isn't deployed yet, the dry run stops after checking the deployment, because the operation can't be simulated until the vault exists. |
| `--factory`, `--implementation`, `--chain-id` | Only for values the SDK's pinned table doesn't have yet, or for a custom network name. They can't override a pinned value. |

The tool exits with `0` on success, `1` if a check or the transaction fails, and `2` for a usage error. You can also use `yarn dlx @runonflux/tron-multisig` in place of `npx @runonflux/tron-multisig`.

## Option 2: Submit through TronScan

This path lets you submit from a browser wallet such as TronLink, including a hardware wallet connected to it, instead of placing a private key in an environment variable.

{% hint style="warning" %}
**TRON charges energy even when a transaction fails.** TronScan doesn't simulate for you, so check first: the deadline is still in the future, the operation's nonce is unused (`isNonceUsed` under **Read Contract** on the vault returns `false`), and the vault holds everything being sent plus any fee.
{% endhint %}

### Prepare the parameters

TronScan needs the arguments exactly as the contract takes them: the signer addresses packed into bytes, the operation as a tuple, and exactly `threshold` signatures packed in the order of the signer each one belongs to. This script computes them from the signed-operation file. It needs no private key, and it throws an error if the file or the signatures are invalid.

```javascript
// params.mjs
import { readFileSync } from 'node:fs';
import { parseSignedOp, verifySignedOp, packSigners, toHex20, to0x } from '@runonflux/tron-multisig';

const signed = parseSignedOp(readFileSync('op.json', 'utf8'));
const v = verifySignedOp(signed, signed.network); // re-derives the vault, checks and orders the signatures
const { op } = signed;

console.log('vault      ', v.vault.address);
console.log('configHash ', to0x(v.vault.configHash));
console.log('signers    ', to0x(packSigners(v.vault.config.signers)));
console.log('threshold  ', v.vault.config.threshold);
console.log(
  'op         ',
  JSON.stringify([
    op.calls.map((c) => [toHex20(c.to), String(c.value), to0x(c.data), String(c.tokenId), String(c.tokenValue)]),
    String(op.nonce),
    String(op.deadline),
    [toHex20(op.fee.token), String(op.fee.amount), toHex20(op.fee.recipient)],
  ])
);
console.log('signatures ', to0x(v.signaturesPacked));
```

The `op` line is the tuple in field order, `[[[to, value, data, tokenId, tokenValue], …], nonce, deadline, [feeToken, feeAmount, feeRecipient]]`, with addresses in `0x` hex form.

### Step 1 — Deploy the vault (only if it has never sent)

If TronScan shows your vault address as an ordinary account rather than a contract, deploy it first:

1. Open the vault factory contract on [tronscan.org](https://tronscan.org) (address above) and go to **Contract → Write Contract**.
2. Connect your wallet.
3. Call **`deploy`** with the `configHash` value from the script, and confirm in your wallet.

When the transaction confirms, your vault address shows as a contract, with your balances unchanged.

### Step 2 — Execute the operation

1. Open your vault address on TronScan and go to **Contract → Write Contract** (or **Write as Proxy**).
2. Connect your wallet.
3. Call **`execute`** with the values from the script:
   * `signers` (bytes): the `signers` value
   * `threshold` (uint8): the `threshold` value
   * `op` (tuple): the `op` value
   * `signatures` (bytes): the `signatures` value
4. Set a fee limit in your wallet that covers the energy (for example 100 TRX; you pay only what is used) and confirm.

If TronScan doesn't offer **Write Contract** or **Write as Proxy** for your vault address, use [Option 1](#option-1-submit-with-the-command-line-tool) instead.

## After submitting

* The transaction appears on TronScan, and the vault records an `Executed` event.
* SSP Wallet and SSP Enterprise show the transfer in the vault's history the next time they sync.
* The operation's nonce is now used, so the same signed operation can never run again.

## Related

* [Supported Blockchains: TRON](supported-blockchains.md#tron-network)
* [Account Abstraction: TRON Contract Vaults](../architecture/account-abstraction.md#tron-contract-vaults)
* [Security Overview (TRON Vault Security)](../security/security-overview.md)
* [Lost or Replaced a Device?](../troubleshooting/lost-or-replaced-device.md)
