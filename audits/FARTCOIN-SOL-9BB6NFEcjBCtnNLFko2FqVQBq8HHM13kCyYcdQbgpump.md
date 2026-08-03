# Security Audit Report: Fartcoin (FARTCOIN) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 3, 2026 |
| **Project** | Fartcoin |
| **Website** | https://dexscreener.com/solana/9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump |
| **Audit Type** | Whole Project (Claim versus Reality) |
| **Methodology** | Product and Traction Review + Website Frontend Review + Onchain Analysis |
| **Mefai Security Score** | **55/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Passed** |
| **Token / Contract** | FARTCOIN `9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump` (Solana) |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Fartcoin is a Solana SPL meme token that originated on pump.fun and its mint 9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump is confirmed live on chain. The mint is owned by the classic SPL Token program with an 82 byte account, so there are no Token 2022 extensions and no transfer fee. Both the mint authority and the freeze authority read null, meaning supply cannot be inflated and holder accounts cannot be frozen. Supply is fixed near 999,974,888.5 tokens at 6 decimals, and the token trades around a market cap of roughly 130 million USD on Raydium. It carries a rich AI agent narrative tied to the Terminal of Truths and GOAT lore, but on chain it is simply a plain meme token with no utility.

**Product reality:** MEME_NO_PRODUCT  |  **Traction:** VERIFIABLE  |  **Token utility:** SPECULATIVE_ONLY  |  **Delivery:** NA

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 1 |
| Informational | 4 |
| **Total** | **5** |

MEFAI whole project analysis places Fartcoin at 55 out of 100 (MEDIUM risk, Passed).

---

## 1. Project and Product Reality

Fartcoin has no formal product, no roadmap, and no promised utility, and that absence is intentional rather than a failure to deliver. It is a Solana SPL meme token that launched on pump.fun in October 2024 as a fair launch, with a self description that plainly frames it as tokenising farts with the help of bots. The narrative traces to the Terminal of Truths AI agent and the surrounding GOAT AI lore, which supplied cultural attention rather than any working software. Judged as what it claims to be, a meme, it is honest about having nothing to build, so there is no gap between promise and delivery. There is simply no application, protocol, or service to evaluate.

**Project level flags:** value is pure speculation and sentiment because the token has no product and no utility; price is highly volatile, having fallen from an all time high near 2.48 dollars to roughly 0.13 to 0.20 dollars; narrative depends on continued attention to the Terminal of Truths and GOAT AI meme lore

---

## 2. Website and Frontend Integrity

VERDICT: NO OFFICIAL SITE | CONFIDENCE: high
Fartcoin has no formal official product website, which is consistent with a community meme that launched on pump.fun, so there is no project frontend to review for address integrity or wallet drainer behavior. The canonical public references are the Dexscreener token page, the pump.fun listing, and the community X account, none of which present a wallet connect or signing flow of their own. Because there is no official site that could embed a wrong address or a drainer script, the main user risk sits entirely with buying the correct mint on a reputable venue rather than with a project webpage. Anyone trading should confirm the official mint 9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump on a trusted explorer before transacting, since meme tokens attract lookalike mints.

---

## 3. Traction and Claims

The traction is genuinely verifiable on chain and through independent aggregators rather than being pure narrative. Circulating supply is about 999.97 million of a 1 billion maximum, and market capitalisation has ranged widely across sources and time, roughly 128 million to 195 million dollars, with the token once reaching a multi billion dollar peak. Reported holders number around 160,000, daily volume runs in the tens of millions, and Raydium pool liquidity sits near 7 to 9 million dollars, all readable via DexScreener, GeckoTerminal, and Solscan. Distribution across major venues such as Coinbase, Gate, HTX, LBank, and DEXes like Raydium and Orca, plus a Binance futures listing, points to real order book depth rather than a single thin wash traded pool. The activity looks organic for a large meme token even though price is driven entirely by sentiment.

---

## 4. Token Utility and Economics

The token confers no utility by design, so its use is speculative only. There is no governance, staking, fee capture, revenue share, or product access tied to holding it, and the playful gas fee fart sound gimmick is cosmetic rather than functional. Holders own a purely speculative meme asset whose worth depends on attention and market mood. Nothing about the token grants a claim on any cash flow or service.

Fartcoin has no formal official product site, which is consistent with a community meme that began on pump.fun. The Dexscreener listing points to an Infinite Backrooms conversation page as its lore source and to the X account FartCoinOfSOL as its main social channel. The public narrative frames the token as an autonomous AI agent meme connected to the Terminal of Truths and GOAT storyline. On chain there is no such agent and no logic at all, only a static token account. We therefore treat the Dexscreener token page as the best canonical link and label the AI agent framing as culture rather than function.

---

## 5. Claim versus Reality

- "An autonomous AI agent meme born from the Terminal of Truths and GOAT lore" / Reality: on chain it is a plain classic SPL token with no program logic, no agent, and no utility
- "Community project with lore links via infinitebackrooms and X" / Reality: there is no formal product website; the canonical references are Dexscreener, the pump.fun page, and the X account FartCoinOfSOL

---

## 6. Contract Security Check (supporting)

- Supply 999,974,888.509394 tokens (raw 999974888509394) at 6 decimals, fixed
- Mint authority null, so supply is renounced and cannot be increased
- Freeze authority null, so no account can be frozen or blacklisted
- Owner program is TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA (classic SPL Token), 82 byte mint, no Token 2022 extensions, no transfer fee, mint suffix pump confirms pump.fun origin

The mint 9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump returns a jsonParsed mint account with decimals 6 and supply 999974888509394, which equals 999,974,888.509394 tokens. The mintAuthority field is null and the freezeAuthority field is null, confirming both powers are renounced. The account owner is the classic SPL Token program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA with a space of 82 bytes, which rules out Token 2022 extensions and any transfer fee. The pump suffix on the mint confirms the pump.fun launch origin, and the token now trades primarily against SOL on Raydium. All of these facts were read directly from the Solana mainnet and cross checked on Dexscreener.

### Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 12 |
| Supply and minting | 6 |
| Liquidity and market | 10 |
| Code safety | 12 |
| Transfer neutrality | 12 |
| Transparency | 3 |
| **Total** | **55/100** |

---

## 7. Findings by Severity

- HIGH: none. MEDIUM: none. LOW: pure meme asset with no utility, revenue, or intrinsic value, so price is driven entirely by speculation and sentiment. INFO: mint authority renounced, freeze authority renounced, fixed supply near 1 billion, classic SPL with no transfer fee, and a transparent pump.fun origin.

---

## 8. Conclusion

Fartcoin is a Solana SPL meme token that originated on pump.fun and its mint 9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump is confirmed live on chain. The mint is owned by the classic SPL Token program with an 82 byte account, so there are no Token 2022 extensions and no transfer fee. Both the mint authority and the freeze authority read null, meaning supply cannot be inflated and holder accounts cannot be frozen. Supply is fixed near 999,974,888.5 tokens at 6 decimals, and the token trades around a market cap of roughly 130 million USD on Raydium. It carries a rich AI agent narrative tied to the Terminal of Truths and GOAT lore, but on chain it is simply a plain meme token with no utility. On the MEFAI whole project scale this token scores 55 out of 100 and is classified Passed.

---

## 9. Verification

- Methodology: product and traction review of the live project, a review of the project website frontend against its stated claims, and onchain analysis using read only public RPC on Solana.
- Sources:
  - `https://api.mainnet-beta.solana.com (getAccountInfo and getTokenSupply, jsonParsed)`
  - `https://dexscreener.com/solana/9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump`
  - `https://solscan.io/token/9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump`
  - `https://www.coingecko.com/en/coins/fartcoin`
  - `https://dexscreener.com/solana/9bb6nfecjbctnnlfko2fqvqbq8hhm13kcyycdqbgpump`
  - `https://www.coinbase.com/price/fartcoin`
  - `https://www.theblock.co/post/357837/coinbase-to-list-the-fartcoin-subsquid-and-pancakeswap-tokens`
  - `https://crypto.news/fartcoin-surges-as-binance-announces-futures-listing/`
  - `https://pump.fun/coin/9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump`

---

*Mefai Security Research. Independent whole project claim versus reality assessment.*
