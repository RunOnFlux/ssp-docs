---
icon: file-lines
---

# SSP Wallet API

## SSP Wallet JavaScript API

The SSP Wallet injects a `window.ssp` object into the website, enabling communication between web pages and the SSP Wallet Chrome extension.

### API Method: `window.ssp.request(method, parameters)`

This function takes two parameters:

* `method`: A string specifying the method to be called.
* `parameters`: An object containing additional parameters specific to the method.

#### Implemented Methods

**1. Pay Request**

* **Method:** `'pay'`
* **Description:** Requests SSP to perform payment actions (sending assets).
* **Parameters:**
  * `message` (string): A message to include in the transaction (e.g., `'Hello SSP'`).
  * `amount` (string): The amount to send in whole units (e.g., `'4.124'`).
  * `address` (string): The recipient's address (e.g., `'t1eabPBaLCqNgttQMnAoohPaQM6u2vFwTNJ'`).
  * `chain` (string): The chain ID identifier of SSP (e.g., `'flux'`).
  * `contract?` (string): For networks that support tokens, the token contract to send from, not needed for ETH itself (e.g., `'0xdac17f958d2ee523a2206206994597c13d831ec7'` to send `'USDT'`).
* **Response:**
  * `status` (string): Indicates success or error.
  * `result?` (string): Explanation of error (if any).
  * `data?` (string): Explanation of success (if any).
  * `txid?` (string): Transaction ID in case of a successful payment.

**Example:**

```javascript
window.ssp.request('pay', {
  message: 'Hello SSP',
  amount: '4.124',
  address: 't1eabPBaLCqNgttQMnAoohPaQM6u2vFwTNJ',
  chain: 'flux'
}).then(response => {
  console.log(response);
});
```

**2. Sign Message with SSP Wallet ID (FluxID)**

* **Method:** `'sspwid_sign_message'`
* **Description:** Requests SSP to sign a message using SSP Wallet Identity.
* **Parameters:**
  * `message` (string): The message to be signed by SSP Wallet Identity (e.g., `'Hello SSP, please sign this message'`).
* **Response:**
  * `status` (string): Indicates success or error.
  * `result?` (string): Explanation of error (if any).
  * `data?` (string): Explanation of success (if any).
  * `signature?` (string): Signature of the signed message.
  * `address?` (string): Address that signed the message.
  * `message?` (string): The message that was signed.

**Example:**

```javascript
window.ssp.request('sspwid_sign_message', {
  message: 'Hello SSP, please sign this message'
}).then(response => {
  console.log(response);
});
```

**3. SSP Chains Info**

* **Method:** `'chains_info'`
* **Description:** Requests SSP to get the list of supported chains.
* **Response:**
  * `status` (string): Indicates success or error.
  * `result?` (string): Explanation of error (if any).
  * `data?` (string): Explanation of success (if any).
  * `chains?` (array of objects): List of all supported chains.
    * `id` (string): Chain ID.
    * `name` (string): Chain name.
    * `symbol` (string): Chain symbol.
    * `decimals` (number): Chain decimals.
    * `chainId?` (string): Chain ID (for EVM chains) of the chain.

**Example:**

```javascript
window.ssp.request('chains_info').then(response => {
  console.log(response);
});
```

**4. Chain Tokens**

* **Method:** `'chain_tokens'`
* **Description:** Requests SSP to get the list of tokens for a given chain.
* **Parameters:**
  * `chain` (string): Chain ID as SSP identifier.
* **Response:**
  * `status` (string): Indicates success or error.
  * `result?` (string): Explanation of error (if any).
  * `data?` (string): Explanation of success (if any).
  * `tokens?` (array of objects): List of tokens for the given chain.
    * `contract` (string): Token contract.
    * `name` (string): Token name.
    * `symbol` (string): Token symbol.
    * `decimals` (number): Token decimals.

**Example:**

```javascript
window.ssp.request('chain_tokens', {
  chain: 'eth'
}).then(response => {
  console.log(response);
});
```

**5. User Synced Chains Info**

* **Method:** `'user_chains_info'`
* **Description:** Requests SSP to get the list of chains that the user has synchronised. Those are the chains that can be immediately used and have addresses. Other chains will require the user to synchronise the chain first.
* **Response:**
  * `status` (string): Indicates success or error.
  * `result?` (string): Explanation of error (if any).
  * `data?` (string): Explanation of success (if any).
  * `chains?` (array of objects): List of user synced chains.
    * `id` (string): Chain ID.
    * `name` (string): Chain name.
    * `symbol` (string): Chain symbol.
    * `decimals` (number): Chain decimals.
    * `chainId?` (string): Chain ID (for EVM chains) of the chain.

**Example:**

```javascript
window.ssp.request('user_chains_info').then(response => {
  console.log(response);
});
```

**6. User Addresses**

* **Method:** `'user_addresses'`
* **Description:** Requests SSP to get the list of addresses for a given chain.
* **Parameters:**
  * `chain` (string): Chain ID as SSP identifier.
* **Response:**
  * `status` (string): Indicates success or error.
  * `result?` (string): Explanation of error (if any).
  * `data?` (string): Explanation of success (if any).
  * `addresses?` (array of strings): List of user approved addresses for the given chain.

**Example:**

```javascript
window.ssp.request('user_addresses', {
  chain: 'btc'
}).then(response => {
  console.log(response);
});
```

**7. User Addresses All Chains**

* **Method:** `'user_chains_addresses_all'`
* **Description:** Requests SSP to get the list of addresses for a all chains. User can select what addresses are shared for what chain. Also provides information about particular chain
* **Response:**
  * `status` (string): Indicates success or error.
  * `result?` (string): Explanation of error (if any).
  * `data?` (string): Explanation of success (if any).
  * `chains?` (array of objects): List of user approved chains with addresses
    * `id` (string): Chain ID.
    * `name` (string): Chain name.
    * `symbol` (string): Chain symbol.
    * `decimals` (number): Chain decimals.
    * `chainId?` (string): Chain ID (for EVM chains) of the chain.
    * `addresses` (array of strings): List of user addresses

**Example:**

```javascript
window.ssp.request('user_chains_addresses_all').then(response => {
  console.log(response);
});
```

**8. Flux Node Start**

* **Method:** `'flux_node_start'`
* **Description:** Requests SSP to sign a Flux node start transaction with the collateral key of an address held in the user's wallet. The user reviews the node details (collateral, UTXO, delegates) in a confirmation dialog and approves or rejects. On approval, SSP signs the start transaction locally and returns the signed transaction hex — **SSP does not broadcast it**; the requesting service broadcasts it to the Flux network.
*   **Key facts:**

    * The collateral address must belong to the user's SSP Wallet on the requested chain. SSP locates the address among its own synced wallets and uses its own derivation records and redeem script — these are never accepted from the requesting page.
    * SSP addresses are 2-of-2 multisig (P2SH). The node start transaction is signed by the wallet-held collateral private key; the Flux network validates the signature against the embedded redeem script, so a single signature suffices for node starts.
    * The node identity keypair is expected to live with whoever operates the node (e.g., a hosting service). Only the identity **public** key is passed — the identity private key is never transmitted and does not sign the start transaction.
    * Delegate public keys, when provided, are granted node management permissions (DELEGATE\_TYPE\_UPDATE). Delegates can start and maintain the node but can never spend or move the collateral. The user sees every delegate key in the confirmation dialog.
    * The signing timestamp is generated by SSP at approval time, not supplied by the caller.
* **Parameters:**
  * `chain` (string): `'flux'` or `'fluxTestnet'`.
  * `collateralAddress` (string): The P2SH address holding the node collateral. Must be an address of the user's SSP Wallet.
  * `collateralTxid` (string): Transaction ID of the collateral UTXO (64 hex characters).
  * `collateralVout` (number): Output index of the collateral UTXO (non-negative integer).
  * `identityPubKey` (string): Compressed node identity public key (66 hex characters, `02`/`03` prefix). Held by the node operator; only the public key is shared.
  * `delegates?` (array of strings): Optional compressed public keys (max 25) to grant node management permissions to.
  * `nodeName?` (string): Optional display name shown in the confirmation dialog.
  * `collateralAmount?` (string): Optional collateral amount in satoshis, shown in the confirmation dialog (e.g., `'4000000000000'` for 40000 FLUX).
  * `siteName?` (string): Optional requesting site display name shown in the dialog.
  * `iconUrl?` (string): Optional HTTPS URL of the requesting site icon shown in the dialog.
* **Response:**
  * `status` (string): Indicates success or error.
  * `result?` (object on success): `{ signedTxHex: string }` — the fully signed node start transaction hex, ready to broadcast to the Flux network. On error, `result` is a string explaining the rejection.
  * `data?` (string): Explanation of success (if any).

**Example:**

```javascript
window.ssp.request('flux_node_start', {
  chain: 'flux',
  collateralAddress: 't3aFe3N7ubc6Bs2ki9vTQi2E1LG6nHTqBLC',
  collateralTxid: '2f8e3c...64 hex chars...9a1b',
  collateralVout: 0,
  collateralAmount: '4000000000000',
  identityPubKey: '02c1e9...66 hex chars...',
  delegates: ['03ab12...66 hex chars...'],
  nodeName: 'my-hosted-node-1',
  siteName: 'Example Node Hosting'
}).then(response => {
  if (response.status === 'SUCCESS') {
    // broadcast response.result.signedTxHex to the Flux network
  }
});
```

_Enterprise note: for collateral held in SSP Enterprise organisation vaults, the equivalent `'enterprise_flux_node_start'` method is used by the SSP Enterprise platform. It additionally scopes signing to the organisation derivation path and supports signing on either the SSP Wallet or SSP Key device._
