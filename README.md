# Bluepages Agent Plugins

Multi-chain wallet address <> social identity and label lookups for AI agents, powered by [bluepages.fyi](https://bluepages.fyi). Covers ETH, BTC, LTC, BCH, SOL, TRON, DASH, DOGE, XMR, ZEC, ADA, XLM, ALGO, BNB, LSK, SC, TON, Celestia, and XRP addresses (case-insensitive lookups) mapped to Twitter, Farcaster, GitHub, Discord, Telegram, and 10+ more identity types, plus labels (CEX wallets, etc.).

## Installation

### Claude Code

```bash
# First install the marketplace
/plugin marketplace add bluepagesdoteth/agent-plugins

# Then install the plugin
/plugin install bluepages
```

## Authentication

You need **one** of two auth methods. The easiest way to set it up is to ask Claude Code directly:

> Save my Bluepages API key bp_xxx to settings.json

or

> Save my private key 0xabc to settings.json

Restart Claude Code after saving. You can also set the env var manually:

### Option 1: API Key (recommended)

20% cheaper, 2x rate limits. Get a key at [bluepages.fyi/api-keys](https://bluepages.fyi/api-keys.html).

```bash
export BLUEPAGES_API_KEY="your-key-here"
```

### Option 2: x402 Payments

Pay-per-request with USDC on Base. No API key needed. Requires a funded wallet on Base.

```bash
export PRIVATE_KEY="your-ethereum-private-key"
```

## What's Included

| Component          | Description                                                                        |
| ------------------ | ---------------------------------------------------------------------------------- |
| MCP Server         | 13 tools — single/batch lookups, tweet search, streaming, credit management        |
| Skill `/bluepages` | Guided workflow, auto-selects the right tool for your query                        |

## Links

- [Homepage](https://bluepages.fyi)
- [API Documentation](https://bluepages.fyi/docs.html)
- [Get API Keys](https://bluepages.fyi/api-keys.html)

## License

MIT
