---
name: bluepages
description: |
  Look up wallet address <> Twitter/Farcaster identity mappings via Bluepages.fyi.
  Use when asked who owns a wallet, finding addresses for a Twitter/Farcaster handle,
  looking up 0x addresses, or any wallet identity and address attribution queries.
metadata:
  author: bluepages
  version: "1.0.0"
---

# Bluepages API

Crypto address <> Twitter/Farcaster identity lookups. Full docs at [bluepages.fyi/docs](https://bluepages.fyi/docs.html).

## Authentication

Set one of these environment variables before starting Claude Code:

- **`BLUEPAGES_API_KEY`** (recommended) - 20% cheaper, 2x rate limits. Get a key at [bluepages.fyi/api-keys](https://bluepages.fyi/api-keys.html).
- **`PRIVATE_KEY`** - Pay-per-request with USDC on Base. No API key needed.

## Getting API Credits

### Option A: Programmatic Purchase (for agents with PRIVATE_KEY)

If you have `PRIVATE_KEY` configured, use the `purchase_credits` tool:

```
Tool: purchase_credits
Args: { "package": "starter" }
```

Packages:
- **starter**: 5,000 credits for $5 USDC
- **pro**: 50,000 credits for $45 USDC (10% discount)
- **enterprise**: 1,000,000 credits for $600 USDC (40% discount)

After purchase, credits are linked to your wallet address. You can then get an API key for that address from the `/api/auth` endpoint.

### Option B: Manual Setup (for humans)

1. Go to [bluepages.fyi/api-keys.html](https://bluepages.fyi/api-keys.html)
2. Connect wallet and sign message
3. Click "Purchase Credits" and select a package
4. Pay with USDC on Base network

### Configure Environment

```bash
export BLUEPAGES_API_KEY="bp_your_key_here"
```

Or ask Claude Code:
> Save my Bluepages API key bp_xxx to settings.json

## MCP Tools

This plugin provides the following tools via MCP:

| Tool                       | Cost                   | Description                                        |
| -------------------------- | ---------------------- | -------------------------------------------------- |
| `check_address`            | 1 credit ($0.001)      | Check if address has data                          |
| `check_twitter`            | 1 credit ($0.001)      | Check if Twitter handle has data                   |
| `get_data_for_address`     | 50 credits ($0.05)     | Full identity data for address (free if not found) |
| `get_data_for_twitter`     | 50 credits ($0.05)     | Full identity data for handle (free if not found)  |
| `batch_check`              | 40 credits ($0.04)     | Check up to 50 items                               |
| `batch_get_data`           | 40 credits/found item  | Data for up to 50 items (x402: $2.00 flat/batch)   |
| `batch_check_streaming`    | same as batch_check    | For large lists (100+), shows progress             |
| `batch_get_data_streaming` | same as batch_get_data | For large lists (100+), shows progress             |
| `check_credits`            | free                   | Check remaining credits (API key only)             |
| `set_credit_alert`         | free                   | Set low-credit warning threshold (API key only)    |
| `purchase_credits`         | $5-$600 USDC           | Buy credits via x402 (PRIVATE_KEY only)            |

## Input Format

- **Addresses**: 0x-prefixed, 42-character hex (e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`). Case-insensitive.
- **Twitter handles**: With or without `@` prefix (e.g. `vitalikbuterin` or `@vitalikbuterin`).

## Workflow

### Single address lookup

1. Use `check_address` first (cheap) to see if data exists
2. If found, use `get_data_for_address` to get full identity data

### Cost-efficient batch processing

Always use the two-phase approach for batch lookups to minimize costs:

1. **Check existence first** - $0.04 per 50 items (use `batch_check_streaming` for 100+)
2. **Fetch full data only for found addresses** - $2.00 per 50 items (use `batch_get_data_streaming` for 100+)

This reduces costs by 90%+ vs fetching data for all addresses.

## Response Format

### Single lookup (`get_data_for_address`)

Returns identities (twitter, farcaster, etc.), cluster info (related wallets), and data sources.

### Batch lookup (`batch_get_data`)

Returns an object keyed by address/handle with primary identity and alternates.

## Rate Limits

- API Key: 60 req/min
- x402 (pay-per-request): 30 req/min
- Batch: max 50 items per request
