# Bluepages Agent Plugins

Crypto address <> Twitter/Farcaster identity lookups for AI agents, powered by [bluepages.fyi](https://bluepages.fyi).

## What is Bluepages?

Bluepages maps Ethereum addresses to Twitter and Farcaster identities (and vice versa), covering 800,000+ verified connections. See [bluepages.fyi](https://bluepages.fyi) for more.

## Installation

### Claude Code

```bash
# First install the marketplace
/plugin marketplace add bluepageseth/agent-plugins

# Then install the plugin
/plugin install bluepages
```

## Authentication

You need **one** of two auth methods. The easiest way to set it up is to ask Claude Code directly:

> Save my Bluepages API key bp_xxx to settings.json

or

> Save my private key 0xabc to settings.json

Restart Claude Code after saving. You can also set the env var manually:

### Option 1: x402 Payments

Pay-per-request with USDC on Base. No API key needed.

```bash
export PRIVATE_KEY="your-ethereum-private-key"
```

Requires a funded wallet on Base with USDC.

### Option 2: API Key

20% cheaper, 2x rate limits. Recommended for power users. Get a key at [bluepages.fyi/api-keys](https://bluepages.fyi/api-keys.html).

```bash
export BLUEPAGES_API_KEY="your-key-here"
```

## What's Included

| Component          | Description                                                   |
| ------------------ | ------------------------------------------------------------- |
| MCP Server         | 10 tools — single/batch lookups, streaming, credit management |
| Skill `/bluepages` | Guided workflow, auto-selects the right tool for your query   |

## Links

- [Homepage](https://bluepages.fyi)
- [API Documentation](https://bluepages.fyi/docs.html)
- [Get API Keys](https://bluepages.fyi/api-keys.html)

## License

MIT
