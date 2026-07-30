# Security Audit Report: GT Protocol (GTAI) - BNB Smart Chain

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | GT Protocol |
| **Token Symbol** | GTAI |
| **Contract** | `0x003d87d02a2a01e9e8a20f507c83e15dd83a33d1` |
| **Chain** | BNB Smart Chain (BSC) |
| **Audit Type** | Token + Project (Claim vs Reality) |
| **Mefai Security Score** | **58/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on-chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

GT Protocol is a real, listed AI-trading-assistant project whose token trades and is not a honeypot. It is not a rug at the mechanics level, but the MEDIUM rating reflects several control and liquidity risks that a trader should weigh:

1. **Ownership is NOT renounced.** A single externally owned account retains the owner role. MEFAI's read of the contract shows the owner's powers are limited (no arbitrary mint, no tax switch to trap sells), but centralized control still exists.
2. **Liquidity is effectively NOT locked.** MEFAI found that roughly **99.89 percent of the LP tokens sit in an externally owned wallet**, not a locker or a burn address, so liquidity could be pulled. The main on-chain pool is thin (roughly 33 thousand dollars).
3. The "autonomous AI executes your trades non-custodially" claim is partly real and partly overstated: execution bots are real, but the "intelligence" leans on a **third-party commercial model provider** rather than proprietary on-chain AI.

The token is genuinely traded and functional; the caution is retained owner control, unlocked and thin liquidity, and an overstated AI-autonomy claim.

---

## 1. Contract Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | GT Protocol / GTAI |
| **Contract** | `0x003d87d02a2a01e9e8a20f507c83e15dd83a33d1` |
| **Decimals** | 18 |
| **Total supply** | 75,000,000 (fixed) |
| **Owner** | Single EOA `0x7c520c275eb5186e5c05fecf2bc3762320288357` (NOT renounced) |
| **Verified source** | Yes |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI's on-chain contract scan returned:

| Check | Result |
|-------|--------|
| Honeypot | No (sells clear) |
| Buy tax | 0 percent |
| Sell tax | 0 percent |
| Mint function | No arbitrary mint (fixed 75M supply) |
| Ownership | **NOT renounced** (single EOA owner) |
| LP status | **~99.89 percent of LP in an EOA** (not locked, not burned) |
| Source verified | Yes |

**Interpretation.** The token itself will not trap a sell (0/0 tax, not a honeypot, fixed supply). The two material risks are (a) the owner role is retained by a single wallet, and (b) liquidity is not locked, sitting almost entirely in an EOA that could withdraw it.

---

## 3. Critical Risk: Unlocked, Thin Liquidity

MEFAI's LP analysis found that approximately **99.89 percent of the liquidity-pool tokens are held in an externally owned account**, not in a time-lock contract and not burned. This means the liquidity can be removed at the holder's discretion, a classic rug vector even when the token contract is otherwise clean.

Compounding this, the main on-chain pool is **thin (roughly 33 thousand dollars)**. Combined with a microcap valuation (roughly 470 thousand dollars), any meaningful on-chain sell would face heavy slippage even before considering the unlocked-LP risk.

---

## 4. Holder Concentration

- MEFAI's holder read shows the **top 10 wallets hold roughly 54.4 percent** of supply.
- Concentration at this level means a small number of wallets can materially move price.

---

## 5. Claim vs Reality: "Non-custodial Autonomous AI Trading"

> Marketing: an "AI" that autonomously executes trades for users "non-custodially".

**Reality: partly true, partly overstated.** The execution layer (bots that place trades via exchange API keys) is real, and the model is largely non-custodial in that users connect their own exchange or wallet credentials. However:
- The "intelligence" is not proprietary on-chain AI; it leans on a **third-party commercial model provider** wrapped by the platform.
- "Autonomous" overstates a rules-plus-model execution engine. It is an automation tool with an AI layer, not an on-chain autonomous agent.

The product is real; the "autonomous AI" framing is marketing gloss over a conventional trading-automation service.

---

## 6. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| GTAI-001 | **HIGH** | ~99.89 percent of LP held in an EOA (not locked/burned): liquidity can be pulled. |
| GTAI-002 | **MEDIUM** | Ownership NOT renounced (single EOA owner). |
| GTAI-003 | **MEDIUM** | Thin on-chain liquidity (~33k dollars) and microcap (~470k dollars): heavy slippage on exit. |
| GTAI-004 | **MEDIUM** | Top 10 wallets hold ~54.4 percent of supply. |
| GTAI-005 | **LOW** | "Autonomous AI" overstates a trading-automation tool built on a third-party model provider. |
| GTAI-006 | **INFO** | Token is not a honeypot, 0/0 tax, fixed supply, verified source (positive). |

---

## 7. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Ownership control | Medium risk | Not renounced, single EOA owner |
| Supply / minting | Low risk | Fixed 75M, no arbitrary mint |
| Liquidity security | High risk | ~99.89 percent LP in EOA, unlocked |
| Concentration | Medium to high risk | Top 10 ~54.4 percent |
| Decentralization | Medium risk | Third-party model provider, bot execution |
| Transparency | Medium risk | "Autonomous AI" overstated |

---

## 8. Technical Specifications

| Item | Value |
|------|-------|
| Contract | `0x003d87d02a2a01e9e8a20f507c83e15dd83a33d1` |
| Decimals / supply | 18 / 75,000,000 (fixed) |
| Owner | `0x7c520c275eb5186e5c05fecf2bc3762320288357` (not renounced) |
| Tax | 0 / 0 (not a honeypot) |
| LP | ~99.89 percent in an EOA (unlocked) |
| Main pool | ~33k dollars |
| Top 10 holders | ~54.4 percent |

---

## 9. Conclusion

GT Protocol is a real, functioning AI-trading-assistant token that will not trap a sell at the contract level (0/0 tax, not a honeypot, fixed supply). It is held back to a MEDIUM (58/100) by retained owner control, and above all by liquidity that is effectively unlocked, roughly 99.89 percent of LP tokens sit in an externally owned wallet, on top of a thin pool and microcap size. The "autonomous AI" claim is marketing over a conventional trading-automation service built on a third-party model provider. Not a scam, but the unlocked-LP and concentration risks are real.

---

## 10. Recommendations

**For the GT Protocol team:**
- Lock or burn the liquidity; ~99.89 percent of LP in an EOA is the single biggest trust gap.
- Renounce ownership or move it to a timelock/multisig and disclose the owner's exact powers.
- Describe the product accurately as trading automation with an AI layer built on a third-party model provider.

**For users:**
- Treat the unlocked LP as a rug vector; liquidity can be pulled at the holder's discretion.
- Size positions for a thin (~33k dollar) pool and ~54.4 percent top-10 concentration.

---

## 11. Verification

- MEFAI on-chain analysis: reads of the GTAI contract (owner, supply, tax, honeypot status), an LP-holder analysis (the ~99.89 percent EOA concentration), pool-size and top-holder reads.
- The contract address, its owner, the LP holder distribution and the pool balance are publicly verifiable by anyone on the BNB Smart Chain explorers.
- Project statements: the project's website and documentation describing the trading-automation product.
