# Assisterr (ASRR): Whitepaper Claims vs Code Reality

**Score: 45/100, MEDIUM RISK**

**Verdict: FLAGGED**

Date: 2026-08-05
Auditor: MEFAI Security, source code and on chain deep audit (ICE/ION style)
Scope: whole project (product, code, on chain state, token utility). Token is secondary but included. No team analysis. Read only, public sources.

---

### Token (live, data as of 2026-08-05)

| Field | Value |
|---|---|
| Symbol | ASRR |
| Solana mint | `ASRRjA1R4RHVk5H9QKKm1jaQqMkxvv6nh5EypPrvwmxQ` |
| Program owner | `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (standard SPL Token program, not Token 2022) |
| Decimals | 9 |
| Supply | 99,998,164.44 ASRR (raw `99998164435243664`); max 100M |
| Mint authority | `null` (revoked, fixed supply, non inflationable) |
| Freeze authority | `null` (holders cannot be frozen) |
| Origin | Token generation event / IEO, 30 May to 06 Jun 2025 (KuCoin launchpool), plus prior funding rounds. Not a pump.fun launch (no pump suffix on the mint; distributed via centralized exchange IEO). A separate BNB Smart Chain deployment also exists. |
| Price | ~$0.001352 |
| Market cap / FDV | ~$135,227 / ~$135,227 (CoinGecko lists market cap equal to FDV; circulating about 14 million of the 100 million total) |
| 24h volume | ~$2.89 (essentially nil on CoinGecko; primary pool Meteora DAMM V2 ASRR/WSOL) |
| ATH / ATL | $0.5009 (03 Jun 2025), now down ~99.7% / $0.001159 (29 Jul 2026) |
| Venues | Tradeable across roughly 18 venues historically (Kraken, Bybit, KuCoin, LBank, Meteora), but live liquidity and volume have collapsed |

RPC verification: `getAccountInfo` and `getTokenSupply` on `api.mainnet-beta.solana.com` (apiVersion 4.1.0, slot 437445634), reconfirmed independently on `solana-rpc.publicnode.com`. Both authorities returned `null` on both nodes.

### Websites

- Product: https://www.assisterr.ai/
- Docs / whitepaper: https://assisterr.gitbook.io/white-paper/
- Litepaper: https://www.assisterr.ai/litepaper-assisterr.pdf

### GitHub

- Organization: https://github.com/assister-xyz (6 public repositories)

---

## Severity Summary

| ID | Finding | Severity |
|---|---|---|
| ASRR 001 | Flagship SLM training and hosting stack is a closed centralized SaaS behind `api.assisterr.ai`; no public training, weights, or inference code | HIGH |
| ASRR 002 | No on chain program governs ASRR. It is a plain SPL token; all staking, rewards, treasury, and revenue share are off chain accounting and unverifiable | HIGH |
| ASRR 003 | "Community owned" and "decentralized governance" (Assisterr Treasury Model) have no on chain implementation. No DAO on Realms or Squads | MEDIUM |
| ASRR 004 | Headline traction (7,690 SLMs, 1.15M users) is self reported and cannot be reconciled with on chain state or public code | MEDIUM |
| ASRR 005 | Token utility flywheel is aspirational. Market is effectively dead (~$2.89 daily volume, ~99.7% below ATH) | MEDIUM |
| ASRR 006 | Positive: mint and freeze authorities both `null`; fixed 100M supply; not a pump.fun cash grab; a real, actively developed off chain codebase exists | INFO |

---

## Why This Report Exists

Assisterr markets itself as a Solana based DePIN for an "AI economy" of community built Small Language Models (SLMs), where anyone can create, own, and monetize specialized AI agents, and where the ASRR token powers deployment fees, agent treasuries, staking, governance, and reward distribution. The litepaper is titled "The DeAI Gig Economy for Mixtures of Small Language Models" and the whitepaper describes "a decentralized no code platform and infrastructure stack."

Those are two very different promises. One is a working AI product with a token attached. The other is a decentralized, community owned, on chain AI network. This report separates the two by reading the actual public source in the `assister-xyz` organization and querying the live Solana state of the ASRR mint, then testing each flagship claim against what is verifiable. Where the core is closed, this report says so plainly and does not credit unseen code.

## Method

1. Identified the real code home: the GitHub organization `assister-xyz` (six repositories), reached from the whitepaper and product site.
2. Read the public repositories and their file trees directly: `slm-integrations`, `quality-oracle`, `quality-oracle-demo`, two 2024 hackathon repos, and one archived repo.
3. Queried the live SPL mint on two independent Solana RPC providers for decimals, supply, mint authority, and freeze authority.
4. Checked for any Solana on chain program tied to Assisterr (staking, rewards, governance, treasury), including a search for a DAO on Realms and Squads.
5. Pulled live market data (CoinGecko) for price, capitalization, volume, and venues.
6. Compared whitepaper and litepaper claims against the above, tagging each CONFIRMED IN CODE / OVERSTATED / FALSE with a file or an on chain fact.

## The Foundation: what is actually public

The `assister-xyz` organization contains six repositories, and the character of the whole project can be read off their contents:

- `eth-denver-ai-web3-hackathon-2024` (Python, last pushed Mar 2024) and `renaissance-solana2024-hackathon` (Python, last pushed Apr 2024): hackathon submissions, two years stale.
- `wb`: archived, empty of signal.
- `slm-integrations` (last pushed May 2025): a thin client SDK plus documentation. Its tree is just `README.md`, `Examples/slm.py`, and three Jupyter notebooks (`1_Basic_API_usage.ipynb`, `2_Twitter_news_poster.ipynb`, `3_SOLANA_onchain_bot.ipynb`).
- `quality-oracle` and `quality-oracle-demo` (last pushed Apr 2026): a newer, substantial off chain Python and TypeScript project ("AgentTrust") for challenge response quality scoring of AI agents and MCP servers.

The one thing missing from all six repositories is the flagship product. There is no SLM training pipeline, no fine tuning code, no model weights, no inference or hosting server, and no Solana on chain program (zero Rust or Anchor files anywhere in the org). The actual SLM engine, the marketplace, the data market, and the "AI economy" accounting are not in the public source. They live behind a closed backend at `https://api.assisterr.ai/`, which the public client SDK simply calls.

## Claim 1: an open, verifiable, decentralized SLM stack

> CLAIM. "a decentralized no code platform and infrastructure stack that empowers individuals and organizations to build, tokenize, and monetize AI agents powered by modular Specialized Language Models," with "Solana native Architecture: Ultra low latency and scalable infrastructure." (whitepaper)

**REALITY: FALSE.** The core is a centralized SaaS, and none of it is public or verifiable.

EVIDENCE. The only SLM code in the org is `slm-integrations/Examples/slm.py`. It hardcodes `ASSISTERR_BASE_URL='https://api.assisterr.ai'` and, for every call, POSTs to `f'{ASSISTERR_BASE_URL}/api/v1/slm/{self.model}/chat/'` with an `X-Api-Key` header. The `README.md` repeats the same base URL and lists eight endpoints (stateless chat, session chat, session create and delete, history). The prompt builder wraps user text in Llama family chat markers (`<|start_header_id|>`, `<|eot_id|>`), which indicates the hosted backend serves off the shelf Llama style models, not a novel decentralized training stack. There is no training, no weights, no hosting, and no on chain inference anywhere in the public code. "Decentralized" and "infrastructure stack" describe a closed API that the audit cannot inspect.

IMPACT. The defining product claim is unverifiable by design. Per the transparency standard, unseen closed code is not credited. This is the single largest driver of the score cap.

## Claim 2: an on chain program governs ASRR (staking, rewards, revenue share)

> CLAIM. ASRR powers "deployment fees," "agent treasuries," "governance, staking, and reward distribution," and "distribution of rewards to Data Contributors and Validators." (whitepaper and Solana Compass)

**REALITY: FALSE.** There is no on chain program. ASRR is a plain SPL token, and every listed mechanism is off chain accounting.

EVIDENCE. The mint `ASRRjA1R4RHVk5H9QKKm1jaQqMkxvv6nh5EypPrvwmxQ` is owned by the standard SPL Token program (`Tokenkeg...`), not by any custom Assisterr program, and not even Token 2022. There is no staking program, no rewards program, and no revenue share program in the public org (no Rust or Anchor code exists). A search for an Assisterr DAO on Realms and Squads returns nothing. Whatever "staking" or "reward distribution" exists is executed and tracked by the same closed backend, off chain, with no on chain enforcement a holder can audit. The token contract itself carries zero business logic beyond standard SPL transfer.

IMPACT. Token holders have no cryptographic guarantee behind any of the advertised token utility. Rewards, fees, and treasuries are promises kept in a private database.

## Claim 3: community owned, decentralized governance, treasury backed sustainability

> CLAIM. Agents are "governed by the very communities that use and maintain them," and the Assisterr Treasury Model (ATM) enables "fractional ownership, decentralized governance, and treasury backed sustainability." (whitepaper)

**REALITY: OVERSTATED.** The governance and treasury are conceptual. There is no on chain DAO, and the treasury is wallet custody, not a program.

EVIDENCE. No `spl-governance` (Realms) realm, no Squads multisig, and no governance program is associated with the token or present in the code. The whitepaper itself concedes implementation details are absent for the ATM. With no on chain voting, no on chain proposal execution, and no program controlled treasury, "decentralized governance" reduces to a marketing frame over centrally held funds. On chain, the project treasury is one or more ordinary wallets, not a governed program.

IMPACT. "Community owned" is not enforceable. Control remains centralized regardless of the label.

## Claim 4: a thriving SLM and agent marketplace with mass adoption

> CLAIM. "over 7,690 SLMs deployed and 1.15M users." (marketing and third party summaries)

**REALITY: OVERSTATED.** The marketplace exists as a hosted web product, but the headline traction is self reported, off chain, and not reconcilable with anything public.

EVIDENCE. None of these counts touch the chain, and none can be verified from the source, because the marketplace is part of the closed backend, not the public repos. The one independent, chain adjacent figure available is far smaller: Solana Compass states "over 60 SLMs deployed for Solana protocols." The gap between "7,690 SLMs / 1.15M users" and roughly 60 verifiable protocol deployments is the difference between a total platform tally kept privately and what can actually be observed. Daily on chain token activity (below) is inconsistent with a live user base of over a million.

IMPACT. Adoption claims should be read as unaudited platform metrics, not verified usage.

## Claim 5: the ASRR utility flywheel

> CLAIM. "As agent adoption grows, the demand for ASRR grows with it, creating a flywheel effect." (whitepaper)

**REALITY: OVERSTATED, and contradicted by the market.** Utility is aspirational and the flywheel has not materialized.

EVIDENCE. With no on chain utility mechanism (Claim 2) and adoption unverifiable (Claim 4), the flywheel has no observable engine. The live market reflects this: price ~$0.001352, market capitalization ~$135K, and 24h volume of roughly $2.89 on CoinGecko, down ~99.7% from the June 2025 all time high of $0.5009, with an all time low set as recently as 29 July 2026. A token whose demand rose with genuine product usage would not print near zero volume at a fresh all time low. The token is technically tradeable but effectively illiquid.

IMPACT. Buying ASRR is a bet on future centralized product success, not on a working on chain economy.

## Positive Findings

Credited on evidence:

1. **Clean token hygiene (CONFIRMED IN CHAIN).** Mint authority is `null` and freeze authority is `null`, confirmed on two independent RPC providers. Supply is fixed at roughly 100M, non inflationable, and holders cannot be frozen. This is correct, conservative token setup and removes the most common rug vectors.
2. **Not a pump.fun cash grab.** ASRR launched through a structured token generation event and exchange IEO (KuCoin) with prior funding rounds and later listings (Binance Alpha, Bybit, Kraken), rather than an anonymous bonding curve mint. The mint address carries no pump suffix.
3. **A real, actively maintained codebase exists.** `quality-oracle` (AgentTrust) is a substantive off chain system: a FastAPI service with 14 route modules, an evaluation and scoring core, W3C Verifiable Credential issuance, x402 payment verification, and a large test suite, last pushed Apr 2026. It is not the flagship SLM stack and it is entirely off chain, but it demonstrates ongoing engineering rather than an abandoned shell.
4. **An honest client SDK.** `slm-integrations` does not pretend to be more than it is; it plainly documents the hosted API, and that API is functional.

## Conclusion

Assisterr is a real, centrally operated AI SaaS with a Solana token bolted on, marketed as a decentralized, community owned, on chain AI network. The gap between those two descriptions is the finding.

Reading the actual public source settles it. The flagship SLM training and hosting stack, the marketplace, and the "AI economy" accounting are all closed, living behind `api.assisterr.ai`; the public repositories amount to a thin client SDK, two stale hackathon entries, and one genuinely active but off chain quality scoring project. There is no Solana on chain program governing ASRR at all: it is a plain SPL token, so staking, rewards, agent treasuries, revenue share, and "decentralized governance" are private off chain bookkeeping with no cryptographic guarantee. The advertised traction is self reported and irreconcilable with the roughly 60 verifiable Solana deployments and with a market that now prints about $2.89 of daily volume at a fresh all time low, roughly 99.7% under its peak.

On the credit side, the token is set up cleanly (both authorities revoked, fixed supply), it was not a pump.fun style launch, and the team is visibly still shipping code, even if that code is not the decentralized product being sold. This keeps the risk at MEDIUM rather than HIGH: the primary danger here is not an exit scam mechanism but overstated decentralization, unshipped on chain utility, a closed and unverifiable core, and a collapsed market.

Net: a legitimate centralized product wearing a decentralization narrative it has not built on chain. The closed core caps the score and drives a transparency flag; the clean token and real engineering keep it out of the danger tier.

**Score: 45/100. Verdict: FLAGGED. Risk: MEDIUM.**

Verdict consistency: score 45 is at or below 50, consistent with a FLAGGED verdict.

Claim tally: CONFIRMED 1 (token hygiene, in chain) / OVERSTATED 3 (governance and treasury, traction, utility flywheel) / FALSE 2 (open verifiable SLM stack, on chain program governing ASRR).
