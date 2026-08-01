# Security Audit Report: Helium (HNT) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 1, 2026 |
| **Project** | Helium |
| **Token Symbol** | HNT |
| **Mint (Solana)** | `hntyVP6YFm1Hg25TN9WGLqM12b8TQmcknKrdu1oxWux` |
| **Chain** | Solana |
| **Audit Type** | Project + Network (Claim vs Reality) |
| **Mefai Security Score** | **62/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Helium is a real, long running decentralized wireless (DePIN) network with a verified token and a genuine consumer mobile product. It is not a scam. The MEDIUM rating reflects a well documented gap between network scale and real usage, plus an active mint authority and emission sell pressure:

1. **The token is verified and capped:** MEFAI confirms the HNT mint on Solana with a fixed 223 million maximum, a revoked freeze authority and a broad holder base.
2. **But the mint authority is active** (a program authority that emits ongoing rewards), so new supply is minted continuously to hotspot owners and subDAOs.
3. **Real network usage has historically been tiny** relative to the large hotspot count, the classic Helium critique, and much consumer mobile usage is subsidized.
4. **Emission sell pressure and a severe drawdown** temper the picture despite a genuine product.

The product and network are real; the caution is an active mint authority, emission sell pressure and a usage versus hotspots gap.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Mint (Solana)** | `hntyVP6YFm1Hg25TN9WGLqM12b8TQmcknKrdu1oxWux` |
| **Decimals** | 8 |
| **Supply** | ~181.6 million HNT (max supply 223 million) |
| **Mint authority** | **ACTIVE** (a program authority that emits ongoing rewards) |
| **Freeze authority** | Revoked |
| **Model** | Burn and mint (Data Credits) with IOT and MOBILE subDAO tokens |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana read of the HNT mint returned:

| Check | Result |
|-------|--------|
| Mint identity | HNT (Solana SPL), 8 decimals, verified (migrated to Solana in 2023) |
| Supply | ~181.6 million HNT (max 223 million) |
| Mint authority | **ACTIVE**, a program authority that emits ongoing rewards to hotspots and subDAOs |
| Freeze authority | Revoked (accounts cannot be frozen) |
| SubDAO tokens | IOT (internet of things) and MOBILE, with a Data Credits burn and mint model |
| Holders | Broad holder base |

**Interpretation.** HNT is a genuine token with a revoked freeze authority, but the mint authority is active by design (a circuit breaker program authority that emits ongoing rewards), so new supply is minted continuously toward the 223 million maximum. That maximum is a net emissions cap under the burn and mint model (Data Credits burn HNT as usage occurs) rather than a simple fixed cap. The main cautions are emission sell pressure and the well documented gap between network scale and real usage.

---

## 3. Claim vs Reality: "The People's Network"

> Site: a decentralized wireless network built by the people, covering the world with internet of things and mobile coverage via hotspots, and a consumer mobile carrier.

**Reality: a real network with a documented usage versus scale gap.** Helium genuinely built a large, contributor owned hotspot network and launched a real consumer mobile carrier, a legitimate DePIN achievement. But the network is well known for a large gap between the number of hotspots and the actual data transferred, with real usage historically tiny relative to the token rewards paid to hotspot owners. The consumer mobile product has real subscribers, but a meaningful share of usage is subsidized by token incentives and roaming rather than organic paid demand.

---

## 4. Claim vs Reality: Emission, Governance and Value

- **Active mint and emission sell pressure:** the mint authority is active and emits ongoing rewards, so hotspot owners receive continuous new HNT and frequently sell it to offset hardware and operating costs, a structural sell pressure.
- **SubDAO complexity:** the IOT and MOBILE subDAO tokens and the Data Credits burn and mint model add complexity that can confuse holders, though the Data Credits sink is a genuine burn mechanism.
- **Governance concentration:** direction and governance remain influenced by the foundation and core stakeholders.
- **Value:** the token is down roughly 99 percent from its 2021 peak.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| HNT 001 | **MEDIUM** | Mint authority is ACTIVE (a program authority emitting ongoing rewards); continuous emission creates structural sell pressure. |
| HNT 002 | **MEDIUM** | Well documented gap between hotspot scale and real usage; consumer mobile usage partly subsidized rather than organic. |
| HNT 003 | **LOW** | SubDAO token and Data Credits complexity; governance influenced by the foundation. |
| HNT 004 | **LOW** | ~99 percent drawdown from the 2021 peak. |
| HNT 005 | **INFO** | Verified capped mint (223 million maximum), revoked freeze authority, real network and consumer mobile product (positives). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token mechanics | Low to medium risk | Capped, freeze revoked, but mint active |
| Supply / minting | Medium risk | Ongoing emission sell pressure |
| Usage reality | Medium risk | Usage small versus hotspot scale |
| Decentralization | Medium risk | Foundation influenced governance |
| Product reality | Low risk | Real network and consumer mobile carrier |
| Value / volatility | High risk | ~99 percent drawdown from peak |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `hntyVP6YFm1Hg25TN9WGLqM12b8TQmcknKrdu1oxWux` |
| Decimals | 8 |
| Supply | ~181.6 million HNT (max 223 million) |
| Mint / freeze authority | Active (program emission) / Revoked |
| Model | Burn and mint (Data Credits); IOT and MOBILE subDAOs |

---

## 8. Conclusion

Helium is a real, long running decentralized wireless network with a verified, capped supply token and a genuine consumer mobile product, which keeps it in the MEDIUM band at 62/100. It is held back because the mint authority is active and emits ongoing rewards that create structural sell pressure, because there is a well documented gap between the hotspot scale and real network usage with consumer mobile usage partly subsidized, and because it is down roughly 99 percent from peak. The product and network are real; the caution is the active emission, the usage versus scale gap and the drawdown.

---

## 9. Recommendations

**For the Helium team:**
- Report real network usage (data transferred and organic paying subscribers) prominently alongside hotspot counts.
- Present the ongoing emission and its sell pressure transparently.
- Simplify or clearly document the subDAO token and Data Credits model for holders.

**For users:**
- Understand the mint authority is active and emits ongoing rewards, and note the historical gap between hotspot scale and real usage.
- Value the real network and consumer mobile product on their merits, and model the emission sell pressure and drawdown.

---

## 10. Verification

- MEFAI on chain analysis: a direct Solana read of the HNT mint (identity, 8 decimals, ~181.6 million supply toward a 223 million cap, active program mint authority, revoked freeze authority) and review of the subDAO and Data Credits model.
- The mint address, supply and authorities are publicly verifiable by anyone on the Solana explorers.
- Project statements: the project's website and materials (the People's Network, hotspot coverage and consumer mobile framing) and the published tokenomics and subDAO structure.
