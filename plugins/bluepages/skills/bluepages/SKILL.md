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
