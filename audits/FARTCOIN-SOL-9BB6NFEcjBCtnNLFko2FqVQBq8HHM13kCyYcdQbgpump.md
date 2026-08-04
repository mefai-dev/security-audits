# Security Audit Report: Fartcoin (FARTCOIN) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Fartcoin |
| **Token Symbol** | FARTCOIN |
| **Mint (Solana)** | `9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump` |
| **Chain** | Solana (SPL token, classic Token program, pump.fun origin) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **55/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Passed** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and community channels, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Fartcoin markets itself through a rich autonomous AI agent narrative tied to the Terminal of Truths and the GOAT lore, but the audit finds that this story is culture rather than function. The token contract itself is clean and the market is genuinely real and liquid, yet there is no product and no token utility by design, so value rests entirely on speculation and sentiment.

1. **The contract is clean.** MEFAI's direct read of the mint on Solana confirms a plain classic SPL token with both the mint authority and the freeze authority renounced (null), a fixed supply near 1 billion at 6 decimals, an 82 byte mint account with no Token 2022 extensions, and no transfer fee. Supply cannot be inflated and no holder account can be frozen.
2. **The market is real and liquid.** Traction is verifiable on chain and through independent aggregators: roughly 160,000 holders, a market capitalisation in the range of about 130 million to 195 million USD across sources and time, daily volume in the tens of millions, Raydium pool liquidity near 7 to 9 million dollars, and genuine listings across major venues including Coinbase, Gate, HTX, LBank, Raydium, and Orca, plus a Binance futures listing. This is real order book depth, not a single thin wash traded pool.
3. **There is no product and no utility.** Fartcoin has no application, protocol, service, roadmap, or promised utility, and that absence is intentional. There is no governance, staking, fee capture, revenue share, or product access tied to holding it. Judged as what it claims to be, a meme, it is honest about having nothing to build.
4. **Value is pure speculation.** Price is driven entirely by attention and market mood. It has been highly volatile, falling from an all time high near 2.48 dollars to roughly 0.13 to 0.20 dollars, and the narrative depends on continued interest in the Terminal of Truths and GOAT meme lore.

The contract is not a scam and the market is genuine, but the AI agent framing describes lore, not an onchain agent, and holders own a purely speculative asset with no intrinsic cash flow. Weighing a clean contract and real liquidity against the complete absence of utility and the sentiment driven volatility, Fartcoin lands at 55 out of 100, Passed.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Fartcoin / FARTCOIN |
| **Mint (Solana)** | `9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump` |
| **Decimals** | 6 |
| **Supply** | 999,974,888.509394 FARTCOIN (raw 999974888509394), fixed |
| **Origin** | Fair launch on pump.fun in October 2024; the `pump` mint suffix confirms the pump.fun launch origin |
| **Contract controls** | Mint authority null (renounced) and freeze authority null (renounced); owned by the classic SPL Token program, no upgradeable token logic |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the FARTCOIN mint on Solana mainnet returned:

| Check | Result |
|-------|--------|
| Token identity | FARTCOIN, 6 decimals, verified live against a public RPC |
| Supply | 999,974,888.509394 (raw 999974888509394), fixed |
| Mint authority | Null, renounced, supply cannot be increased |
| Freeze authority | Null, renounced, no account can be frozen or blacklisted |
| Owner program | `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (classic SPL Token), 82 byte mint |
| Token 2022 extensions | None |
| Transfer fee | None |

**Interpretation.** At the contract level FARTCOIN is clean. It is a standard classic SPL token with a fixed supply, no Token 2022 extensions, and no transfer fee, and both privileged authorities are renounced. Because the mint authority is null, no one can dilute holders by printing more tokens, and because the freeze authority is null, no one can freeze or blacklist a wallet. There is no proxy or upgradeable program logic controlling the token. The residual risk is not a contract exploit path, it is that the asset carries no utility, so the real considerations for this project sit at the product, value, and market level below, not in the token contract.

---

## 3. Claim vs Reality: "Autonomous AI Agent Meme"

> Narrative: Fartcoin is presented as an autonomous AI agent meme born from the Terminal of Truths and the GOAT lore, an AI native token with a mind of its own.

**Reality: on chain it is a plain classic SPL token with no logic.** MEFAI's read of the mint shows a static token account under the classic SPL Token program, with no program logic, no agent, and no onchain behaviour of any kind. The Terminal of Truths and GOAT storyline supplied cultural attention and a meme identity, not working software. The AI agent framing is genuine culture and community lore, but it is not a function of the token, and a security minded reader should treat it as narrative rather than as an onchain capability.

---

## 4. Claim vs Reality: Token Value and Utility

> Narrative: FARTCOIN is framed as a token worth holding, riding an AI meme movement with real momentum.

**Reality: no utility by design, so value is speculation only.** Fartcoin confers no governance, staking, fee capture, revenue share, or product access, and the playful gas fee fart sound gimmick is cosmetic rather than functional. Holders own a purely speculative meme asset whose worth depends on attention and market mood. Nothing about the token grants a claim on any cash flow or service. This is not hidden or misrepresented, the project is honest that it is a meme, but it does mean the price carries the full volatility of sentiment, having fallen from an all time high near 2.48 dollars to roughly 0.13 to 0.20 dollars.

---

## 5. Claim vs Reality: Where the Project Lives

> Narrative: Fartcoin is presented as a community project with lore links via infinitebackrooms and an active X presence.

**Reality: no formal product website, only community references.** There is no official product site, which is consistent with a community meme that launched on pump.fun. The canonical public references are the Dexscreener token page, the pump.fun listing, and the community X account FartCoinOfSOL. Because there is no official site that could embed a wrong address or a wallet drainer script, there is no project frontend to compromise, and the main user risk shifts to buying the correct mint on a reputable venue. Meme tokens attract lookalike mints, so users should confirm the official mint `9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump` on a trusted explorer before transacting.

---

## 6. Positive Findings (Credited)

- The token contract is clean: a classic SPL token with a fixed supply, no Token 2022 extensions, and no transfer fee.
- Both privileged authorities are renounced. The mint authority is null, so supply cannot be inflated, and the freeze authority is null, so no wallet can be frozen or blacklisted.
- Traction is genuinely verifiable, not narrative only: roughly 160,000 holders, market capitalisation around 130 million to 195 million USD, daily volume in the tens of millions, and Raydium liquidity near 7 to 9 million dollars.
- Distribution is broad and real, spanning Coinbase, Gate, HTX, LBank, Raydium, and Orca, plus a Binance futures listing, which points to real order book depth rather than a single thin pool.
- The origin is transparent. The `pump` mint suffix confirms a pump.fun fair launch, and the project is honest that it is a meme with nothing to build.

---

## 7. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| FARTCOIN 001 | **MEDIUM** | No product and no token utility by design; value is pure speculation and sentiment with no governance, staking, revenue, or product access. |
| FARTCOIN 002 | **MEDIUM** | High price volatility driven by attention; the token fell from an all time high near 2.48 dollars to roughly 0.13 to 0.20 dollars, and the narrative depends on continued interest in the AI meme lore. |
| FARTCOIN 003 | **LOW** | No formal official site; canonical references are Dexscreener, pump.fun, and X, so users must confirm the correct mint to avoid lookalike mints. |
| FARTCOIN 004 | **INFO** | Mint authority renounced (null); supply is fixed near 1 billion and cannot be inflated (positive). |
| FARTCOIN 005 | **INFO** | Freeze authority renounced (null); no account can be frozen or blacklisted (positive). |
| FARTCOIN 006 | **INFO** | Classic SPL token, 82 byte mint, no Token 2022 extensions, no transfer fee (positive). |
| FARTCOIN 007 | **INFO** | Real, verifiable traction: about 160,000 holders, market cap roughly 130 million to 195 million USD, and genuine CEX and DEX listings (positive). |

---

## 8. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified classic SPL, fixed supply, no transfer fee |
| Supply / minting | Low risk | Mint authority renounced, supply fixed near 1 billion |
| Account control | Low risk | Freeze authority renounced, no freeze or blacklist power |
| Product / utility | Medium risk | No product and no utility by design, value is speculation only |
| Market / volatility | Medium risk | Sentiment driven price, sharp drawdown from the all time high |
| Traction | Low risk | Real holders, liquidity, and CEX and DEX listings, verifiable on chain |
| Transparency | Low risk | Transparent pump.fun origin, honest meme framing |

---

## 9. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump` |
| Decimals | 6 |
| Supply | 999,974,888.509394 FARTCOIN (raw 999974888509394), fixed |
| Owner program | `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (classic SPL Token) |
| Mint account size | 82 bytes (no Token 2022 extensions) |
| Mint authority | Null (renounced) |
| Freeze authority | Null (renounced) |
| Transfer fee | None |
| Origin | pump.fun fair launch (mint suffix `pump`) |
| Primary market | Raydium (against SOL), plus broad CEX and DEX distribution |

---

## 10. Conclusion

Fartcoin is a clean contract wrapped around a pure meme. The mint is a plain classic SPL token with both authorities renounced, a fixed supply near 1 billion, no Token 2022 extensions, and no transfer fee, which keeps its contract level risk low. The market around it is genuinely real and liquid, with roughly 160,000 holders, a market cap in the 130 million to 195 million USD range, and real CEX and DEX listings, so the traction is not a mirage. The caution is that there is no product and no token utility by design. The autonomous AI agent narrative tied to the Terminal of Truths and GOAT is culture, not an onchain function, and value therefore rests entirely on speculation and sentiment, which has produced sharp volatility from an all time high near 2.48 dollars down to roughly 0.13 to 0.20 dollars. Weighing a clean, liquid, transparent meme against the complete absence of utility and the sentiment driven risk, Fartcoin scores 55 out of 100 and is Passed. The verdict reflects a token that is honest about what it is, not one that hides a defect.

---

## 11. Recommendations

**For the Fartcoin community:**
- Keep presenting the AI agent story as lore rather than implying an onchain agent or product that does not exist.
- Maintain and clearly signpost the canonical references (the correct mint, Dexscreener, pump.fun, and the official X account) so buyers can avoid lookalike mints.
- Continue to publish the transparent origin and the renounced authorities, which are the strongest honest signals here.

**For users:**
- Understand that FARTCOIN has no utility, no cash flow, and no product; its price is driven entirely by attention and market mood and has been highly volatile.
- Treat the AI agent narrative as culture, not as an onchain capability, and size any exposure as pure speculation.
- Verify the official mint `9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump` on a trusted explorer before buying, since meme tokens attract lookalike mints, and trade only on reputable venues.

---

## 12. Verification

- MEFAI onchain analysis: a direct Solana mainnet read of the FARTCOIN mint (identity, 6 decimals, fixed supply 999974888509394, mint authority null, freeze authority null, owner program the classic SPL Token program, 82 byte mint with no Token 2022 extensions, no transfer fee), cross checked on Dexscreener and Solscan.
- Traction checks: holder count, market capitalisation range, daily volume, and Raydium liquidity via DexScreener, GeckoTerminal, and Solscan, plus confirmation of CEX and DEX listings including Coinbase, Gate, HTX, LBank, Raydium, Orca, and a Binance futures listing.
- Project reality: the pump.fun fair launch origin, the absence of a formal product website, and the community narrative around the Terminal of Truths and GOAT lore, treated as culture rather than onchain function.
- Sources: `https://api.mainnet-beta.solana.com` (getAccountInfo and getTokenSupply, jsonParsed); `https://dexscreener.com/solana/9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump`; `https://solscan.io/token/9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump`; `https://www.coingecko.com/en/coins/fartcoin`; `https://www.coinbase.com/price/fartcoin`; `https://pump.fun/coin/9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump`.
