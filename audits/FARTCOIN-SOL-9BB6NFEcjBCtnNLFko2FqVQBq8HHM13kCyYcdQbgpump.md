# Security Audit Report: Fartcoin (FARTCOIN) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 3, 2026 |
| **Project** | Fartcoin |
| **Token Symbol** | FARTCOIN |
| **Contract / Program** | `9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump` |
| **Chain** | Solana |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **72/100** |
| **Overall Risk** | **LOW** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Fartcoin is a Solana SPL meme token that originated on pump.fun and its mint 9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump is confirmed live on chain. The mint is owned by the classic SPL Token program with an 82 byte account, so there are no Token 2022 extensions and no transfer fee. Both the mint authority and the freeze authority read null, meaning supply cannot be inflated and holder accounts cannot be frozen. Supply is fixed near 999,974,888.5 tokens at 6 decimals, and the token trades around a market cap of roughly 130 million USD on Raydium. It carries a rich AI agent narrative tied to the Terminal of Truths and GOAT lore, but on chain it is simply a plain meme token with no utility.

Fartcoin is a Solana meme token whose mint address was verified live against a public RPC and matches the supplied value. The mint sits under the classic SPL Token program with a standard 82 byte account, confirming it is not a Token 2022 mint and carries no transfer fee. Both authorities are null, so the supply of about 999.97 million cannot grow and no wallet can be frozen. The market values it near 130 million USD, but that value rests on meme demand rather than any product. From a security standpoint the contract is clean, so the recommendation is PASS with the caveat that this is a speculative meme.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 1 |
| Informational | 4 |
| **Total** | **5** |

### Overall Risk Assessment: LOW

MEFAI onchain analysis places Fartcoin at 72 out of 100 (LOW risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Fartcoin / FARTCOIN |
| **Contract or program** | `9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump` |
| **Chain** | Solana |
| **Tags** | SPL, AI Agent Meme, Solana, Passed |

The mint 9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump returns a jsonParsed mint account with decimals 6 and supply 999974888509394, which equals 999,974,888.509394 tokens. The mintAuthority field is null and the freezeAuthority field is null, confirming both powers are renounced. The account owner is the classic SPL Token program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA with a space of 82 bytes, which rules out Token 2022 extensions and any transfer fee. The pump suffix on the mint confirms the pump.fun launch origin, and the token now trades primarily against SOL on Raydium. All of these facts were read directly from the Solana mainnet and cross checked on Dexscreener.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- Supply 999,974,888.509394 tokens (raw 999974888509394) at 6 decimals, fixed
- Mint authority null, so supply is renounced and cannot be increased
- Freeze authority null, so no account can be frozen or blacklisted
- Owner program is TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA (classic SPL Token), 82 byte mint, no Token 2022 extensions, no transfer fee, mint suffix pump confirms pump.fun origin

---

## 3. Claim versus Reality

- "An autonomous AI agent meme born from the Terminal of Truths and GOAT lore" / Reality: on chain it is a plain classic SPL token with no program logic, no agent, and no utility
- "Community project with lore links via infinitebackrooms and X" / Reality: there is no formal product website; the canonical references are Dexscreener, the pump.fun page, and the X account FartCoinOfSOL

Fartcoin has no formal official product site, which is consistent with a community meme that began on pump.fun. The Dexscreener listing points to an Infinite Backrooms conversation page as its lore source and to the X account FartCoinOfSOL as its main social channel. The public narrative frames the token as an autonomous AI agent meme connected to the Terminal of Truths and GOAT storyline. On chain there is no such agent and no logic at all, only a static token account. We therefore treat the Dexscreener token page as the best canonical link and label the AI agent framing as culture rather than function.

---

## 4. Website and Frontend Integrity

VERDICT: NO OFFICIAL SITE | CONFIDENCE: high
Fartcoin has no formal official product website, which is consistent with a community meme that launched on pump.fun, so there is no project frontend to review for address integrity or wallet drainer behavior. The canonical public references are the Dexscreener token page, the pump.fun listing, and the community X account, none of which present a wallet connect or signing flow of their own. Because there is no official site that could embed a wrong address or a drainer script, the main user risk sits entirely with buying the correct mint on a reputable venue rather than with a project webpage. Anyone trading should confirm the official mint 9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump on a trusted explorer before transacting, since meme tokens attract lookalike mints.


---

## 5. Findings by Severity

- HIGH: none. MEDIUM: none. LOW: pure meme asset with no utility, revenue, or intrinsic value, so price is driven entirely by speculation and sentiment. INFO: mint authority renounced, freeze authority renounced, fixed supply near 1 billion, classic SPL with no transfer fee, and a transparent pump.fun origin.

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 14 |
| Supply and minting | 14 |
| Liquidity and market | 10 |
| Code safety | 13 |
| Transfer neutrality | 15 |
| Transparency | 6 |
| **Total** | **72/100** |

---

## 7. Conclusion

Fartcoin is a Solana SPL meme token that originated on pump.fun and its mint 9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump is confirmed live on chain. The mint is owned by the classic SPL Token program with an 82 byte account, so there are no Token 2022 extensions and no transfer fee. Both the mint authority and the freeze authority read null, meaning supply cannot be inflated and holder accounts cannot be frozen. Supply is fixed near 999,974,888.5 tokens at 6 decimals, and the token trades around a market cap of roughly 130 million USD on Raydium. It carries a rich AI agent narrative tied to the Terminal of Truths and GOAT lore, but on chain it is simply a plain meme token with no utility. On the MEFAI scale this token scores 72 out of 100 and is classified Passed.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Solana, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `https://api.mainnet-beta.solana.com (getAccountInfo and getTokenSupply, jsonParsed)`
  - `https://dexscreener.com/solana/9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump`
  - `https://solscan.io/token/9BB6NFEcjBCtnNLFko2FqVQBq8HHM13kCyYcdQbgpump`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
