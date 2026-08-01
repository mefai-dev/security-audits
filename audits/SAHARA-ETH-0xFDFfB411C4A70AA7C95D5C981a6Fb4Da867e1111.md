# Security Audit Report: Sahara AI (SAHARA) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 1, 2026 |
| **Project** | Sahara AI |
| **Token Symbol** | SAHARA |
| **Contract (Ethereum)** | `0xFDFfB411C4A70AA7C95D5C981a6Fb4Da867e1111` |
| **Also on** | BNB Smart Chain (same address) |
| **Chain** | Ethereum (and BNB Smart Chain) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **58/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Sahara AI is a real, well backed AI project with a verified token, but its token carries centralized controls and heavy unlock overhang. It is not a scam. The MEDIUM rating reflects the gap between the "decentralized AI blockchain" branding and a controlled, early stage reality:

1. **The token is verified:** MEFAI confirms a verified SAHARA token on Ethereum and BNB Smart Chain with a fixed 10 billion supply.
2. **But it has centralized controls.** MEFAI's read shows the token has an owner (a multisig or timelock contract) and a pause function, so transfers can be paused and the owner holds privileged control, which is not what "decentralized" implies.
3. **The decentralized AI blockchain is early while the real business is data services.** The demonstrated activity is data annotation and labeling and a data and model marketplace, with the fully decentralized AI blockchain still early.
4. **A moderate float with a large locked majority,** heavy multi year unlocks and a drawdown from the launch peak temper the picture.

The backing and product are real; the caution is centralized token controls, an aspirational decentralized AI narrative and unlock overhang.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | Sahara AI / SAHARA |
| **Contract (Ethereum, verified)** | `0xFDFfB411C4A70AA7C95D5C981a6Fb4Da867e1111` (same address on BNB Smart Chain) |
| **Decimals** | 18 |
| **Total supply** | 10,000,000,000 SAHARA |
| **Owner** | An owner (a multisig contract) with privileged control |
| **Pause** | The token has a pause function (transfers can be paused); it has no mint function, so supply cannot be inflated |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified the SAHARA token on Ethereum and BNB Smart Chain:

| Check | Result |
|-------|--------|
| Token identity | "Sahara AI", symbol SAHARA, 18 decimals, verified |
| Total supply | 10,000,000,000 SAHARA (same on both chains) |
| Owner | An owner exists (a multisig contract, not an anonymous single key) with privileged control |
| Pause | A pause function is present; the token is not currently paused |
| Mintable | No mint function in the token code; the 10 billion supply cannot be inflated (a positive) |

**Interpretation.** SAHARA is a verified token, but unlike a fully decentralized token it retains an owner and a pause function, so the owner can pause transfers and holds privileged control. That the owner is a multisig (rather than a plain single key) and that the token has no mint function so supply cannot be inflated are mitigants, but the centralized pause and owner controls are a real caution against the decentralized framing.

---

## 3. Claim vs Reality: "A Decentralized AI Blockchain and Economy"

> Site: a decentralized AI blockchain and collaborative AI economy, with data annotation and labeling, an AI model and dataset marketplace, and provenance and attribution for AI.

**Reality: real data services, an early decentralized AI blockchain.** The demonstrated business is data services (data annotation and labeling) and a data and model marketplace, which are real and generate activity. The fully decentralized AI blockchain and the collaborative on chain AI economy are still early, so the "decentralized AI blockchain" framing runs ahead of the demonstrated substance, which is closer to an AI data services company with a token and a developing chain.

---

## 4. Claim vs Reality: Float, Unlocks and Value

- **Moderate float, heavy locked majority:** roughly 35.8 percent of the 10 billion supply is circulating with a large majority still locked, with big allocations to insiders and the ecosystem on multi year vesting running to 2029, so sell side overhang is significant.
- **Centralized controls:** the owner and pause function mean the token is not trustless; holders rely on the owner not to pause transfers or misuse privileged control (the supply itself cannot be inflated, as there is no mint function).
- **Backing (positive):** the project is genuinely backed by well known venture and strategic investors, a real credibility signal.
- **Value:** the token is down roughly 95 percent from its launch peak (an all time high in mid 2025).

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| SAHARA 001 | **MEDIUM** | The token has an owner (a multisig or timelock) and a pause function, so transfers can be paused and the owner holds privileged control, against the decentralized framing. |
| SAHARA 002 | **MEDIUM** | The decentralized AI blockchain is early while the demonstrated business is data annotation and a data and model marketplace. |
| SAHARA 003 | **MEDIUM** | Moderate float (~35.8 percent) with a large locked majority and heavy insider and ecosystem unlocks on multi year vesting to 2029 (sell side overhang). |
| SAHARA 004 | **LOW** | ~95 percent drawdown from the launch peak. |
| SAHARA 005 | **INFO** | Verified 10 billion supply token with no mint function (supply cannot be inflated) on Ethereum and BNB Smart Chain; genuine venture backing (positives). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, well backed |
| Contract controls | Medium risk | Owner plus pause function |
| Decentralization | Medium risk | Decentralized AI blockchain early |
| Supply / unlocks | Medium risk | Moderate float, heavy multi year unlocks, non mintable |
| Claim accuracy | Medium risk | Decentralized framing over data services |
| Value / volatility | High risk | Drawdown from the launch peak |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Contract (Ethereum) | `0xFDFfB411C4A70AA7C95D5C981a6Fb4Da867e1111` |
| Decimals | 18 |
| Total supply | 10,000,000,000 SAHARA |
| Owner / pause | Owner (multisig) present; pause function present; no mint function |
| Also on | BNB Smart Chain (same address) |

---

## 8. Conclusion

Sahara AI is a real, well backed AI project with a verified 10 billion supply token, which keeps it in the MEDIUM band at 58/100. It is held back because the token retains centralized controls (an owner and a pause function, so transfers can be paused and the owner holds privileged control), because the fully decentralized AI blockchain is early while the demonstrated business is data annotation and a data and model marketplace, and because a large locked majority with heavy insider and ecosystem unlocks to 2029 creates sell side overhang, against a roughly 95 percent drawdown from the launch peak. The backing and product are real; the caution is centralized token controls, an aspirational decentralized AI narrative and unlock overhang.

---

## 9. Recommendations

**For the Sahara AI team:**
- Disclose the owner and pause capabilities clearly, and set out a path to remove or constrain them so the decentralized framing is accurate.
- Distinguish the demonstrated data services business from the early decentralized AI blockchain in messaging.
- Present the unlock schedule and low float prominently.

**For users:**
- Understand the token has an owner and a pause function, so it is not trustless, and note the demonstrated business is data services.
- Model the low float, the heavy multi year unlocks and the drawdown.

---

## 10. Verification

- MEFAI on chain analysis: a direct read of the SAHARA token on Ethereum and BNB Smart Chain (identity, 18 decimals, 10 billion supply, the presence of an owner and a pause function).
- The contract address, supply and controls are publicly verifiable on the Ethereum and BNB Smart Chain explorers.
- Project statements: the project's own pages (the decentralized AI blockchain, data services and marketplace framing) and the published tokenomics and unlock schedule.
