# Security Audit Report: Aptos (APT) on Aptos Mainnet

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Aptos |
| **Token Symbol** | APT |
| **Native coin type** | `0x1::aptos_coin::AptosCoin` (Aptos mainnet, non EVM) |
| **Chain** | Aptos |
| **Audit Type** | Network Native Token (Claim vs Reality) |
| **Mefai Security Score** | **62/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Aptos is a real, technically capable Move based layer 1 with genuine sub second finality and a working parallel execution engine. It is a legitimate network. The MEDIUM rating reflects a large throughput marketing gap, a concentration and allocation gap, and an infrastructure centralization gap:

1. **The "160,000 TPS" / "fastest blockchain" framing is a benchmark ceiling, not live reality.** MEFAI's review finds a best measured live mainnet peak on the order of 13,000 TPS, with typical throughput in the tens to low hundreds, roughly a thousand times below the marketed maximum. Sub second finality itself is genuine.
2. **The "community 51 percent" allocation is Foundation heavy.** A large share of the "community" bucket is held by the Foundation and the core company, so genuinely community controlled float at genesis is far below the headline, and insider (contributor plus investor) allocations continue to unlock on a four year schedule concluding October 2026.
3. **Decentralization is undercut by infrastructure concentration:** while the validator distribution is moderate, more than a third of stake runs on a single cloud provider, and Foundation directed delegation dominates validator stake.

MEFAI verified an on chain circulating supply on the order of 845 million APT. Governance Proposal 183 (about March 2026) introduced a protocol level hard cap of 2.1 billion APT, so circulating supply is roughly 40 percent of the cap.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Native coin** | `0x1::aptos_coin::AptosCoin` (APT) |
| **Decimals (verified)** | 8 |
| **Genesis supply** | 1,000,000,000 APT at genesis (October 2022) |
| **Hard cap** | 2.1 billion APT (introduced by governance Proposal 183, about March 2026; previously uncapped) |
| **Circulating (verified)** | ~845 million APT (~40 percent of the 2.1 billion cap; total minted supply ~1.206 billion, ~57 percent of the cap) |
| **Staking inflation** | Launch schedule 7 percent declining 1.5 percent per year to a 3.25 percent floor; 2026 governance reduced the reward rate further (to about 2.6 percent APR) |
| **Allocation** | Community 51.02 percent (Foundation heavy); core contributors 19 percent; Foundation 16.5 percent; investors 13.48 percent |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI read the AptosCoin resource and reviewed staking and validator parameters:

| Check | Result |
|-------|--------|
| Coin identity / decimals | "Aptos Coin", 8 decimals, verified |
| Circulating supply | ~845 million APT (~40 percent of the 2.1 billion cap) |
| Genesis / cap | 1 billion at genesis; 2.1 billion hard cap added by governance Proposal 183 (about March 2026) |
| Staking inflation | Launch schedule 7 percent to 3.25 percent; reduced further by 2026 governance (AIP 119) to about 2.6 percent APR |
| Validators | ~92 to 115 active; validator distribution Nakamoto ~13 to 14 |
| Infrastructure | More than a third of stake on a single cloud provider (operational Nakamoto 1) |

**Interpretation.** APT is a genuine, liquid token, and the 2026 hard cap is a positive that bounds long run dilution. The two structural cautions are that a large part of the "community" allocation is Foundation controlled (so effective decentralization of holdings is lower than the headline), and that despite a moderate validator distribution figure, stake is concentrated on one cloud provider and directed heavily by the Foundation.

---

## 3. Claim vs Reality: "Fastest Blockchain" / TPS

> Official blog: "Aptos is the fastest blockchain to consistently achieve sub second E2E latency for all transaction types"; "peak throughput of 30,000 transactions per second" (previewnet); "establishing Aptos as the fastest blockchain in the industry." A "160,000 TPS" maximum lives in the technical / Block STM materials.

**Reality: sub second finality real, TPS headline is a benchmark.** Sub second latency and the parallel execution engine are genuine strengths. But the **160,000 TPS figure is a theoretical or benchmark ceiling** (Block STM on a single multicore machine under low contention); the best geo distributed test figures are ~20,000 to 30,000 TPS, and the **best measured live mainnet peak is on the order of 13,000 TPS with typical throughput in the tens to low hundreds**. The network is demand bound, not capacity bound: the marketed maximum is roughly a thousand times above normal live load.

---

## 4. Claim vs Reality: Allocation and Decentralization

- The **"community 51.02 percent" allocation is Foundation heavy**: a large portion (hundreds of millions of APT, roughly 410 million held by the Foundation and 100 million by the core company) sits within the "community" bucket, so genuinely community controlled float at genesis is far below the headline.
- **Insider unlocks:** core contributor and investor tokens vest on a four year schedule **concluding October 2026**, a persistent dilution and sell pressure factor alongside declining staking inflation.
- **Infrastructure concentration:** the validator distribution Nakamoto figure (~13 to 14) is moderate, but more than a third of stake runs on a **single cloud provider** (an operational Nakamoto coefficient of 1), and the Foundation effectively directs a majority of validator stake.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| APT 001 | **MEDIUM** | "160,000 TPS / fastest blockchain" is a benchmark ceiling; best live mainnet peak on the order of 13,000 TPS, typical tens to low hundreds. |
| APT 002 | **MEDIUM** | "Community 51 percent" is Foundation heavy (roughly 410 million with the Foundation, 100 million with the core company); genuinely community controlled float far below the headline. |
| APT 003 | **MEDIUM** | Infrastructure concentration: more than a third of stake on a single cloud provider (operational Nakamoto 1); Foundation directed stake dominates. |
| APT 004 | **LOW** | Insider (contributor + investor) unlocks on a four year schedule concluding October 2026 (dilution overhang). |
| APT 005 | **INFO** | governance Proposal 183 (about March 2026) added a 2.1 billion hard cap, bounding long run dilution (positive). |
| APT 006 | **INFO** | Genuine sub second finality and working parallel execution (positive). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, liquid, real network |
| Supply / minting | Low to medium risk | 2026 hard cap (positive), insider unlocks to Oct 2026 |
| Throughput claims | Medium risk | 160k TPS benchmark vs ~13k live peak |
| Concentration | Medium risk | Foundation heavy "community" allocation |
| Infrastructure | Medium risk | >1/3 stake on one cloud; Foundation directed |
| Transparency | Low to medium risk | Sub second real; TPS and allocation framing overstated |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Native coin | `0x1::aptos_coin::AptosCoin` (8 decimals) |
| Genesis / cap | 1 billion genesis; 2.1 billion hard cap (2026 update) |
| Circulating (verified) | ~845 million APT (~40 percent of cap) |
| Staking inflation | Launch 7 percent to 3.25 percent; reduced by 2026 governance |
| Validators | ~92 to 115; distribution Nakamoto ~13 to 14; >1/3 stake on one cloud (operational Nakamoto 1) |

---

## 8. Conclusion

Aptos is a real, capable Move layer 1 with genuine sub second finality and parallel execution, and a 2026 hard cap of 2.1 billion APT that bounds long run dilution, which keeps it in the MEDIUM band at 62/100. It is held back because its "fastest blockchain / 160,000 TPS" framing is a benchmark ceiling roughly a thousand times above live throughput, its "community 51 percent" allocation is Foundation heavy, more than a third of stake runs on one cloud provider, and insider unlocks run to October 2026. The technology is real; the caution is throughput marketing, allocation concentration and infrastructure centralization.

---

## 9. Recommendations

**For the Aptos team:**
- Present "160,000 TPS" as a theoretical maximum and publish sustained live throughput alongside it.
- Disclose how much of the "community" allocation is Foundation or company controlled, and publish validator infrastructure (cloud) distribution.
- Continue reducing single cloud stake concentration.

**For users:**
- Treat the TPS headline as capacity, not demonstrated demand, and the "community" allocation as partly Foundation controlled.
- Model insider unlocks through October 2026; note the 2.1 billion hard cap as a long run positive.

---

## 10. Verification

- MEFAI on chain analysis: a read of the AptosCoin resource (identity, 8 decimals, circulating supply on the order of 845 million) and review of staking inflation and validator distribution parameters, plus governance Proposal 183 (about March 2026) introducing the 2.1 billion hard cap.
- The coin resource, supply and validator set are publicly verifiable on the Aptos explorers.
- Project statements: the Aptos Labs blog ("fastest blockchain... sub second E2E latency", "peak throughput of 30,000 TPS"), the Aptos tokenomics materials (genesis 1 billion, allocation splits, staking reward schedule, the 2026 hard cap), and the technical materials citing the 160,000 TPS maximum.
