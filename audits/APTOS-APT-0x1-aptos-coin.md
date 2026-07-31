# Security Audit Report: Aptos (APT) - Aptos Mainnet

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-31 |
| **Project** | Aptos |
| **Token Symbol** | APT |
| **Native coin type** | `0x1::aptos_coin::AptosCoin` (Aptos mainnet, non-EVM) |
| **Chain** | Aptos |
| **Audit Type** | Network Native Token (Claim vs Reality) |
| **Mefai Security Score** | **62/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on-chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Aptos is a real, technically capable Move-based layer-1 with genuine sub-second finality and a working parallel-execution engine. It is a legitimate network. The MEDIUM rating reflects a large throughput-marketing gap, a concentration/allocation gap, and an infrastructure-centralization gap:

1. **The "160,000 TPS" / "fastest blockchain" framing is a benchmark ceiling, not live reality.** MEFAI's review finds a best measured live mainnet peak on the order of ~13,000 TPS, with typical throughput in the tens to low hundreds, roughly a thousand times below the marketed maximum. Sub-second finality itself is genuine.
2. **The "community 51 percent" allocation is Foundation-heavy.** A large share of the "community" bucket is held by the Foundation and the core company, so genuinely community-controlled float at genesis is far below the headline, and insider (contributor plus investor) allocations continue to unlock on a four-year schedule concluding October 2026.
3. **Decentralization is undercut by infrastructure concentration:** while the validator distribution is reasonable, more than a third of stake runs on a single cloud provider, and Foundation-directed delegation dominates validator stake.

MEFAI verified an on-chain circulating supply on the order of ~797 million APT, consistent with roughly 38 percent of the token unlocked.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Native coin** | `0x1::aptos_coin::AptosCoin` (APT) |
| **Decimals (verified)** | 8 |
| **Genesis / trajectory** | 1,000,000,000 at genesis, rising toward ~1.5 billion by ~2031 |
| **Circulating (verified)** | ~797 million APT (~38 percent unlocked) |
| **Staking inflation** | Max 7 percent, declining 1.5 percent per year to a 3.25 percent floor |
| **Allocation** | Community 51.02 percent (Foundation-heavy); core contributors 19 percent; Foundation 16.5 percent; investors 13.48 percent |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI read the AptosCoin resource and reviewed staking and validator parameters:

| Check | Result |
|-------|--------|
| Coin identity / decimals | "Aptos Coin", 8 decimals, verified |
| Circulating supply | ~797 million APT (~38 percent unlocked) |
| Genesis / trajectory | 1 billion genesis, rising toward ~1.5 billion by ~2031 |
| Staking inflation | 7 percent, declining to 3.25 percent |
| Validators | ~132 active; validator distribution Nakamoto ~21 |
| Infrastructure | More than a third of stake on a single cloud provider (operational concentration) |

**Interpretation.** APT is a genuine, liquid token. The two structural cautions are that a large part of the "community" allocation is Foundation-controlled (so effective decentralization of holdings is lower than the headline), and that despite a reasonable validator-distribution figure, stake is concentrated on one cloud provider and directed heavily by the Foundation.

---

## 3. Claim vs Reality: "Fastest Blockchain" / TPS

> Official blog: "Aptos is the fastest blockchain to consistently achieve sub-second E2E latency for all transaction types"; "peak throughput of 30,000 transactions per second" (previewnet); "establishing Aptos as the fastest blockchain in the industry." A "160,000 TPS" maximum lives in the technical / Block-STM materials.

**Reality: sub-second finality real, TPS headline is a benchmark.** Sub-second latency and the parallel-execution engine are genuine strengths. But the **160,000 TPS figure is a theoretical/benchmark ceiling**; the best geo-distributed test figures are ~20,000 to 30,000 TPS, and the **best measured live mainnet peak is on the order of 13,000 TPS with typical throughput in the tens to low hundreds**. The network is demand-bound, not capacity-bound: the marketed maximum is roughly a thousand times above normal live load.

---

## 4. Claim vs Reality: Allocation and Decentralization

- The **"community 51.02 percent" allocation is Foundation-heavy**: a large portion (hundreds of millions of APT) is held by the Foundation and the core company, so genuinely community-controlled float at genesis is far below the headline.
- **Insider unlocks:** core-contributor and investor tokens vest on a four-year schedule **concluding October 2026**, a persistent dilution/sell-pressure factor alongside declining staking inflation.
- **Infrastructure concentration:** the validator-distribution Nakamoto figure (~21) is reasonable, but more than a third of stake runs on a **single cloud provider** (an operational single-point concentration), and the Foundation effectively directs a majority of validator stake.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| APT-001 | **MEDIUM** | "160,000 TPS / fastest blockchain" is a benchmark ceiling; best live mainnet peak ~13,000 TPS, typical tens to low hundreds. |
| APT-002 | **MEDIUM** | "Community 51 percent" is Foundation-heavy; genuinely community-controlled float far below the headline. |
| APT-003 | **MEDIUM** | Infrastructure concentration: more than a third of stake on a single cloud provider; Foundation-directed stake dominates. |
| APT-004 | **LOW** | Insider (contributor + investor) unlocks on a four-year schedule concluding October 2026 (dilution overhang). |
| APT-005 | **INFO** | Genuine sub-second finality and working parallel execution (positive). |
| APT-006 | **INFO** | Verified ~797 million circulating; declining staking inflation (7 percent to 3.25 percent). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, liquid, real network |
| Supply / minting | Low to medium risk | Inflationary (declining), insider unlocks to Oct 2026 |
| Throughput claims | Medium risk | 160k TPS benchmark vs ~13k live peak |
| Concentration | Medium risk | Foundation-heavy "community" allocation |
| Infrastructure | Medium risk | >1/3 stake on one cloud; Foundation-directed |
| Transparency | Low to medium risk | Sub-second real; TPS and allocation framing overstated |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Native coin | `0x1::aptos_coin::AptosCoin` (8 decimals) |
| Genesis / trajectory | 1 billion genesis, rising toward ~1.5 billion by ~2031 |
| Circulating (verified) | ~797 million APT (~38 percent) |
| Staking inflation | 7 percent declining to 3.25 percent |
| Validators | ~132; distribution Nakamoto ~21; >1/3 stake on one cloud |

---

## 8. Conclusion

Aptos is a real, capable Move layer-1 with genuine sub-second finality and parallel execution, which keeps it in the MEDIUM band at 62/100. It is held back because its "fastest blockchain / 160,000 TPS" framing is a benchmark ceiling roughly a thousand times above live throughput, its "community 51 percent" allocation is Foundation-heavy, more than a third of stake runs on one cloud provider, and insider unlocks run to October 2026. The technology is real; the caution is throughput marketing, allocation concentration and infrastructure centralization.

---

## 9. Recommendations

**For the Aptos team:**
- Present "160,000 TPS" as a theoretical maximum and publish sustained live throughput alongside it.
- Disclose how much of the "community" allocation is Foundation/company-controlled, and publish validator infrastructure (cloud) distribution.
- Continue reducing single-cloud stake concentration.

**For users:**
- Treat the TPS headline as capacity, not demonstrated demand, and the "community" allocation as partly Foundation-controlled.
- Model insider unlocks through October 2026.

---

## 10. Verification

- MEFAI on-chain analysis: a read of the AptosCoin resource (identity, 8 decimals, circulating supply ~797 million) and review of staking inflation and validator distribution parameters.
- The coin resource, supply and validator set are publicly verifiable on the Aptos explorers.
- Project statements: the Aptos Labs blog ("fastest blockchain... sub-second E2E latency", "peak throughput of 30,000 TPS"), the Aptos tokenomics summary (genesis 1 billion, allocation splits, staking-reward schedule), and the technical materials citing the 160,000 TPS maximum.
