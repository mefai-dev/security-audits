# Fartcoin (FARTCOIN): Whitepaper Claims vs Code Reality

**Score: 62/100, PASSED (Low security risk, high market risk)**

**Date:** 2026-08-06

**Token (live):** Solana SPL mint `9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump`, classic SPL Token program (`TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA`), decimals 6, supply 999,974,877.033989, mint authority null, freeze authority null, mint account size 82 bytes (no Token-2022 extensions, no transfer fee). Confirmed live and tradeable via public RPC on 2026-08-06.

**Websites:** infinitebackrooms.com (lore archive, not a product), Twitter/X @FartCoinOfSOL. No project owned application, dashboard, or documentation site.

**GitHub:** None for this token. Searches surface `fartcoin-project/fartcoin`, `HashBasher/fartcoin`, `plazma48/fartcoin`, and `FartCoin-Official`, but every one of these is an unrelated proof of work coin (Dogecoin and Litecoin style forks) and `fartcoin.gold` (ticker FRTC) is a separate, unrelated token. None of them are the Solana FARTCOIN under audit.

---

## Severity Summary

| Area | Finding | Severity |
| --- | --- | --- |
| Mint authority | Null (renounced). No new supply can ever be minted. | None (positive) |
| Freeze authority | Null (renounced). No account can be frozen or blacklisted. | None (positive) |
| Token standard | Classic SPL, 82 byte mint, zero extensions. No transfer fee, no transfer hook, no honeypot vector. | None (positive) |
| Product / protocol | No product, no protocol, no on chain program beyond the plain SPL token. | Informational |
| Utility claims | Novelty features described on listings (fart joke token claiming, sound on transfer) do not exist in code. | Low (cosmetic overstatement) |
| AI agent narrative | Concept emerged from an AI text conversation; a human deployed the token. No live AI runs or controls it. | Low (narrative, not code) |
| Market / speculation | Pure meme, no cash flow, roughly 95 percent below all time high. | High market risk |

Net: no security or scam findings. The remaining risk is ordinary speculative meme volatility, which is a market risk, not a code defect.

---

## Why This Report Exists

FARTCOIN is one of the largest so called AI meme coins on Solana, with a market capitalization near 133 million USD and a name that invites easy dismissal. The interesting audit question is not whether the name is silly, but whether the token quietly hides the machinery that most rug pulls rely on: a live mint authority to dilute holders, a freeze authority to trap sellers, a Token-2022 transfer fee or transfer hook to tax or block transfers, or a marketed product that does not exist and is used to justify a valuation.

This report tests the popular framing of FARTCOIN against the actual bytes on chain. It also separates two things that get conflated in coverage: the colorful origin story (an AI agent conceiving the meme) and the technical reality (a plain fungible token deployed by a human). The goal is a fair verdict. A clean, honestly framed meme with renounced authorities and no fake utility is not a scam, and it is not scored as one here. It also has no product to credit, and that is stated plainly.

## Method

Read only and public. Two independent classes of evidence were used.

1. On chain state. Direct JSON RPC calls to Solana mainnet (`getAccountInfo` with jsonParsed encoding, and `getTokenSupply`) against the mint address. This returns the authoritative authority fields, decimals, supply, owning program, and account size that define what the token can and cannot do. These values are not self reported by the project; they are consensus state.

2. Public material. Listing pages (CoinGecko, CoinMarketCap and similar), explainer articles, and the token origin coverage were reviewed to capture every material claim, then each claim was checked against the on chain state and against the absence or presence of a real codebase.

Every claim below is labeled CONFIRMED, OVERSTATED, or FALSE, with the raw evidence attached.

## The Foundation: A Plain SPL Token With Nothing Behind It

FARTCOIN launched on 2024-10-18 through pump.fun, the Solana memecoin launchpad. The concept came out of the Infinite Backrooms experiment run by researcher Andy Ayrey, in which a customized Claude model (Truth Terminal, also styled Terminal of Truths) and another AI instance generated the idea of a fart themed coin. Truth Terminal did not deploy anything; an anonymous person minted the token on pump.fun and sent roughly 20.1 million tokens to a Truth Terminal wallet as a symbolic and marketing gesture. There was no presale, no venture allocation, and no reserved team supply.

Technically, the foundation is the simplest object on Solana: a single Mint account under the original SPL Token program, 82 bytes long, holding no extensions. Everything the community layers on top (the lore, the AI branding, the exchange listings) sits on this one plain token. There is no separate program, no vault, no staking contract, no governance module, and no repository of source code that belongs to the project. The audit surface is therefore tiny, which is itself a meaningful finding: there is very little that can go wrong because there is very little there.

## Claim 1: "FARTCOIN is a live, freely tradeable Solana token"

**CLAIM:** Listings present FARTCOIN as a live SPL token with about 1 billion supply, trading actively across exchanges.

**REALITY: CONFIRMED.** The mint exists, is initialized, and reports a real circulating supply. Independent market pages show active daily volume in the tens of millions of USD and a rank near the top 220 assets.

**EVIDENCE:** `getTokenSupply` returns amount 999974877033989 at 6 decimals, i.e. 999,974,877.033989 tokens (about 25,123 tokens have been burned from the 1,000,000,000 launch supply). `getAccountInfo` shows `isInitialized: true`, owner `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA`. CoinGecko on the same date reports price near 0.133 USD, market cap near 133 million USD, and near 1 billion circulating supply against the same contract address.

**IMPACT:** Positive. The asset is exactly what it presents itself to be: a live, liquid fungible token.

## Claim 2: "The token is fully renounced and cannot be diluted, frozen, or taxed" (token hygiene)

**CLAIM:** Both mint and freeze authorities are renounced, so no one can print more supply, freeze wallets, or block sells; there is no hidden transfer tax.

**REALITY: CONFIRMED.** This is the single most important security fact and it checks out completely.

**EVIDENCE:** In `getAccountInfo` the parsed mint info shows `"mintAuthority": null` and `"freezeAuthority": null`. Because mint authority is null, the fixed supply can never be increased and holders cannot be diluted. Because freeze authority is null, no account can be frozen and no seller can be locked out (the classic Solana honeypot). The owning program is the classic SPL Token program, and the mint account `space` is 82 bytes, which is the exact size of a standard mint with zero extensions. A Token-2022 mint carrying a transfer fee, a transfer hook, permanent delegate, or default frozen state would be owned by the Token-2022 program (`TokenzQd...`) and would be larger than 82 bytes. Neither condition is present, so there is no transfer fee, no transfer hook, no blacklist, and no honeypot mechanism of any kind.

**IMPACT:** Positive and strong. On the dimensions that matter for holder safety at the token level, FARTCOIN is clean.

## Claim 3: "FARTCOIN was created by an AI agent"

**CLAIM:** Coverage frequently describes FARTCOIN as created, invented, or run by an autonomous AI agent, which reads as ongoing machine agency behind the coin.

**REALITY: OVERSTATED.** The meme concept was generated inside an AI text conversation, and a symbolic token allocation was sent to the Truth Terminal wallet. That is the full extent of the AI involvement. The deployment was performed by an anonymous human on pump.fun, and no AI runs, governs, upgrades, or controls the token. There is no autonomous agent embedded in or attached to the mint.

**EVIDENCE:** The origin coverage consistently states that Truth Terminal conceived the idea and that an anonymous developer launched it. On chain there is no program, oracle, or agent wired to the mint; it is an inert SPL token whose behavior is fixed by the SPL Token program. The AI story is genuine as history but is narrative, not executing code.

**IMPACT:** Low. The AI framing is a marketing and cultural narrative. It becomes a problem only if read as functional utility, which it is not, and which the honest listings do not claim.

## Claim 4: "FARTCOIN has novelty product features" (joke to earn, sound on transfer)

**CLAIM:** Some listing descriptions state that FARTCOIN lets users submit fart jokes or memes to claim initial tokens and includes a gas fee system that plays a digital fart sound on each transaction.

**REALITY: FALSE.** No such mechanism exists. There is no on chain program to accept jokes, no claim contract, and the SPL Token program does not and cannot emit sounds or run custom logic on transfer for a classic mint with no transfer hook. These lines are flavor text that describes nothing real.

**EVIDENCE:** The mint has zero extensions (82 byte account, classic program), so a transfer executes standard SPL logic only. There is no auxiliary program owned by or referenced from the mint. No repository implements these features (the GitHub results are unrelated forks of other coins). The described features are absent from both code and chain.

**IMPACT:** Low. This is a cosmetic overstatement in third party copy rather than a valuation justifying utility claim by the project. It is labeled FALSE because the specific described features do not exist, but it does not create holder risk.

## Claim 5: "There is no product, protocol, GitHub, or on chain program beyond the SPL token"

**CLAIM:** The honest framing of FARTCOIN is that it is a pure meme with no product, no protocol, no maintained codebase, and nothing on chain except the token.

**REALITY: CONFIRMED.** This matches reality precisely.

**EVIDENCE:** No project owned website application, no whitepaper of substance, no documented protocol, and no GitHub repository belonging to this token were found. The repositories surfaced by search (`fartcoin-project/fartcoin` forked from Dogecoin, `plazma48/fartcoin` built from Litecoin, `HashBasher/fartcoin`, `FartCoin-Official`) are unrelated proof of work coins, and the `fartcoin.gold` FRTC whitepaper is a different token entirely. On chain there is only the single Mint account. The primary linked site, infinitebackrooms.com, is a lore and conversation archive, not a product.

**IMPACT:** Informational. This is the correct baseline for valuing the asset: there is nothing to build a fundamental case on, positive or negative.

## Claim 6: "FARTCOIN is a pure speculative meme with no utility"

**CLAIM:** FARTCOIN offers no yield, no cash flow, no governance rights, and no functional use; its value is entirely cultural and speculative.

**REALITY: CONFIRMED.** The token confers holding and transfer, nothing more.

**EVIDENCE:** No staking, no fee capture, no treasury mechanism, and no governance are present on chain. Price history underlines the speculative nature: an all time high near 2.52 USD on 2025-01-19 versus roughly 0.13 USD on the audit date, a decline of about 95 percent from peak. Value tracks attention, not fundamentals.

**IMPACT:** High market risk for holders, but this is honestly disclosed by the meme framing rather than concealed. It is a speculation risk, not a security or fraud finding.

## Positive Findings

1. Mint authority is null. Supply is permanently capped; holders cannot be diluted by new minting.
2. Freeze authority is null. No wallet can be frozen or blacklisted, removing the most common Solana honeypot vector.
3. Classic SPL token with no extensions. No transfer fee, no transfer hook, no permanent delegate, and no default frozen state. Transfers behave as plain SPL transfers.
4. Fair launch structure. Deployed on pump.fun with no presale, no venture allocation, and no reserved team supply; nearly the entire fixed supply is in circulation.
5. Honest framing. The dominant public description is that it is a meme with no utility. There is no fabricated product or protocol used to inflate the token, which is the key distinction between a clean meme and a scam.

## Conclusion

FARTCOIN is exactly what an honest reading says it is: a clean, fully renounced, classic SPL meme token with no product, no protocol, no codebase, and no utility beyond being held and traded. The on chain state confirms the genuinely important safety properties directly. Mint authority and freeze authority are both null, the mint carries no Token-2022 extensions, and there is therefore no dilution path, no freeze or blacklist path, and no transfer tax or honeypot. The celebrated AI agent origin is real as history but is narrative rather than executing code, and the occasional listing copy about joke to earn claiming and sound on transfer describes features that do not exist. Neither of those creates holder risk.

The fair verdict is that this is not a scam and should not be scored as one. It earns a passing score on token hygiene and honest framing. It cannot earn a high score because there is no product, no cash flow, and no utility to credit, and because the market risk is severe: the token sits roughly 95 percent below its peak and its value is pure speculation on attention. The honest one line summary is: clean token, pure meme, no utility.

**Score: 62/100, PASSED.** Low security risk at the token level; high market and speculation risk for holders.
