# Security Audit Report: Sui (SUI) - Sui Mainnet

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-31 |
| **Project** | Sui |
| **Token Symbol** | SUI |
| **Native coin type** | `0x2::sui::SUI` (Sui mainnet, non-EVM) |
| **Chain** | Sui |
| **Audit Type** | Network Native Token (Claim vs Reality) |
| **Mefai Security Score** | **62/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on-chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Sui is a real, technically capable Move-based layer-1 with genuine sub-second finality, a parallel-execution consensus (Mysticeti), and a genuinely fixed supply cap. It is a legitimate network. The MEDIUM rating reflects a throughput-marketing gap and a supply-overhang/concentration gap, offset by the fixed cap:

1. **A recent "6 million-plus TPS" headline is an off-chain figure, not on-chain consensus.** MEFAI's review finds the project's own blog attributes that number to off-chain "programmable tunnels" (payment channels), not the public mainnet. The documented on-chain benchmarks (~300,000 to 400,000 TPS) are controlled 10-to-100-node tests, while live mainnet runs in the hundreds of TPS (peaks around 1,000). Sub-second finality is genuine.
2. **Large insider and foundation supply overhang with cliff unlocks.** Roughly 22 percent of supply sits with investors and early contributors, on top of a Foundation-controlled reserve exceeding 50 percent released after 2030, with lumpy cliff unlocks through 2030.
3. **Stake concentration is an acknowledged concern**, with an indicative validator Nakamoto figure around 18.

To its credit, SUI has a **genuinely hard-capped 10 billion supply**, verified, which is a real structural positive.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Native coin** | `0x2::sui::SUI` |
| **Decimals (verified)** | 9 (1 SUI = 10^9 MIST) |
| **Max supply** | 10,000,000,000 SUI (hard cap, verified) |
| **Circulating** | ~40.5 percent (~4.05 billion), rest vesting to ~2030 |
| **Allocation** | Post-2030 reserve 52.17 percent; community reserve 10.65 percent; stake subsidies 9.49 percent; investors (Series A+B) ~14.1 percent; early contributors 6.13 percent; company treasury ~1.64 percent |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI verified SUI's supply cap and reviewed distribution and consensus:

| Check | Result |
|-------|--------|
| Coin identity / decimals | `0x2::sui::SUI`, 9 decimals (MIST), verified |
| Max supply | 10,000,000,000 SUI, hard-capped (verified) |
| Circulating | ~40.5 percent (~4.05 billion), rest vesting to ~2030 |
| Insider + investor allocation | ~22 percent, plus a >50 percent Foundation-controlled post-2030 reserve |
| Validator concentration | Indicative Nakamoto ~18 (acknowledged concern) |

**Interpretation.** SUI is a genuine, liquid token with a real hard cap, a structural positive. The cautions are a large, lumpy unlock overhang (insiders/investors plus a very large foundation reserve) and stake concentration.

---

## 3. Claim vs Reality: "6 Million TPS" and Throughput

> Docs: "Mysticeti handles 300,000 transactions per second (TPS) before latency crosses the 1-second marker"; "sustained throughput of 200,000 TPS"; "reaches consensus commitment in about 0.5 seconds." A 2026 blog post is headlined "Sui Processes Over 6 Million Transactions Per Second."

**Reality: sub-second finality real, throughput headlines are lab/off-chain.** The ~0.5-second commit and parallel execution are genuine. But:
- The **"6 million-plus TPS" figure is off-chain**: the project's own post attributes it to "programmable tunnels" (off-chain payment channels), not the public mainnet consensus.
- The **300,000 to 400,000 TPS figures are controlled benchmarks** (10-to-100-node tests), and the older "297,000 TPS" number is a testnet benchmark, not a current headline.
- **Live mainnet runs roughly three orders of magnitude lower**, hundreds of TPS with peaks near 1,000; the best controlled figure (~103,000 certificates per second) is still a lab number.

This is capacity/headroom, not demonstrated demand, and the "6 million TPS" framing conflates an off-chain channel throughput with on-chain performance.

---

## 4. Claim vs Reality: Supply Cap vs Unlock Overhang

The **10 billion hard cap is genuine and verified**, a real positive that many peers lack. But circulating supply is only ~40.5 percent, and the schedule to ~2030 is dominated by **cliff unlocks** (all-at-once releases after a waiting period), producing lumpy supply shocks. Insider and investor allocations total roughly **22 percent**, and a **Foundation-controlled reserve exceeding 50 percent** is released after 2030, a very large long-tail overhang. Effective decentralization of holdings is therefore far lower than the fixed-cap headline suggests.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| SUI-001 | **MEDIUM** | "6 million-plus TPS" is an off-chain payment-channel figure, not on-chain mainnet; benchmark TPS (~300k to 400k) is lab, live mainnet is hundreds of TPS. |
| SUI-002 | **MEDIUM** | Large unlock overhang: ~22 percent insider/investor plus a >50 percent Foundation reserve released after 2030; lumpy cliff unlocks to 2030. |
| SUI-003 | **LOW** | Stake concentration an acknowledged concern (indicative validator Nakamoto ~18). |
| SUI-004 | **INFO** | Genuinely hard-capped 10 billion supply, verified (structural positive). |
| SUI-005 | **INFO** | Genuine sub-second finality and parallel-execution consensus (positive). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, liquid, real network |
| Supply / minting | Low risk | Hard-capped 10 billion (positive) |
| Unlock / concentration | Medium risk | ~22 percent insider + >50 percent foundation reserve, cliff unlocks |
| Throughput claims | Medium risk | 6M TPS off-chain; benchmarks vs ~1,000 TPS live |
| Decentralization | Low to medium risk | Indicative Nakamoto ~18 |
| Transparency | Low to medium risk | Sub-second real; TPS framing conflates off-chain |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Native coin | `0x2::sui::SUI` (9 decimals, MIST) |
| Max supply | 10,000,000,000 SUI (hard cap, verified) |
| Circulating | ~40.5 percent (~4.05 billion) |
| Insider + investor allocation | ~22 percent (+ >50 percent post-2030 foundation reserve) |
| Consensus | Mysticeti, ~0.5s commit; benchmark ~300k to 400k TPS |

---

## 8. Conclusion

Sui is a real, capable Move layer-1 with genuine sub-second finality and, notably, a genuinely hard-capped 10 billion supply, which keeps it in the MEDIUM band at 62/100. It is held back because its "6 million-plus TPS" headline is an off-chain payment-channel figure rather than on-chain performance, its documented benchmarks are lab tests roughly a thousand times above live mainnet throughput, and its supply carries a large insider/investor plus Foundation-reserve overhang with lumpy cliff unlocks to 2030. The technology and the fixed cap are real; the caution is throughput framing and unlock overhang.

---

## 9. Recommendations

**For the Sui team:**
- Clearly label off-chain "programmable tunnels" throughput separately from on-chain mainnet performance, and publish sustained live TPS alongside benchmarks.
- Smooth or clearly disclose the cliff-unlock schedule and the size of the post-2030 Foundation reserve.
- Continue initiatives to raise validator decentralization.

**For users:**
- Treat the "6 million TPS" and benchmark figures as off-chain/lab capacity, not on-chain demonstrated demand.
- Model the insider and Foundation-reserve unlock overhang through 2030; value the hard cap as a genuine positive.

---

## 10. Verification

- MEFAI on-chain analysis: verification of the SUI coin type and 9-decimal (MIST) denomination and the 10 billion hard-cap tokenomics (the public JSON-RPC total-supply method has been deprecated on public nodes, so the fixed cap was confirmed from the network's published tokenomics), plus review of the allocation and unlock schedule.
- The coin type, supply cap and unlock schedule are publicly verifiable on the Sui explorers and tokenomics documentation.
- Project statements: the Sui documentation (the Mysticeti "300,000 TPS" and "about 0.5 seconds" wording and the "10,000,000,000 SUI" cap), the official blog (the "6 million-plus TPS" off-chain post and the MIST denomination), and the published token allocation and vesting schedule.
