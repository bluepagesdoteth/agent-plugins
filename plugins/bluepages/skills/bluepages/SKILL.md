---
name: bluepages
description: >
  Look up wallet <> social identity and label mappings via Bluepages.fyi.
  Use when asked who owns a crypto address, finding wallets for a social handle,
  identifying CEX/exchange wallets, or searching tweets mentioning an address.
compatibility: >
  Requires MCP server (npx github:bluepagesdoteth/bluepages-mcp) and one of:
  BLUEPAGES_API_KEY or PRIVATE_KEY (Ethereum, for x402 payments).
metadata:
  author: bluepages
  version: "1.2.0"
  openclaw:
    emoji: 📘
    install:
      - kind: node
        package: "github:bluepagesdoteth/bluepages-mcp"
    homepage: https://bluepages.fyi/docs.html
    requires:
      env:
        - BLUEPAGES_API_KEY
        - PRIVATE_KEY
---

# Bluepages

Address <> identity + label lookups across ETH, BTC, LTC, BCH, SOL, TRON, DASH, DOGE, XMR, ZEC, ADA, XLM, ALGO, BNB, LSK, SC, TON, Celestia, XRP, APT/SUI, DOT, ATOM, ZIL, EGLD, INJ, NEAR, and EOS.

## Lookup strategy

The optimal workflow depends on the payment method:

### API key users (per-result pricing)

Data endpoints only charge when data is found — call them directly, no check step needed.

- **Single**: `get_data_for_address` or `get_data_for_identity`.
- **Batch**: `batch_get_data` (up to 50 items).
- **Lists > 50**: `batch_get_data_streaming` for automatic batching with progress.

### x402 users (flat pricing per request)

Data requests charge a flat fee regardless of results — check first to filter.

- **Single**: `check_address`/`check_identity` ($0.001) → `get_data_for_*` ($0.05) only if found.
- **Batch**: `batch_check` ($0.04) → `batch_get_data` ($2.00) on found items only.
- **Lists > 50**: `batch_check_streaming` → `batch_get_data_streaming` on found items.

**No data?** Try `search_tweets` — finds tweet mentions even when no identity record exists.

## Tools

| Tool                    | Cost                   | Returns                                 |
| ----------------------- | ---------------------- | --------------------------------------- |
| `check_address`         | 1 credit               | Whether address has data                |
| `check_identity`        | 1 credit               | Whether identity has data               |
| `get_data_for_address`  | 50 (free if not found) | Identities, labels, sanctions, clusters |
| `get_data_for_identity` | 50 (free if not found) | Addresses, labels, sanctions, clusters  |
| `search_tweets`         | 50 (always charged)    | Tweets mentioning the address           |
| `batch_check`           | 40/batch               | Which items have data                   |
| `batch_get_data`        | 40/found item          | Full data, up to 50 items               |

`batch_check_streaming` and `batch_get_data_streaming` exist for lists > 50.

Account tools (discoverable via MCP): `check_credits`, `set_credit_alert`, `get_api_key`, `purchase_credits`.

## Supported inputs

**Addresses**: ETH, BTC (bech32 + base58), LTC, BCH, SOL, TRON, DASH, DOGE, XMR (+ payment IDs), ZEC, ADA, XLM, ALGO, BNB, LSK, SC, TON, Celestia, XRP, APT/SUI, DOT, ATOM, ZIL, EGLD, INJ, NEAR (named accounts), EOS — validated by the server; lookups are case-insensitive, pass addresses as given.

**Identities**: twitter, farcaster, github, discord, telegram, email, linkedin, reddit, instagram, facebook, atproto, circles, ens, phone, name.

## Authentication

- **`BLUEPAGES_API_KEY`** — Bulk discounts up to 40%, 2x rate limits. Get at [bluepages.fyi/api-keys](https://bluepages.fyi/api-keys.html).
- **`PRIVATE_KEY`** — x402 pay-per-request (USDC on Base). Can also purchase an API key via `get_api_key` + `purchase_credits`.

## HTTP fallback

If MCP tools are unavailable, call `https://bluepages.fyi` directly with `X-API-KEY` header. Full docs: [bluepages.fyi/docs](https://bluepages.fyi/docs.html)
