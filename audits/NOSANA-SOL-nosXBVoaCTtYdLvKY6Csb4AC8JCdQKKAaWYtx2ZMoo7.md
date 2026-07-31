# Security Audit Report: Nosana (NOS) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Nosana |
| **Token Symbol** | NOS |
| **Mint (Solana)** | `nosXBVoaCTtYdLvKY6Csb4AC8JCdQKKAaWYtx2ZMoo7` |
| **Chain** | Solana |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **60/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Nosana is a real Solana based decentralized GPU compute project with a clean, hard capped token contract. It is not a rug at the contract level. The MEDIUM rating reflects a large gap between the marketed scale and the actual on chain reality, plus thin liquidity and insider concentration:

1. **The token contract is clean:** MEFAI confirms the mint and freeze authorities are both revoked, supply is hard capped near 100 million and fully circulating, so there is no mint based inflation, no freeze capability and no hidden vesting cliff.
2. **But the marketed scale is inconsistent and hard to verify.** The project's own site cites on the order of a couple of thousand nodes, while larger host counts and inference figures cited on third party listings cannot be independently sourced, and the on chain footprint is dominated by trading of the token rather than verifiable compute jobs.
3. **Rewards are incentive driven,** and on chain liquidity is thin relative to the valuation.
4. **The genesis allocation is insider heavy** (roughly 62 percent to company, team and backers), now fully liquid, and the token is down roughly 97 percent from peak; the project also carries a prior pivot from a continuous integration product to GPU compute.

The contract is clean; the caution is inflated scale claims, thin liquidity, insider concentration and a prior pivot.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Mint (Solana)** | `nosXBVoaCTtYdLvKY6Csb4AC8JCdQKKAaWYtx2ZMoo7` |
| **Decimals** | 6 |
| **Supply** | ~99.999 million of a fixed 100 million cap, effectively fully circulating |
| **Mint authority** | Revoked |
| **Freeze authority** | Revoked |
| **Liquidity** | Thin: ~430 thousand dollars in the primary pool (~640 thousand total across pools), roughly 2 to 3 percent of market capitalization |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana read of the NOS mint returned a clean contract profile:

| Check | Result |
|-------|--------|
| Mint identity | NOS (Solana SPL, standard token program), 6 decimals, verified |
| Supply | ~99.999 million of a fixed 100 million cap, effectively fully circulating (no locked vesting remaining) |
| Mint authority | **Revoked** (no new tokens can ever be minted) |
| Freeze authority | **Revoked** (accounts cannot be frozen) |
| Rewards model | Host and staking rewards paid from a pre allocated supply bucket, not from new minting |
| Liquidity | Thin (~430 thousand dollars primary pool, ~640 thousand total) relative to the valuation |

**Interpretation.** On pure token mechanics, NOS is clean: revoked mint and freeze authorities, a hard cap, and no remaining vesting cliff. The risks are not in the contract; they are in the marketed scale versus reality, the thin liquidity and the insider heavy genesis allocation.

---

## 3. Claim vs Reality: "GPU Compute Cloud at Scale for AI Inference"

> Site: an affordable, on demand, decentralized GPU compute cloud optimized for AI inference, with tens of thousands of GPU hosts and very large inference throughput.

**Reality: real network, but scale claims are inconsistent and hard to verify.** The project's own site cites on the order of a couple of thousand nodes, while larger host counts and inference figures cited on third party listings cannot be independently sourced, so the marketed scale is inconsistent and unverifiable. MEFAI's read of the on chain footprint shows thousands of transactions per day, but these are dominated by decentralized exchange and arbitrage trading of the token rather than verifiable compute jobs, so on chain settlement is not evidence of the advertised inference volume. Node onboarding is genuinely permissionless, a positive, but the verifiable compute activity is hard to substantiate at the marketed scale.

---

## 4. Claim vs Reality: Economics, Concentration and History

- **Incentive driven rewards:** daily volume is low relative to the market capitalization and primary on chain liquidity is thin (~430 thousand dollars, ~640 thousand across pools); host activity is driven by reward emissions from a pre allocated supply bucket rather than clearly demonstrated external compute revenue, closer to an incentivized network than a proven self sustaining marketplace.
- **Insider heavy allocation:** the genesis allocation was insider heavy (company ~25 percent plus team and shareholders ~22.5 percent, with the treasury a further ~18 percent and only a small public sale), and all allocations are now fully liquid, so latent concentration and sell pressure are high (an exact top holder figure was not retrievable on chain during this review).
- **No mint inflation (positive):** because the mint authority is revoked, "emissions" are transfers from a pre allocated bucket, not new supply, so there is no dilution by minting.
- **Prior pivot:** the project launched around 2021 to 2022 as a decentralized continuous integration pipeline before pivoting to GPU and AI compute, and its own mainnet dating is inconsistent across surfaces.
- **Value and liquidity:** the token is down roughly 97 percent from its 2024 peak, and the thin on chain liquidity (roughly 2 to 3 percent of market capitalization) creates meaningful slippage and manipulation exposure.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| NOS 001 | **MEDIUM** | Marketed GPU host and inference scale is inconsistent and hard to verify; on chain activity is dominated by trading rather than verifiable compute jobs. |
| NOS 002 | **MEDIUM** | Thin on chain liquidity (~430 thousand dollars primary, ~640 thousand total, roughly 2 to 3 percent of market capitalization) creates slippage and manipulation exposure. |
| NOS 003 | **MEDIUM** | Insider heavy genesis allocation (company ~25 percent, team ~22.5 percent, treasury ~18 percent), now fully liquid; rewards are incentive driven. |
| NOS 004 | **LOW** | ~97 percent drawdown from peak; a prior pivot from continuous integration to GPU compute. |
| NOS 005 | **INFO** | Clean token contract: mint and freeze authorities revoked, hard capped, fully circulating, no vesting cliff (positive). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token mechanics | Low risk | Mint and freeze revoked, hard capped |
| Scale claims | Medium risk | Inconsistent, hard to verify host and inference figures |
| Liquidity | Medium to high risk | Thin (~430 thousand primary, ~640 thousand total) |
| Concentration | Medium to high risk | Insider heavy genesis, fully liquid |
| Product reality | Medium risk | Real but incentive driven, scale hard to verify |
| Value / volatility | High risk | ~97 percent drawdown |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `nosXBVoaCTtYdLvKY6Csb4AC8JCdQKKAaWYtx2ZMoo7` |
| Decimals | 6 |
| Supply | ~99.999 million (fixed 100 million cap, fully circulating) |
| Mint / freeze authority | Revoked / Revoked |
| Liquidity | ~430 thousand dollars primary (~640 thousand total) |

---

## 8. Conclusion

Nosana is a real Solana based decentralized GPU compute project with genuinely clean token mechanics (mint and freeze revoked, hard capped, fully circulating), which keeps it in the MEDIUM band at 60/100. It is held back because the marketed scale is inconsistent and hard to verify (the site cites a couple of thousand nodes while larger figures cannot be sourced, and on chain activity is dominated by trading rather than compute jobs), because on chain liquidity is thin relative to the valuation and rewards are incentive driven, and because the genesis allocation is insider heavy and now fully liquid, against a roughly 97 percent drawdown and a prior pivot. The contract is clean; the caution is hard to verify scale, thin liquidity and insider concentration.

---

## 9. Recommendations

**For the Nosana team:**
- Report active (not cumulative) GPU hosts and real utilization, and remove unverifiable "billions of daily inference" directory text.
- Deepen on chain liquidity and publish verifiable paying demand versus incentivized activity.
- Disclose the insider allocation and the product history transparently.

**For users:**
- Note the clean contract does not offset thin liquidity, insider concentration and hard to verify scale claims.
- Treat host and inference figures cautiously, and model the liquidity and drawdown risk.

---

## 10. Verification

- MEFAI on chain analysis: a direct Solana read of the NOS mint (identity, 6 decimals, ~99.999 million of a 100 million cap, mint and freeze authorities revoked) and the primary pool liquidity and on chain activity footprint.
- The mint address, supply and authorities are publicly verifiable by anyone on the Solana explorers.
- Project statements: the project's own pages (the GPU compute cloud and scale claims), and the public record of the allocation, the drawdown and the prior continuous integration to GPU pivot.
