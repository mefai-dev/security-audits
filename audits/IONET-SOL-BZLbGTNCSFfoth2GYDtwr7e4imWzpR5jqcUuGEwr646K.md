# Security Audit Report: io.net (IO) - Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | io.net |
| **Token Symbol** | IO |
| **Contract (mint)** | `BZLbGTNCSFfoth2GYDtwr7e4imWzpR5jqcUuGEwr646K` |
| **Chain** | Solana |
| **Audit Type** | Token + Project (Claim vs Reality) |
| **Mefai Security Score** | **50/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on-chain analysis. The assessments are Mefai Security Research's analysis and opinion, not statements about the private intentions of any team. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Executive Summary

io.net is a genuine GPU rental marketplace that resells real third party compute at transparent hourly prices, and its token has no rug vector: MEFAI's on-chain analysis confirms both the mint and freeze authorities are revoked. The MEDIUM rating is driven by a metrics and transparency problem rather than a contract problem:

1. The headline "over 320,000 verified GPUs" is materially inflated: only a low single digit thousands are actually usable at any time, and independent measurement placed daily verified-active GPUs near 2 percent of the registered total.
2. The project itself publicly admitted, in 2024, that roughly 400,000 "workers" were being spoofed on the network through an API that accepted metadata without proper authentication.
3. The "open source AI infrastructure" positioning masks a **closed, centralized orchestration backend**; only worker binaries and demos are public.

The token is clean; the risk is that the network's advertised scale and decentralization do not match measured reality.

---

## 1. Contract Overview

| Field | Value |
|-------|-------|
| **Token** | IO |
| **Mint** | `BZLbGTNCSFfoth2GYDtwr7e4imWzpR5jqcUuGEwr646K` |
| **Decimals** | 8 |
| **Supply** | ~798.8 million of an 800 million cap |
| **Mint authority** | `null` (revoked) |
| **Freeze authority** | `null` (revoked) |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana reads (`getAccountInfo`, `getTokenSupply`) confirmed:

| Check | Result |
|-------|--------|
| Mint authority | **Revoked** (`null`) - no new IO can be minted |
| Freeze authority | **Revoked** (`null`) - accounts cannot be frozen |
| Supply | ~798.8 million, just under the 800 million cap |
| Rug vector | None at the contract level |

**Interpretation.** The IO token itself is clean and low risk. There is no dilution, freeze or honeypot vector on the token. The project's risk lies in its off-chain network claims, assessed below.

---

## 3. Claim vs Reality: "Over 320,000 Verified GPUs"

> Marketing: "the world's largest decentralized GPU cloud network ... over 320,000 verified GPUs".

**Reality:** the same marketing material concedes only close to 7,000 GPUs are readily available for larger deployments. Independent measurement placed daily verified-active GPUs at roughly **6,720 out of about 327,000 registered (about 2 percent)**, with the verified count declining quarter over quarter. The advertised-versus-usable gap is on the order of 40 to 50 times. io.net's current homepage has also quietly moved from a "600K+ GPUs" headline to a vaguer "thousands", which is itself consistent with the larger figures not holding up.

---

## 4. Confirmed Controversy: ~400,000 Spoofed Workers

In April 2024, io.net **publicly stated on its own official channel** that it was aware of virtual GPU abuse spoofing approximately **400,000 workers**. Its own incident report attributes the root cause to an application programming interface that accepted worker metadata updates without proper authentication. The fix introduced a periodic proof-of-work check. This is not an allegation by MEFAI; it is io.net's own public admission, and it demonstrates that the raw "worker" and "GPU" counts were, at least during that period, trivially inflatable.

---

## 5. Claim vs Reality: "Open Source" and "Decentralized"

> Marketing: "The Open Source AI Infrastructure Platform" and "a decentralized GPU network".

**Reality:** the public code consists of worker binaries, a setup script, an attestation agent and chatbot demos. The **network orchestration, scheduling and matching logic (the actual "platform") is not open sourced** and runs on a proprietary centralized backend (consistent with the 2024 incident being an internal worker-API flaw). The live homepage inventory shows internal-supplier capacity sold out with rentable compute brokered from independent external suppliers, while scheduling and verification remain centralized. The aggregation is real; the "decentralized, open source platform" framing overstates it.

---

## 6. Positive Findings (Credited)

- **The token is clean:** mint and freeze authorities revoked, capped supply, widely listed, real project with real on-chain revenue and reputable backers.
- **The marketplace is real:** the live inventory lists genuine, bookable GPU SKUs at concrete hourly prices, and the price-advantage over centralized cloud is directionally credible.

---

## 7. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| IONET-001 | **MEDIUM** | Headline GPU count (320,000+) is inflated ~40-50x versus usable/active reality (~2 percent active). |
| IONET-002 | **MEDIUM** | Project's own 2024 admission of ~400,000 spoofed workers via an unauthenticated API. |
| IONET-003 | **MEDIUM** | Core orchestration is closed source and centralized despite "open source" and "decentralized" framing. |
| IONET-004 | **LOW** | Metrics methodology behind headline figures is undisclosed; figures drift over time. |
| IONET-005 | **INFO** | Token authorities revoked, capped supply, no rug vector (positive). |
| IONET-006 | **INFO** | Real, bookable GPU marketplace with transparent pricing (positive). |

---

## 8. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Ownership control | Low risk | Authorities revoked |
| Supply / minting | Low risk | Capped, mint revoked |
| Liquidity security | Low to medium risk | Widely listed |
| Decentralization | Medium to high risk | Centralized closed orchestration |
| Metrics integrity | High risk | ~2 percent active vs advertised; spoofing history |
| Transparency | Medium risk | Undisclosed methodology, quietly reduced headline numbers |

---

## 9. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `BZLbGTNCSFfoth2GYDtwr7e4imWzpR5jqcUuGEwr646K` |
| Decimals / supply | 8 / ~798.8M of 800M cap |
| Mint / freeze authority | Revoked / Revoked |
| Advertised GPUs | 320,000+ |
| Independently measured active | ~6,720 (~2 percent) |

---

## 10. Conclusion

io.net is a legitimate GPU marketplace with a clean, capped, authority-revoked token, which keeps it out of the high-risk band. The MEDIUM rating (50/100) reflects a serious gap between advertised scale and measured reality: a ~40-50x inflated GPU headline, a self-admitted ~400,000 worker spoofing incident, and a closed, centralized orchestration backend behind "open source, decentralized" marketing. The token is safe to hold at the contract level; the caution is about believing the network's advertised size and decentralization.

---

## 11. Recommendations

**For the io.net team:**
- Publish a live, independently reproducible count of verified-active GPUs and the methodology behind headline figures.
- Open source the orchestration layer, or stop describing the platform as open source and decentralized.
- Maintain a public post-incident dashboard so spoofing mitigation is verifiable over time.

**For users:**
- The IO token is clean; treat network scale and decentralization claims with skepticism until independently reproducible.
- Do not size compute or investment decisions on the 320,000 GPU headline.

---

## 12. Verification

- MEFAI on-chain analysis: direct Solana reads of the IO mint account (mint authority, freeze authority, supply).
- The mint address and its revoked authorities are publicly verifiable by anyone on the Solana explorers.
- Project statements: io.net's own homepage inventory, its official channel's April 2024 spoofing admission, and its own published incident report.
- The GPU active-versus-registered gap was independently measured and publicly reported.
