# Arc (ARC): Whitepaper Claims vs Code Reality

**Score: 46/100, RISK MEDIUM**

**Date:** 2026-08-05
**Verdict:** FLAGGED (decoupled token on a genuinely excellent framework)

**Token (live, on chain verified 2026-08-05):**
- Mint: `61V8vBaqAGMpgDQi4JcAwo1dmBGHsyhzodcPqnEVpump` (Solana)
- Owner program: `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (classic SPL Token, account space 82, no Token-2022 extensions)
- Decimals: 6
- Supply: 999,982,742.288065 ARC (raw 999982742288065, about 1 billion)
- Mint authority: **null** (revoked, no new supply can ever be minted)
- Freeze authority: **null** (holder accounts cannot be frozen)
- Origin: pump.fun launch (mint address carries the `pump` suffix; corroborated by market data sources), launched December 2024
- Market (CoinGecko, 2026-08-05): about $0.0511, market cap about $51.06M, FDV about $51.06M (fully circulating), 24h volume about $2.85M, ATH $0.6232 on 21 Jan 2025 (about 92 percent below ATH). Listed on Gate, BitMart and others. Liquid and tradeable.

**Websites:** rig.rs (framework docs), ryzome.ai (product), plus third party listing pages (CoinGecko, Gate, BitMart, crypto.com university)

**GitHub:** github.com/0xPlaygrounds/rig (framework, MIT, 8,179 stars, v0.41.0), github.com/0xPlaygrounds/rig-onchain-kit (agent blockchain toolkit)

---

## Severity Summary

| # | Finding | Marketing verdict | Severity |
|---|---------|-------------------|----------|
| 1 | ARC is a "utility token for processing transactions and rewarding Rig contributors" | FALSE | HIGH |
| 2 | Ryzome agent service fees settle in ARC (85 percent providers / 10 percent treasury / 5 percent ops) | FALSE | HIGH |
| 3 | Per agent DAO governance, Proxy NFT super votes, staking based voting | FALSE | HIGH |
| 4 | Staking rewards, graded lock ups, airdrops, NFT integration | FALSE | MEDIUM |
| 5 | Arc Forge launchpad and Handshake developer rewards powered by ARC | OVERSTATED | MEDIUM |
| 6 | An "agent economy" powered by the ARC token across the ecosystem | OVERSTATED | MEDIUM |
| 7 | An on chain program / DAO / treasury governs ARC | FALSE | MEDIUM |
| P1 | Rig is a genuine, substantial, widely adopted agent framework | CONFIRMED IN CODE | Positive |
| P2 | Token contract is technically clean (authorities revoked, fixed supply) | CONFIRMED ON CHAIN | Positive |
| P3 | Token is tradeable and liquid on major venues | CONFIRMED | Positive |

Counts: **CONFIRMED 3, OVERSTATED 2, FALSE 5.**

---

## Why This Report Exists

Arc, also branded AI Rig Complex, is one of the flagship names of the 2024 to 2025 "AI agent" token wave on Solana. Its pitch is unusually credible on the surface: unlike the many meme coins that point at a thin demo, Arc points at Rig, a large, professional, open source Rust framework for building LLM and agent applications, authored by 0xPlaygrounds. Rig is real and good. The question a source auditor must answer is narrower and sharper: does owning the ARC token give a holder any claim on, or role inside, that framework or any Arc product? Marketing across listing sites describes ARC as a transactional utility token, a governance token with per agent DAOs, a staking asset, and the settlement currency of an "agent economy." This report reads the actual Rust source of every relevant 0xPlaygrounds repository and queries the live Solana mint to test each of those claims against code and chain. No team analysis is included; this is a code and chain audit.

## Method

1. Cloned and read the primary framework repository `0xPlaygrounds/rig` (v0.41.0, about 237,000 lines of Rust across 20 crates and 65 example packages) and the one blockchain touching repository `0xPlaygrounds/rig-onchain-kit`.
2. Grepped both codebases for the exact ARC mint address, for `pump.fun`, and for every token utility term (staking, governance, DAO, treasury, tokenomics, airdrop, buyback, reward). Grepped dependency manifests for any Solana, Anchor, `spl-token`, or web3 dependency.
3. Enumerated the entire `0xPlaygrounds` GitHub organization to find any repository containing an Anchor program, smart contract, or tokenomics code.
4. Read the marketed "agent economy" product (Ryzome) to check whether it settles in ARC.
5. Queried the live Solana mainnet mint via `getAccountInfo` (jsonParsed) and `getTokenSupply` to confirm address, program owner, decimals, supply, mint authority and freeze authority. Cross checked market state on CoinGecko.

Everything below is reproducible from public source and a public RPC.

## The Foundation: Rig is a real, substantial framework (CONFIRMED IN CODE)

Rig is not vaporware. It is a mature, well engineered Rust agent framework, and this deserves explicit credit.

```text
rig v0.41.0 (MIT license, 8,179 GitHub stars)
  20 crates, 65 example packages, ~237,345 lines of Rust
  crates/rig-core alone: ~86,756 lines
```

```text
crates/rig-core/src/providers/  (20+ model providers, one unified interface)
  anthropic  openai  gemini  cohere  mistral  xai  groq  deepseek
  ollama  huggingface  together  openrouter  perplexity  copilot
  hyperbolic  voyageai  minimax  moonshot  mira  zai  azure  bedrock ...
```

```text
crates/rig-core/src/  subsystems
  completion/   embeddings/   vector_store/   tool/ (+ tool/builtin)
  loaders/ (epub, pdf)   telemetry/ (OpenTelemetry GenAI semconv)   client/
Dedicated vector store crates: qdrant, lancedb, mongodb, neo4j, postgres,
  sqlite, surrealdb, milvus, scylladb, s3vectors, helixdb, memory, ...
```

This is a genuine providers plus tools plus RAG plus vector store agent framework with real breadth, WASM support, and strict lint hygiene (`unwrap_used`, `panic`, `todo` all denied in the workspace `Cargo.toml`). Framework adoption is real and independent of any token. That is the crux of the whole report: the good thing here (Rig) and the traded thing here (ARC) are two separate objects.

## Claim 1: "ARC is an application utility token for processing transactions and a reward for contributors to the open source Rig framework"

**CLAIM (paraphrased from CoinGecko and multiple listing pages):** ARC settles protocol service fees when agents access services and rewards open source Rig contributors.

**REALITY: FALSE.**

The Rig framework contains zero references to the ARC token, to any Solana mint, or to any payment, reward, staking, or governance path. It has no Solana, Anchor, `spl-token`, or web3 dependency at all. Rig is a pure LLM orchestration library; it never touches a blockchain.

**EVIDENCE:**
```text
# In the full rig tree, searching source, docs and manifests:
$ grep -riE "61V8vBaq|pump\.fun|pumpfun" rig/   --include=*.rs --include=*.md --include=*.toml
   (no matches)

$ grep -riwE "staking|governance|tokenomics|airdrop|buyback|treasury|dao" rig/ ...
   (no matches)

$ grep -riE "\$ARC|arc\.fun|agent economy" rig/ --include=*.md --include=*.rs
   (no matches)

$ grep -riE "solana|anchor-lang|spl-token|web3" rig/ --include=*.toml --include=*.rs
   (no matches)
```
Contributors to Rig are credited in `CHANGELOG.md` and git history; there is no code path, on or off chain, that pays them in ARC. The "reward for contributors" utility exists in marketing only.

**IMPACT:** The single most repeated utility claim (ARC pays for framework usage and rewards contributors) is unsupported by a single line of the framework it names.

## Claim 2: "Agent service fees are settled in ARC through Ryzome, split 85 percent to providers, 10 percent to the ARC treasury, 5 percent to operations"

**CLAIM (Gate crypto wiki and others):** When an agent invokes a service through the Ryzome app store, the fee is paid in ARC and split to service providers, an ARC treasury, and operations.

**REALITY: FALSE.**

Ryzome (ryzome.ai) is the actual product built by the Rig team, and it is a context management SaaS ("centralize scattered context into a searchable library"), sold with a conventional "Try For Free" fiat pricing model. Its site contains zero mentions of ARC, $ARC, staking, a DAO, a treasury, or fees settled in any token. There is no on chain fee splitting program anywhere in the organization. The `ryzome-mcp-plugins` and `hermes-ryzome-plugin` repositories are TypeScript MCP integrations with no token logic.

**EVIDENCE:**
```text
ryzome.ai product page: context management SaaS, fiat "Try For Free" pricing.
  Token references (ARC / staking / DAO / fee split): ZERO.
0xPlaygrounds org: no repository contains a fee split, escrow, or treasury program.
On chain: the mint is owned by the standard SPL Token program (TokenkegQ...),
  not by any custom fee routing program.
```

**IMPACT:** A precise, quantified economic mechanism (85/10/5) is presented as fact. No such mechanism exists in code or on chain. This is fabricated tokenomics.

## Claim 3: "Each AI agent is managed by an independent DAO; Proxy NFT holders have super voting rights; Proxy Token holders govern through staking"

**CLAIM:** Governance is exercised per agent via DAOs, weighted by NFTs and staked tokens.

**REALITY: FALSE.**

No governance module, DAO program, voting logic, NFT weighting, or staking mechanism exists in any 0xPlaygrounds repository. The organization contains no Anchor program and no deployed governance program that the mint is subject to. On chain, the mint has both authorities revoked, so there is not even an administrative key that a DAO could plausibly control.

**EVIDENCE:**
```text
$ grep -riwE "staking|governance|dao|treasury|voting|proxy nft" \
      rig/ rig-onchain-kit/   --include=*.rs --include=*.md
   (no matches)

On chain getAccountInfo(mint).value.data.parsed.info:
   "mintAuthority": null,
   "freezeAuthority": null,
   "owner program": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"  (stock SPL, no governance)
```

**IMPACT:** "Per agent DAOs" and staking based governance are described as live features. Zero exist. HIGH severity misrepresentation.

## Claim 4: "Staking rewards, graded lock up rewards, airdrops and NFT integration"

**CLAIM:** Holders stake for graded rewards by lock up period, combinable with NFTs for governance weight and airdrop share.

**REALITY: FALSE.**

There is no staking contract, reward accrual, lock up, vesting, or airdrop distributor in any repository or on chain program. CoinGecko itself reports the token as fully circulating (circulating supply equals total supply equals about 999.98M, and FDV equals market cap), which is inconsistent with any enforced on chain lock up or staking escrow holding supply.

**EVIDENCE:**
```text
$ grep -riwE "stake|staking|lockup|vesting|airdrop|reward" rig/ rig-onchain-kit/ ...
   (no matches beyond generic library identifiers)
CoinGecko: circulating == total == max-adjacent; FDV == market cap  → nothing locked on chain.
```

**IMPACT:** Investors are told the token accrues yield through staking. No staking exists.

## Claim 5: "Arc Forge launchpad and the Handshake developer rewards program are powered by ARC"

**CLAIM:** ARC powers a launchpad ("Arc Forge") and a developer rewards program ("Handshake").

**REALITY: OVERSTATED.**

The only launch related code in the organization is `rig-onchain-kit/src/solana/deploy_token.rs`, and it is a generic pump.fun token launcher: it generates a random mint, uploads metadata to pump.fun IPFS, and creates a bonding curve buy. It is a tool an agent can use to deploy any token; it has no ARC gating, no ARC fee, and no reference to the ARC mint. No "Handshake" reward distributor exists in code.

**EVIDENCE:**
```text
rig-onchain-kit/src/solana/deploy_token.rs
   fn create_deploy_token_tx(params: DeployTokenParams)   // any token
   fn generate_mint() -> (Pubkey, Keypair)                // fresh random mint
   push_meta_to_pump_ipfs(...)  ->  POST https://pump.fun/api/ipfs
$ grep -riwE "61V8vBaq|handshake|forge|reward" rig-onchain-kit/src/  → no ARC mint, no reward program
```

**IMPACT:** A real, generic capability (deploy any token, trade any token) is rebranded as an ARC powered product. The ARC token plays no role in it.

## Claim 6: "An agent economy powered by the ARC token"

**CLAIM:** ARC is the economic layer uniting Rig, Ryzome, and the on chain agents into an "agent economy."

**REALITY: OVERSTATED.**

The three real products all exist and are competent, but none of them use ARC. Rig is a fiat/BYO API key library. Ryzome is a fiat SaaS. `rig-onchain-kit` is a general Solana plus EVM agent toolkit (transfers, Jupiter swaps via `src/solana/jup.rs`, generic pump.fun trading via `src/solana/pump.rs` and `trade_pump.rs`), built in partnership with `piotrostr/listen`, and it references the ARC mint nowhere. The only occurrences of the string "arc" in its source are Rust's `std::sync::Arc` smart pointer.

**EVIDENCE:**
```text
$ grep -riwE "arc" rig-onchain-kit/src | grep -v "std::sync::Arc"  → only Arc<T> pointers
$ grep -riwE "staking|governance|dao|treasury" rig-onchain-kit/src  → (none)
README: "companion crate ... in partnership with listen ... Solana and EVM networks"
```

**IMPACT:** The ecosystem is real; the token's place in it is not. This is the defining pattern of a decoupled associational token.

## Claim 7: "An on chain program, DAO, or treasury governs ARC"

**CLAIM (implicit across marketing):** ARC is administered by an on chain program and a treasury.

**REALITY: FALSE.**

The mint is a plain SPL token account (space 82, no Token-2022 extensions) owned by the stock SPL Token program. Both mint authority and freeze authority are null. There is no custom Anchor program that owns or governs the mint. Any "treasury" or "DAO wallet" referenced in marketing is, at most, an ordinary Solana wallet holding tokens; there is no program enforced treasury logic. Note: top holder concentration could not be independently pulled at audit time because free public RPC endpoints rate limited or plan gated `getTokenLargestAccounts`; this one distribution metric is therefore unverified here, while all authority and supply facts above are confirmed.

**EVIDENCE:**
```json
// getAccountInfo(61V8vBaq...pump, jsonParsed).value
{ "owner": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
  "space": 82,
  "data": { "parsed": { "type": "mint", "info": {
      "decimals": 6, "isInitialized": true,
      "mintAuthority": null, "freezeAuthority": null,
      "supply": "999982742288065" } } } }
```

**IMPACT:** There is no programmatic governance or treasury behind ARC. It is an authority revoked SPL token, nothing more, nothing less.

## Positive Findings

1. **Rig is a legitimately excellent open source framework (CONFIRMED IN CODE).** About 237,000 lines of well engineered Rust, 20+ model providers, 15+ vector store integrations, RAG loaders, OpenTelemetry GenAI telemetry, WASM support, strict lint policy, MIT licensed, 8,179 stars. This is one of the strongest Rust agent frameworks in existence and its adoption is real and token independent.
2. **`rig-onchain-kit` is real, functional tooling (CONFIRMED IN CODE).** Genuine Solana and EVM agent operations (transfers, Jupiter swaps, Privy signer isolation, pump.fun trading, token deployment), built with the `listen` project. Useful software, even though it grants ARC no special role.
3. **The token contract is technically clean (CONFIRMED ON CHAIN).** Mint authority and freeze authority are both revoked, supply is fixed at about 1 billion, it is a standard SPL mint with no Token-2022 traps. From a pure contract safety standpoint there is no honeypot, no dilution vector, and no freeze vector. This is not a Scam Shield danger.
4. **The token is liquid and widely traded (CONFIRMED).** About $51M market cap, roughly $2.85M daily volume, listed on Gate, BitMart and others, FDV equal to market cap (fully circulating).

## Conclusion

Arc is the textbook case of a **real, high quality open source framework paired with a fully decoupled token.** Rig deserves genuine respect as software; ARC deserves scrutiny as an asset. Every utility the token is marketed on (settling agent service fees, an 85/10/5 treasury split, per agent DAO governance, staking rewards, launchpad and developer reward programs, an "agent economy" powered by ARC) is absent from the code of every product that bears the Arc name and absent from any on chain program. Grepping the entire framework and the entire organization for the mint address, for token payment paths, and for staking or governance logic returns nothing. The token is an authority revoked pump.fun SPL launch whose value rests entirely on association with, and sentiment toward, the Rig framework, not on any code enforced claim on it.

The contract itself is technically safe, which is why this is not a HIGH risk rug rating. But the gap between the advertised utility and the on chain and in code reality is large and quantified in false specifics, which is why it cannot pass. **Score 46/100, FLAGGED, RISK MEDIUM:** a great framework, a clean but decoupled token, and a set of utility claims that exist only in marketing.

---
*Sources: on chain via Solana mainnet RPC getAccountInfo and getTokenSupply (2026-08-05); source from github.com/0xPlaygrounds/rig (v0.41.0) and github.com/0xPlaygrounds/rig-onchain-kit; product at ryzome.ai; market data from CoinGecko. Read only, public data.*
