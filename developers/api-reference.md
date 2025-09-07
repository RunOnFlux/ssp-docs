# SSP Relay API Reference

This document describes the actual API endpoints provided by SSP Relay for communication between SSP Wallet and SSP Key.

## Base URL

```
Production: https://relay.ssp.runonflux.io/v1
```

## Authentication

SSP Relay endpoints use simple parameter validation and do not require complex authentication headers. Parameters are validated for format and length constraints.

## Available Endpoints

### Synchronization Endpoints

#### GET /v1/sync/:id
Retrieve synchronization data for a wallet identity.

**Parameters:**
- `id` (string): Wallet identity (alphanumeric, underscore, colon, hyphen allowed, max 200 chars)

**Response:**
```json
{
  "success": true,
  "data": {
    "chain": "bitcoin",
    "walletIdentity": "wallet_id",
    "keyXpub": "xpub6...",
    "wkIdentity": "key_identity",
    "generatedAddress": "bc1q...",
    "publicNonces": ["..."]
  }
}
```

#### POST /v1/sync
Create new synchronization data.

**Request Body:**
```json
{
  "chain": "bitcoin",
  "walletIdentity": "wallet_id", 
  "keyXpub": "xpub6...",
  "wkIdentity": "key_identity",
  "generatedAddress": "bc1q..."
}
```

#### POST /v1/token
Add token synchronization data.

### Transaction Endpoints

#### GET /v1/action/:id
Retrieve transaction action data.

**Parameters:**
- `id` (string): Action identifier

#### POST /v1/action
Submit transaction action for processing.

### Network Information

#### GET /v1/rates
Get current exchange rates for supported cryptocurrencies.

**Response:**
```json
{
  "bitcoin": {
    "usd": 43000.00,
    "eur": 36000.00
  },
  "ethereum": {
    "usd": 3200.00,
    "eur": 2700.00
  }
}
```

#### GET /v1/networkfees
Get current network fee estimates for all supported blockchains.

**Response:**
```json
{
  "bitcoin": {
    "slow": 1,
    "standard": 5,
    "fast": 10
  },
  "ethereum": {
    "slow": 20000000000,
    "standard": 25000000000,
    "fast": 30000000000
  }
}
```

### Token Information

#### GET /v1/tokeninfo/:network/:address
Get information about tokens on supported networks.

**Parameters:**
- `network` (string): Blockchain network (e.g., "ethereum", "polygon")
- `address` (string): Token contract address

### Services

#### GET /v1/services
Get configuration for enabled third-party services.

**Response:**
```json
{
  "onramper": true,
  "exchangeRates": true
}
```

### Support Endpoints

#### POST /v1/ticket
Submit support ticket via Freshdesk integration.

#### POST /v1/contact
Submit contact form.

### Third-Party Integration

#### POST /v1/sign/onramper
Sign data for Onramper integration.

## Error Handling

All endpoints return appropriate HTTP status codes:

- `200` - Success
- `400` - Bad Request (invalid parameters)
- `404` - Not Found
- `500` - Internal Server Error

**Error Response Format:**
```json
{
  "error": "Invalid ID"
}
```

## Rate Limits

Rate limiting is implemented but specific limits are not documented in the public API.

## Development

### Local Development
SSP Relay runs on port 9876 by default in development mode.

```
http://localhost:9876/v1
```

### Configuration
Relay configuration is managed through environment variables and config files. See the SSP Relay repository for development setup instructions.

## Implementation Notes

- All string parameters have length and format validation
- Wallet identities must be alphanumeric with limited special characters
- The relay acts as a communication facilitator and does not store private keys
- All transaction data is temporary and cleared after processing

## Source Code

The complete SSP Relay implementation is available at:
[https://github.com/RunOnFlux/ssp-relay](https://github.com/RunOnFlux/ssp-relay)

For detailed development guides, see the SSP Relay Development section in this documentation.