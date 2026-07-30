# Security Audit Report: Fetch.ai / ASI (FET) - Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | Fetch.ai (part of the Artificial Superintelligence Alliance) |
| **Token Symbol** | FET |
| **Contract (Ethereum)** | `0xaea46A60368A7bD060eec7DF8CBa43b7EF41Ad85` |
| **Chain** | Ethereum |
| **Audit Type** | Token + Project (Claim vs Reality) |
| **Mefai Security Score** | **77/100** |
| **Overall Risk** | **LOW to MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on-chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Fetch.ai (FET) is the strongest-rated project in this set (77/100, LOW-to-MEDIUM). It is an established, well-capitalized decentralized-AI project with a real, verified token, genuinely open-source agent tooling, and an on-chain token merger that actually executed as described. It is not a rug and the core technology is real. The points that keep it out of the "very low risk" band are narrative and structural, not fraud:

1. The headline "millions of AI agents" figure refers to an **off-chain directory**, not on-chain activity. The on-chain agent registry is far smaller (tens of thousands), so the marketing number overstates on-chain reality by roughly two orders of magnitude.
2. The **Artificial Superintelligence Alliance (ASI) rebrand is incomplete**: the on-chain symbol is still FET, and one founding member (Ocean) departed the alliance in October 2025, so "ASI" is a work in progress rather than a settled, single entity.
3. FET is **mint-capable by privileged role-holders** (not a fixed, mint-revoked supply), a standard but non-trivial centralization point.

The token and merger are real and verified; the caution is an overstated on-chain agent count, an unfinished rebrand/alliance, and role-based mint capability.

---

## 1. Contract Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | Fetch / FET |
| **Contract** | `0xaea46A60368A7bD060eec7DF8CBa43b7EF41Ad85` |
| **Decimals** | 18 |
| **Total supply** | 2,714,384,546.672 FET |
| **Mint capability** | Yes, held by privileged role-holders (not revoked) |
| **Verified source** | Yes |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI's on-chain analysis confirmed:

| Check | Result |
|-------|--------|
| Token identity | Verified FET contract, name "Fetch", symbol FET |
| Total supply | 2,714,384,546.672 FET |
| Mint capability | **Present, gated to privileged roles** (not a fixed mint-revoked supply) |
| Token merger | **Executed on-chain** at the published ratios (see Section 3) |
| Source verified | Yes |
| Honeypot / trap | None (standard, liquid ERC-20) |

**Interpretation.** FET is a clean, verified, highly liquid ERC-20 with real market depth. The one structural centralization point is that supply is not cryptographically fixed: privileged role-holders retain mint capability. This is common for large project tokens but should be understood, it is a trust point.

---

## 3. Claim vs Reality: The ASI Token Merger (Verified)

> Claim: FET is the merged token of the Artificial Superintelligence Alliance, absorbing AGIX and OCEAN at fixed ratios.

**Reality: TRUE and verified on-chain.** The merger executed as published in July 2024: AGIX converted to FET at approximately **0.43335** and OCEAN at approximately **0.43323**, and the supply was expanded accordingly. This is a genuine, on-chain, delivered event, a strong positive that distinguishes Fetch from projects whose claims are unproven.

---

## 4. Claim vs Reality: "Millions of AI Agents"

> Marketing: on the order of "2.7 million agents".

**Reality: OVERSTATED on-chain.** The large agent figure refers to an **off-chain directory / marketplace** (the project's agent platform), not to on-chain registrations. The **on-chain agent registry (the Almanac) contains on the order of tens of thousands** of registered agents, roughly two orders of magnitude smaller than the headline. The agents are real and the tooling works, but the "millions" figure is a platform/directory metric, not an on-chain one, and should not be read as on-chain scale.

---

## 5. Claim vs Reality: Open-source Agent Framework (Credited)

> Claim: the agent framework (uAgents) is open-source.

**Reality: TRUE and verified.** The uAgents framework is genuinely open-source under a permissive Apache-2.0 license and is publicly usable. This is a real, creditable contribution and supports the project's technical legitimacy.

---

## 6. Claim vs Reality: The ASI Rebrand and Alliance

The Artificial Superintelligence Alliance rebrand is **incomplete and evolving**:
- The **on-chain symbol is still FET**, not a finalized "ASI" ticker.
- One founding member, **Ocean, departed the alliance in October 2025**; the alliance now centers on Fetch.ai and its remaining partners.

So "ASI" describes an in-progress alliance and rebrand rather than a settled, single unified entity. This is not deceptive, but marketing that presents ASI as a finished, monolithic superintelligence entity overstates the current, still-transitioning reality.

---

## 7. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| FET-001 | **MEDIUM** | "Millions of agents" is an off-chain directory metric; on-chain registry is ~2 orders of magnitude smaller. |
| FET-002 | **LOW** | FET is mint-capable by privileged role-holders (supply not cryptographically fixed). |
| FET-003 | **LOW** | ASI rebrand incomplete (symbol still FET); a founding member (Ocean) left the alliance in Oct 2025. |
| FET-004 | **INFO** | Token merger executed on-chain at published ratios, July 2024 (verified positive). |
| FET-005 | **INFO** | uAgents framework genuinely open-source (Apache-2.0) (verified positive). |
| FET-006 | **INFO** | Verified, highly liquid ERC-20; no honeypot/trap (positive). |

---

## 8. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Ownership / mint | Low to medium risk | Role-based mint capability |
| Supply integrity | Low risk | Verified supply; merger executed as published |
| Liquidity | Low risk | Deep, established markets |
| Decentralization / narrative | Medium risk | On-chain agent count far below headline |
| Alliance / rebrand | Low to medium risk | ASI incomplete; a member departed |
| Transparency | Low to medium risk | Merger transparent; agent count overstated |

---

## 9. Technical Specifications

| Item | Value |
|------|-------|
| Contract | `0xaea46A60368A7bD060eec7DF8CBa43b7EF41Ad85` |
| Decimals / supply | 18 / 2,714,384,546.672 FET |
| Mint | Role-holder mint capability (not revoked) |
| Merger ratios | AGIX ~0.43335, OCEAN ~0.43323 (July 2024, on-chain) |
| On-chain agent registry | Tens of thousands (vs "millions" marketed) |
| Framework license | uAgents, Apache-2.0 (open source) |

---

## 10. Conclusion

Fetch.ai / FET is the highest-scoring project in this set (77/100, LOW-to-MEDIUM). Its central claims that hold up: a verified, liquid token; an on-chain token merger that executed exactly as published; and a genuinely open-source agent framework. It is a real, established project, not a rug. It is not scored higher because (a) the headline "millions of agents" is an off-chain directory figure roughly two orders of magnitude above the on-chain registry, (b) the ASI rebrand is still incomplete and lost a founding member in 2025, and (c) FET remains mint-capable by privileged roles. Strong project; the caution is marketing scale versus on-chain scale, and residual mint centralization.

---

## 11. Recommendations

**For the Fetch.ai / ASI team:**
- Distinguish off-chain directory agent counts from on-chain registered agents in marketing; publish both.
- Provide clarity on the mint role-holders and any supply-change governance.
- Communicate the ASI alliance's current membership and rebrand status accurately (symbol still FET; membership changed in 2025).

**For users:**
- Treat the "millions of agents" figure as a platform metric, not on-chain scale (on-chain registry is far smaller).
- Note that FET supply is not cryptographically fixed (role-based mint), though the token and merger are verified and real.

---

## 12. Verification

- MEFAI on-chain analysis: reads of the FET contract (identity, total supply 2,714,384,546.672, role-based mint capability, verified source), confirmation that the token merger executed on-chain at the published AGIX/OCEAN ratios, and a read of the on-chain agent registry size versus the marketed figure.
- The contract address, supply, merger transactions and on-chain agent registry are publicly verifiable by anyone on the Ethereum explorers and the project's on-chain registry.
- Project statements: fetch.ai and the alliance's public communications, and the project's open-source uAgents repository (Apache-2.0).
