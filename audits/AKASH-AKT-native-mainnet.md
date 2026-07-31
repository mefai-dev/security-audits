# Security Audit Report: Akash Network (AKT) on Akash

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Akash Network |
| **Token Symbol** | AKT |
| **Native denom** | `uakt` (Akash, chain id `akashnet-2`) |
| **Chain** | Akash (Cosmos SDK, native, non EVM) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **66/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Akash Network is a real, working decentralized compute marketplace with a capped supply token, a genuine positive in a sector full of vaporware. The MEDIUM rating reflects a large gap between the "supercloud" marketing and the actual, small and recently shrinking hardware footprint:

1. **The token is capped and real**, and the reverse auction marketplace genuinely works, credit where due.
2. **But the "Global Grid, No Off Switch, Supercloud for AI" framing overstates a tiny fleet.** MEFAI's review of the most recent network data finds roughly 58 active providers and a few hundred GPUs total (with only tens actually in use), and both supply and usage fell sharply in the most recent quarter.
3. **Utilization is low** (roughly a third), so most GPU capacity sits idle, and the headline cheap prices partly reflect oversupply and low demand rather than a durable at scale alternative.
4. **Roadmap control remains with the founding company** despite on chain governance.

Akash is a legitimate, working product; the caution is that its scale and utilization are far below the "supercloud" branding.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token** | AKT (staking, governance, compute payment) |
| **Native denom** | `uakt` (chain id `akashnet-2`), 6 decimals |
| **Max supply** | ~388.5 million AKT (capped) |
| **Circulating** | ~292 million AKT |
| **Inflation** | Capped at 8 percent (Proposal 283 lowered the ceiling from 13 percent to a 4 to 8 percent band); realized rate recently near 8 to 9 percent |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI reviewed AKT's supply and the network's economic parameters:

| Check | Result |
|-------|--------|
| Token type | Cosmos SDK native (CometBFT), delegated proof of stake; not an ERC 20 |
| Denom / decimals | `uakt`, 6 decimals |
| Max supply | ~388.5 million AKT (capped) |
| Circulating | ~292 million AKT |
| Inflation | ~8 to 9 percent realized (8 percent ceiling after Proposal 283; 4 to 8 percent band), governance adjustable |
| Value accrual | Staking, governance, and compute payment; newer burn mint credit model (AEP 76) |

**Interpretation.** AKT is a genuine, capped supply staking and governance token, a real positive versus uncapped peers. It is inflationary today (~8 to 9 percent), and value accrual depends on real compute demand.

---

## 3. Claim vs Reality: "Supercloud for AI" / Scale

> Site: "The Open Cloud for AI's Next Frontier"; "Supercloud for AI"; "Global Grid. No Off Switch."; H100 pricing framed as roughly a third of a large centralized provider's rate.

**Reality: real marketplace, tiny fleet.** MEFAI's review of the most recent network data finds:
- **~58 active providers** (among the lowest in recent history, down from roughly 63 to 69 in prior periods).
- **A few hundred GPUs total capacity** (on the order of ~334), with **only tens in active use** (~84), and both figures fell roughly 57 percent quarter over quarter.
- **~33.7 percent utilization**, meaning roughly two thirds of GPU capacity sits idle.

Against any centralized provider (which operate hundreds of thousands of GPUs), Akash's fleet is minuscule. "Supercloud / Global Grid, No Off Switch" describes an aspiration, not the current footprint.

---

## 4. Claim vs Reality: Cheap Prices and Utilization

The headline cheap prices (an H100 well below a large centralized provider's rate; claims of large cost savings) are **real for spot inventory**, but availability of specific high end GPUs is thin, shrinking and volatile. Crucially, the low price partly reflects **oversupply and low demand**, not a durable, at scale cost advantage, the same quarter that prices looked attractive, utilization was only ~33.7 percent and both supply and usage contracted. The "maximize hardware utilization via reverse auction" claim sits against roughly two thirds idle GPUs.

---

## 5. Claim vs Reality: Decentralization and Control

Akash runs genuine on chain (Cosmos) governance over parameters and proposals, a real strength. But the **founding company continues to drive the technical roadmap**, and the small, declining provider count means effective decentralization of the actual compute supply is limited. Decentralization is real at the token governance layer and thin at the hardware layer.

---

## 6. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| AKT 001 | **MEDIUM** | "Supercloud / Global Grid" vs a tiny fleet (~58 providers, ~334 GPUs, ~84 in use), both down ~57 percent quarter over quarter. |
| AKT 002 | **MEDIUM** | Low utilization (~33.7 percent): most GPU capacity idle; cheap prices partly reflect oversupply and low demand. |
| AKT 003 | **LOW** | Roadmap control remains with the founding company despite on chain governance. |
| AKT 004 | **LOW** | Inflationary today (~8 to 9 percent), value accrual dependent on unproven at scale demand. |
| AKT 005 | **INFO** | Capped supply (~388.5 million) and a real, working reverse auction marketplace (positives). |
| AKT 006 | **INFO** | On chain Cosmos governance genuine (positive). |

---

## 7. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, capped, real product |
| Supply / minting | Low to medium risk | Capped but inflationary (~8 to 9 percent) |
| Scale claims | Medium risk | Tiny, shrinking GPU fleet vs "supercloud" |
| Utilization / demand | Medium risk | ~33.7 percent utilization, declining usage |
| Decentralization | Low to medium risk | Company led roadmap, thin provider set |
| Transparency | Low risk | Tokenomics and governance documented on chain |

---

## 8. Technical Specifications

| Item | Value |
|------|-------|
| Native denom | `uakt` (chain id `akashnet-2`), 6 decimals |
| Max supply | ~388.5 million AKT (capped) |
| Circulating | ~292 million AKT |
| Inflation | ~8 to 9 percent (8 percent ceiling) |
| Active providers | ~58 |
| GPU capacity / in use | ~334 total / ~84 in use (~33.7 percent utilization) |

---

## 9. Conclusion

Akash Network is a real, working decentralized compute marketplace with a genuinely capped supply token, which keeps it in the MEDIUM band at 66/100. It is held back because the "Supercloud for AI / Global Grid, No Off Switch" branding overstates a tiny and recently shrinking fleet (roughly 58 providers, a few hundred GPUs, tens in use, ~33.7 percent utilization, both supply and usage down about 57 percent quarter over quarter), and because roadmap control stays with the founding company. The product is legitimate; the caution is scale and utilization far below the marketing.

---

## 10. Recommendations

**For the Akash team:**
- Publish live, prominent provider, GPU and utilization figures so the "supercloud" claim is grounded in the real footprint.
- Present cheap prices with the utilization context (idle capacity, oversupply), rather than as a proven at scale advantage.
- Continue decentralizing roadmap control beyond the founding company.

**For users:**
- Understand the real fleet is small and volatile; specific high end GPU availability is not guaranteed.
- Treat AKT as a capped but inflationary token whose value depends on compute demand that is not yet proven at scale.

---

## 11. Verification

- MEFAI on chain analysis: review of AKT's capped supply (~388.5 million), current inflation (~8 to 9 percent) and the Cosmos governance and marketplace parameters, plus the most recent network provider, GPU and utilization data.
- The supply, inflation and governance are publicly verifiable on the Akash explorers and on chain parameters.
- Project statements: the project's website ("Supercloud for AI", "Global Grid. No Off Switch.", the H100 pricing comparison) and its token and documentation pages.
