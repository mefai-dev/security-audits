# Security Audit Report: Vana (VANA) on Vana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 1, 2026 |
| **Project** | Vana |
| **Token Symbol** | VANA |
| **Native token** | VANA (Vana Layer 1, chain id 1480); Ethereum omnichain form `0x7ff7fa94b8b66ef313f7970d4eebd2cb3103a2c0` |
| **Chain** | Vana (and Ethereum, Base) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **58/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Vana is a legitimate, top tier venture backed Layer 1 for user owned data with a genuinely capped, non inflationary token. It is not a scam. The MEDIUM rating reflects a low float, unlock heavy, permissioned reality behind the "open, user owned data for AI" branding:

1. **The supply is clean and capped:** MEFAI confirms a fixed 120 million maximum with no discretionary minting and no inflationary emission, so there is no hidden emission risk.
2. **But the structure is low float and unlock heavy.** Only about a quarter of supply is circulating, roughly a third is held by the team and investors, and those cliffs are unlocking on multi year schedules into 2029, a classic low float and high fully diluted valuation overhang.
3. **The validator set is permissioned,** whitelisted and undisclosed in count, with early phase validator rewards redirected to a public goods fund, so decentralization is early.
4. **DataDAO usage is thin** versus the "data liquidity for AI" framing, and impostor tokens exist on other chains.

The supply and backing are real; the caution is a heavy unlock overhang, a permissioned validator set, thin usage and impostor token risk.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token** | VANA (native gas on the Vana Layer 1, chain id 1480) |
| **Ethereum omnichain form** | `0x7ff7fa94b8b66ef313f7970d4eebd2cb3103a2c0` (a LayerZero omnichain token, 18 decimals) |
| **Decimals** | 18 |
| **Max supply** | 120,000,000 VANA (fixed cap, no discretionary minting, non inflationary) |
| **Circulating** | ~30.8 million (~25.7 percent); fully diluted valuation roughly four times market capitalization |
| **Insider allocation** | Team ~18.8 percent and investors ~14.3 percent (both 0 percent at launch), unlocking on cliffs into 2029 |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified VANA:

| Check | Result |
|-------|--------|
| Native chain | Vana Layer 1, EVM compatible, chain id 1480, verified |
| Ethereum omnichain form | `0x7ff7fa94b8b66ef313f7970d4eebd2cb3103a2c0`, 18 decimals (a LayerZero omnichain token) |
| Max supply | 120,000,000 VANA (fixed cap, no discretionary minting, non inflationary) |
| Circulating | ~30.8 million (~25.7 percent), low float with a high fully diluted valuation |
| Validators | Permissioned: whitelisted (approval plus a stake requirement), count not publicly disclosed; early phase rewards to a public goods fund |
| Impostor risk | Fake VANA tokens exist on Ethereum (for example one reporting a 1 billion supply); only the official omnichain token is genuine |

**Interpretation.** VANA has a genuinely capped, non inflationary 120 million supply, so there is no hidden emission risk, a real positive. The concerns are a low circulating float with heavy insider unlocks flowing into 2029, a permissioned and undisclosed validator set, and impostor tokens on other chains that holders must avoid.

---

## 3. Claim vs Reality: "Open, User Owned Data for AI"

> Site: open data infrastructure for human grounded AI, DataDAOs where users pool, own, govern and monetize their data for AI training, and data liquidity for AI.

**Reality: partly substantive, largely aspirational, and permissioned.** Real data liquidity pools, a proof of contribution mechanism and some live data activity exist, but the volumes are modest for an AI training network and on chain utilization is very low, so real enterprise AI training demand for this data is unproven. The "open and permissionless" framing collides with a permissioned validator set that is whitelisted with an undisclosed count and early phase rewards redirected to a public goods fund, so the network is meaningfully centralized and early stage rather than a permissionless validator set.

---

## 4. Claim vs Reality: Float, Unlocks and Value

- **Low float and high fully diluted valuation:** only about 25.7 percent circulating with a fully diluted valuation roughly four times the market capitalization, the classic low float and high valuation structure.
- **Heavy insider unlock overhang:** team and investors hold roughly a third of supply, were at zero percent at launch, and are unlocking on cliffs into 2029, a persistent sell side overhang.
- **Supply integrity (positive):** the 120 million cap is genuinely fixed with no discretionary minting, so the overhang is unlock driven, not emission driven.
- **Impostor risk:** multiple fake VANA tokens exist on Ethereum; holders must use only the official omnichain token.
- **Value:** the token is down roughly 97 percent from its launch day peak, near its all time low.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| VANA 001 | **MEDIUM** | Low float (~25.7 percent) with roughly a third to insiders unlocking on cliffs into 2029 and a high fully diluted valuation (persistent overhang). |
| VANA 002 | **MEDIUM** | Permissioned validator set (whitelisted, undisclosed count) with early phase rewards to a public goods fund, against the open and permissionless framing. |
| VANA 003 | **LOW** | DataDAO usage is thin versus the data liquidity for AI framing; ~97 percent drawdown near all time lows. |
| VANA 004 | **LOW** | Impostor VANA tokens exist on other chains; only the official omnichain token is genuine. |
| VANA 005 | **INFO** | Genuinely capped 120 million supply with no discretionary minting or inflationary emission, and top tier venture backing (positives). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, capped, well backed |
| Supply / minting | Low risk | Fixed cap, no discretionary minting |
| Supply / unlocks | Medium to high risk | Low float, insider cliffs into 2029 |
| Decentralization | Medium risk | Permissioned, undisclosed validators |
| Usage reality | Medium risk | Thin DataDAO usage vs framing |
| Value / volatility | High risk | ~97 percent drawdown near all time lows |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Native chain | Vana Layer 1, chain id 1480, EVM compatible |
| Ethereum omnichain form | `0x7ff7fa94b8b66ef313f7970d4eebd2cb3103a2c0` |
| Decimals | 18 |
| Max supply | 120,000,000 VANA (fixed cap, no discretionary minting) |
| Circulating | ~30.8 million (~25.7 percent) |

---

## 8. Conclusion

Vana is a legitimate, top tier venture backed Layer 1 for user owned data with a genuinely capped 120 million supply with no discretionary minting or inflationary emission, which keeps it in the MEDIUM band at 58/100. It is held back because only about a quarter of supply is circulating with roughly a third held by insiders unlocking on cliffs into 2029 and a high fully diluted valuation, because the validator set is permissioned, whitelisted and undisclosed with early phase rewards to a public goods fund against the open and permissionless framing, because DataDAO usage is thin, and because impostor tokens exist on other chains, against a roughly 97 percent drawdown. The supply and backing are real; the caution is the heavy unlock overhang, a permissioned validator set, thin usage and impostor token risk.

---

## 9. Recommendations

**For the Vana team:**
- Publish the active validator count and a path toward permissionless validation, so the open and permissionless framing is accurate.
- Present the low float, the insider unlock schedule and the high fully diluted valuation prominently.
- Clearly document the canonical token to protect holders from impostor VANA tokens on other chains.

**For users:**
- Use only the official omnichain VANA token, never impostor tokens on Ethereum.
- Value the genuinely capped, non inflationary supply, but model the low float, the insider unlock overhang into 2029 and the drawdown.

---

## 10. Verification

- MEFAI on chain analysis: review of the Vana chain identity (chain id 1480), the official omnichain VANA token and its 18 decimals, the fixed non mintable 120 million cap, the ~25.7 percent circulating float and insider unlock schedule, and the permissioned validator set.
- The chain parameters, token and supply are publicly verifiable on the Vana and Ethereum explorers.
- Project statements: the project's website and documentation (the user owned data, DataDAO and data liquidity for AI framing) and the published tokenomics and validator model.
