# ElizaOS (ai16z): Whitepaper Claims vs Code Reality
**Score: 49/100, HIGH (Flagged)**
Date: 2026-08-06

Token (live, independently found and confirmed onchain):
- Legacy token symbol `ai16z`, canonical SPL mint `HeLp6NuQkmYB4pYWo2zYs22mESHXPQYzXbB8n4V98jwC`, standard `spl-token-2022` program, decimals 9.
- Legacy freeze authority is null (renounced). Legacy mint authority is NOT null: `AZtt8LUScEAG74iKnPNRuYgQhwmGJhAf6yUkAXjAd8sp`, which is a 256 byte data account owned by the upgradeable BPF program `4FqThZWv3QKWkSyXCDmATpWkpEiCHq5yhkdGWpSEDAZM` (a launchpad or vesting style PDA, not a plain wallet). The same address is also the mutable token metadata update authority.
- Legacy supply is 1,099,896,290.79 (about 1.0999 billion). The only Token-2022 extensions present are `metadataPointer` and `tokenMetadata`; there is no transfer fee extension.
- Legacy live market (DexScreener plus CoinGecko): price about $0.0003055, market cap about $336K, FDV about $336K, market cap rank #3636, 24h volume about $20K to $23K. Deepest pool is a Raydium CPMM `ai16z/SOL` pool holding about $36.1K liquidity (about 59.09M ai16z and 244.89 wSOL). The legacy token is effectively a dead market.
- The project rebranded to ElizaOS and migrated to a NEW token `ELIZAOS`, mint `DuMbhu7mvQvqQHGcnikDgb4XegXJRyhUBfdU22uELiZA`, classic `spl-token` program, decimals 9. Freeze authority is null; mint authority is NOT null: `D4MYCaoyT5XZFBke16JwaNJa6TWDeCTuZMYErukMGerU`, which resolves on chain to a 1 of 2 SPL multisig (numRequiredSigners 1, signers `6CL7GkpTUrFNVpFrex3aoHXk69DHU5mSB5YZLnnGAMJ7` and `EaFFhbBegozJz8xMkfSduq71oFkqZLBgdR8f7YhEDXAt`). One signer alone can mint.
- New token supply is 9,548,836,609.42 (about 9.55 billion), circulating about 7.48 billion, max supply 11 billion per CoinGecko. New token market: price about $0.0002862, market cap about $2.14M, FDV about $2.73M, rank #2071, 24h volume about $2.3M.
- Public reporting (The Block, Decrypt, BeInCrypto, Gate, Tangem, The Coinomist) confirms Eliza Labs settled a Burwick Law federal class action (filed April 2026, U.S. District Court, Southern District of New York), declared the native token obsolete, and is winding down the foundation using remaining treasury funds.

Websites:
- https://eliza.app
- https://docs.elizaos.ai/
- https://github.com/elizaOS/eliza
- https://app.elizacloud.ai

### Verified in code and on chain (MEFAI deep source review)
- The framework is genuinely real and open source. I fetched the `elizaOS/eliza` GitHub root, the raw `README.md`, and `docs.elizaos.ai`. License is MIT. The repo is large and active, structured around `packages/` (runtime, hosts, UI, CLI) and `plugins/` (model, connector, domain, device plugins).
- The core is credible as described by the repo itself: `@elizaos/core` defines `AgentRuntime` with the message loop, memory primitives, and plugin contracts (actions, providers, evaluators, services, model handlers, routes, events). Companion packages include `@elizaos/agent`, `@elizaos/app-core`, `@elizaos/ui`, and the `elizaos` CLI. The docs advertise 90+ integrations (Discord, X, Telegram, Ethereum, Solana, OpenAI) and on device inference via Eliza-1 (Gemma based) through `plugin-local-inference`.
- Neither the `README.md` nor the core docs page reference the token as a protocol input. The framework runs without holding `ai16z` or `ELIZAOS`. The docs expose only a "$elizaOS Token Information" card that links to "Tokenomics, contract addresses, vesting schedules, and token release details," which is disclosure, not enforced protocol utility.
- On chain I independently confirmed both mints, their decimals, freeze authorities (both null), mint authorities (both non null), supplies, and the legacy Token-2022 extension set, using `getAccountInfo` and `getTokenSupply` against Solana mainnet RPC, plus DexScreener and CoinGecko for market data. Largest holder enumeration was rate limited by the public RPC, so I did not confirm a specific single treasury account address on chain and I do not invent one.

### Claim vs reality
- "Open source TypeScript framework for autonomous AI agents with an agent runtime and plugin system." CONFIRMED IN CODE. Real `elizaOS/eliza` repo, MIT licensed, active, with `@elizaos/core` `AgentRuntime` and a documented plugin contract. This is a genuine positive and is credited.
- "Large plugin and integration ecosystem (90+ connectors, on device models)." CONFIRMED IN CODE. The `plugins/` tree and docs support a broad first party plugin set and local inference. Marked confirmed at the framework level.
- "ai16z is an autonomous investor DAO (an AI venture fund making onchain investments)." OVERSTATED. The construct existed as a launchpad style DAO and treasury narrative, but there is no framework code that makes the token an autonomous onchain investing protocol; investing and treasury actions are governance and operational, not token enforced logic.
- "The token has onchain protocol utility, buybacks, or staking." OVERSTATED bordering FALSE. The open source runtime never references the token as an input, and the docs describe no enforced buyback, fee capture, or staking mechanism. The token is effectively a governance and meme asset tied to a real framework and a DAO treasury, not a protocol token.
- "Fixed or renounced token supply." OVERSTATED. Freeze is renounced on both tokens, which is good, but mint authority is live on both. The legacy mint is controlled by a program PDA and the new `ELIZAOS` mint by a 1 of 2 multisig where one signer suffices, so supply is not provably capped by renouncement.
- "Active flagship AI project token." Reality is a wind down. Eliza Labs declared the native token obsolete after a Burwick Law settlement and is dissolving the foundation; the legacy `ai16z` market has collapsed to about $336K and the migrated `ELIZAOS` token sits near all time lows. This is material adverse status for any holder.

### Severity
- Critical: The native token is being rendered obsolete. Eliza Labs announced a foundation wind down and declared the token dead after settling a Burwick Law class action, creating abandonment and discontinuity risk for holders of both `ai16z` and `ELIZAOS`.
- High: Live (non renounced) mint authority on both mints. Legacy mint authority is a program PDA; the new `ELIZAOS` mint authority is a 1 of 2 multisig that a single signer can operate, so supply can be inflated.
- High: No enforced onchain protocol utility. The open source runtime does not consume the token; value rests on narrative, governance, and a treasury rather than code enforced demand.
- Medium: Thin and fragmented liquidity. The legacy deepest pool holds about $36K and the token is a dead market; the new token liquidity is modest and there is no evidence of a locked LP position.
- Medium: Migration and duplication risk. Two live tokens (`ai16z` and `ELIZAOS`) plus mutable Token-2022 metadata on the legacy mint create holder confusion and an impersonation surface during a wind down.
- Low: The legacy Token-2022 metadata update authority is the same non null PDA as the mint authority, so on chain token metadata remains mutable.
- Informational: The `elizaOS/eliza` framework is a genuine, MIT licensed, actively developed open source project. It is credited on its own merits and is deliberately separated here from the token security assessment.
