# Security Audit Report: Golem (GLM) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 1, 2026 |
| **Project** | Golem |
| **Token Symbol** | GLM |
| **Contract (Ethereum)** | `0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429` |
| **Chain** | Ethereum (also a bridged representation on Polygon) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **64/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Golem (GLM) is one of the oldest decentralized compute projects, with real open source software, a clean fixed cap token, and a long track record. It is not a scam. The MEDIUM rating reflects modest usage and weak product market fit behind a legitimate project:

1. **The token is clean and non inflationary:** MEFAI confirms an on chain total of roughly 796 million GLM against a fixed combined cap of about 1 billion, with new GLM only appearing through the one to one migration from the older GNT, so there is no discretionary inflation.
2. **The software and network are real,** with permissionless provider onboarding and a genuine provider and requestor compute marketplace running for years.
3. **But usage is modest and arguably declining in relative terms,** and headline utilization, fee and partnership figures are self reported and small relative to the valuation.
4. **After nearly a decade the project has not reached breakout adoption,** and the pivot toward GPU and AI compute is early.

The software, track record and clean supply are real strengths; the caution is modest usage, a valuation disconnected from realized demand, and a deep drawdown.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | Golem Network Token / GLM |
| **Contract (Ethereum, verified)** | `0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429` |
| **Decimals** | 18 |
| **Supply** | ~796 million GLM on the contract; a fixed combined GNT and GLM cap of about 1 billion |
| **Emission** | Non inflationary: new GLM only appears via the one to one migration from the older GNT (a mint bound to that migration, capped at the original supply) |
| **Governance** | Founder and foundation led (Golem Factory), not a broad DAO |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified GLM on Ethereum:

| Check | Result |
|-------|--------|
| Token identity | "Golem Network Token", symbol GLM, 18 decimals, verified |
| Supply | ~796 million GLM on chain (the remainder of the ~1 billion cap is un migrated GNT) |
| Mint | A mint function exists but is bound to the one to one GNT migration (capped at the original supply), so there is no discretionary inflation |
| Cap | A fixed combined GNT and GLM cap of about 1 billion (non inflationary) |
| Onboarding | Permissionless provider onboarding (anyone can run a provider node) |
| Governance | Steered by the founding foundation (Golem Factory), a directional centralization |

**Interpretation.** GLM is a clean, effectively non inflationable token (the mint is migration bound, not discretionary), and the network is real and permissionless, genuine strengths. The cautions are fundamental: modest usage, a founder and foundation led direction, and a valuation disconnected from realized demand.

---

## 3. Claim vs Reality: "A Worldwide Supercomputer"

> Site: a worldwide supercomputer, a permissionless decentralized marketplace for CPU and GPU compute connecting providers and requestors, one of the oldest decentralized compute projects.

**Reality: real software, modest usage.** Golem is one of the oldest surviving decentralized compute projects, with real, open source software shipped across multiple iterations and genuinely permissionless provider onboarding, which distinguishes it from typical projects. But realized usage is modest: neutral coverage repeatedly describes a gap between the utility metrics and the market valuation, headline utilization, weekly fee and partnership figures are self reported and small in absolute terms, and the most economically real use case remains a narrow niche (rendering and some proof generation). After nearly a decade the network has not reached breakout adoption, and the current pivot to GPU and AI compute is early rather than a proven revenue engine.

---

## 4. Claim vs Reality: Supply, Usage and Value

- **Clean, fixed cap supply (positive):** the combined GNT and GLM cap is about 1 billion and is not discretionarily mintable, so there is no dilution or emission risk; the ~796 million on chain reflects un migrated GNT.
- **Modest realized usage:** transacted compute is small relative to the valuation, a common pattern where price outruns realized demand.
- **Directional centralization:** the roadmap is founder and foundation led (Golem Factory), not a decentralized DAO, though this is control of direction, not of supply.
- **Value:** the token is down roughly 93 percent from its 2018 peak across two full market cycles.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| GLM 001 | **MEDIUM** | Valuation disconnected from realized usage; headline utilization, fee and partnership figures are self reported and small in absolute terms. |
| GLM 002 | **MEDIUM** | No breakout adoption after nearly a decade; the GPU and AI compute pivot is early. |
| GLM 003 | **LOW** | Competitive erosion from newer, better funded GPU compute networks. |
| GLM 004 | **LOW** | Deep drawdown (~93 percent from the 2018 peak) and thin liquidity. |
| GLM 005 | **INFO** | Clean, fixed combined cap of about 1 billion with a migration bound mint (non inflationary), real open source software and permissionless onboarding; founder and foundation led direction. |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, clean, fixed cap |
| Supply / minting | Low risk | Migration bound mint, no discretionary inflation |
| Usage reality | Medium risk | Modest, self reported usage vs valuation |
| Decentralization | Low to medium risk | Founder and foundation led direction; permissionless onboarding |
| Product reality | Medium risk | Real software, no breakout after nearly a decade |
| Value / volatility | Medium to high risk | ~93 percent drawdown, thin liquidity |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Contract (Ethereum) | `0x7DD9c5Cba05E151C895FDe1CF355C9A1D5DA6429` |
| Decimals | 18 |
| Supply | ~796 million on chain; ~1 billion combined GNT and GLM cap (fixed) |
| Emission | Non inflationary (migration bound mint) |
| Governance | Golem Factory led |

---

## 8. Conclusion

Golem (GLM) is one of the oldest decentralized compute projects, with real open source software, a clean fixed cap and effectively non inflationable token, and permissionless provider onboarding, which keeps it in the MEDIUM band at 64/100. It is held back because realized usage is modest and its valuation is disconnected from demonstrable demand, because headline utilization and fee figures are self reported and small, because it has not reached breakout adoption after nearly a decade and the GPU and AI pivot is early, and because it is down roughly 93 percent from its 2018 peak. The software, track record and clean supply are real strengths; the caution is modest usage, a valuation ahead of realized demand, and the drawdown.

---

## 9. Recommendations

**For the Golem team:**
- Publish verifiable, independently reproducible network usage, fee and provider figures, so the utility is grounded in data rather than self reported.
- Deliver and demonstrate real paid GPU and AI compute demand, not incentive driven activity.
- Present the fixed, non inflationable supply as the genuine strength it is.

**For users:**
- Value the real software, long track record and clean supply on their merits.
- Understand realized usage is modest relative to the valuation, that direction is foundation led, and model the drawdown.

---

## 10. Verification

- MEFAI on chain analysis: a direct read of the GLM token on Ethereum (identity, 18 decimals, the ~796 million on chain supply, and the migration bound mint against a fixed ~1 billion combined cap) and review of the provider and requestor marketplace and the foundation led governance.
- The contract address and supply are publicly verifiable on the Ethereum explorers.
- Project statements: the project's own pages (the worldwide supercomputer and decentralized compute framing) and the published tokenomics and migration mechanics.
