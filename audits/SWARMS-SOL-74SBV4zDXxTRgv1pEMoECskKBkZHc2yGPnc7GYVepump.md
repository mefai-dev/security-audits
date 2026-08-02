# Security Audit Report: Swarms (SWARMS) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 2, 2026 |
| **Project** | Swarms |
| **Token Symbol** | SWARMS |
| **Contract / Program** | `74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump` |
| **Chain** | Solana |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis (read only public RPC) |
| **Mefai Security Score** | **73/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Flagged** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Claim vs reality audit of Swarms (SWARMS). A Solana multi agent AI framework token whose contract is clean at the mint level with both authorities revoked and no fee, but whose pump.fun origin, near fully circulating micro capitalization, and severe drawdown from a peak above 600 million dollars mark it as a high volatility asset despite a functioning open source product.

Swarms is the SPL token that powers the Swarms multi agent AI orchestration framework built by The Swarm Corporation on Solana. The token was created on pump.fun in December 2024 and grew rapidly to a peak valuation in the hundreds of millions before retracing sharply to a much smaller market cap. The contract itself is clean at the mint level, but the pump.fun origin, thin current market cap, and severe price drawdown mark it as a higher risk, higher volatility asset that warrants caution despite a functioning underlying product.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 1 |
| Medium | 1 |
| Low | 1 |
| Informational | 2 |
| **Total** | **5** |

### Overall Risk Assessment: MEDIUM

MEFAI onchain analysis places Swarms at 73 out of 100 (MEDIUM risk, Flagged).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Swarms / SWARMS |
| **Contract or program** | `74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump` |
| **Chain** | Solana |
| **Tags** | SPL, AI Agent, Pump.fun Origin, Solana, Flagged |

A direct read of the mint returns decimals of six and a raw supply equal to about 999.97 million SWARMS, close to a one billion nominal cap. The mint authority is null, so the supply is permanently fixed, and the freeze authority is null, so individual token accounts cannot be frozen. The account owner is the classic SPL Token program, meaning it is not a Token 2022 mint and carries no built in transfer fee or other extension, so transfers move the full amount with no protocol level tax.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- SWARMS on Solana (mint 74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump, 6 decimals) verified live: supply about 999.97 million, both the mint authority and the freeze authority are null, and it is a classic SPL token with no transfer fee. Supply is effectively fully circulating so market capitalization equals fully diluted value.

---

## 3. Claim versus Reality

- "A fixed supply, community weighted agent token" / Reality: the fixed supply and revoked authorities are confirmed on chain, and near full circulation matches trackers. The specific community allocation and any liquidity or team locks are project stated and not provable from the mint account.

The official documentation presents SWARMS as a fixed supply SPL token with revoked control, and public chain data confirms this precisely, since both the mint authority and the freeze authority read as null and the mint uses the standard SPL Token program with no transfer fee extension. The project also states a near fully circulating, community weighted distribution, and market trackers agree that circulating supply equals total supply, so market cap and fully diluted value are the same. The specific allocation percentages and any liquidity or team locks are project stated figures that could not be independently proven from the mint account.

---

## 4. Findings by Severity

- HIGH: a pump.fun bonding curve origin and a severe drawdown from a peak above 600 million dollars (extreme volatility). MEDIUM: holder concentration and liquidity pool locks were not verifiable on chain; impostor tokens exist. LOW: thin current market capitalization. INFO: both authorities revoked on a standard fee free SPL token and a real open source multi agent product with major listings.

---

## 5. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 14 |
| Supply and minting | 14 |
| Liquidity and market | 9 |
| Code safety | 13 |
| Transfer neutrality | 15 |
| Transparency | 8 |
| **Total** | **73/100** |

---

## 6. Conclusion

Claim vs reality audit of Swarms (SWARMS). A Solana multi agent AI framework token whose contract is clean at the mint level with both authorities revoked and no fee, but whose pump.fun origin, near fully circulating micro capitalization, and severe drawdown from a peak above 600 million dollars mark it as a high volatility asset despite a functioning open source product. On the MEFAI scale this token scores 73 out of 100 and is classified Flagged.

---

## 7. Verification

- Methodology: manual review plus onchain analysis using read only public RPC on Solana.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `solana RPC`
  - `docs.swarms.world/web3/token`
  - `coinmarketcap swarms`
  - `solanacompass`
  - `github The-Swarm-Corporation`
  - `opensea/solana.fm mint.`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
