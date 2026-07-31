# Security Audit Report: Cookie DAO (COOKIE) on BNB Smart Chain

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Cookie DAO (Cookie.fun / Cookie3) |
| **Token Symbol** | COOKIE |
| **Contract (omnichain, same address)** | `0xc0041ef357b183448b235a8Ea73Ce4E4eC8c265F` |
| **Chain** | BNB Smart Chain (primary), also Ethereum and Base |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **60/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Cookie DAO is a real, working analytics product (the Cookie.fun AI agent index) with a verified, capped supply omnichain token and genuine user traction. It is not a scam. The MEDIUM rating reflects a gap between the "decentralized data layer / DAO" branding and a centralized company reality:

1. **It is a centralized analytics company with a token attached.** The data collection, indexing, agent curation, the API and the frontend are all built and run by a for profit company; there is no evidence of decentralized data provision, on chain compute or verifiable data.
2. **"Index and data layer for all AI agents" describes a curated subset.** Coverage is a company selected set of roughly 1,250 plus agents, and the headline "mindshare" metric reduces to the percentage of conversation about a token on one social platform, a share of voice figure, not verifiable on chain data.
3. **"DAO / decentralized governance" overstates the reality**; token voting over the actual data pipeline is not demonstrated.
4. A naming and contract ambiguity exists (several older Cookie3 token contracts plus the current omnichain contract deployed at the same address on three chains) that can confuse holders.

The product and traction are real; the caution is the centralized SaaS reality behind the "decentralized data layer / DAO" framing.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | Cookie / COOKIE |
| **Contract (canonical, omnichain)** | `0xc0041ef357b183448b235a8Ea73Ce4E4eC8c265F` (same address on BNB Smart Chain, Ethereum, Base) |
| **Decimals** | 18 |
| **Max supply** | 1,000,000,000 COOKIE (cap) |
| **On chain supply (verified)** | ~772 million on BNB, ~228 million on Base, 0 on Ethereum (omnichain distribution) |
| **Legacy contracts (do not confuse)** | Several legacy Cookie3 contracts exist, e.g. `0xA90693857b4fE6019512FE80Bb6fA023548Ce920` |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified the COOKIE token:

| Check | Result |
|-------|--------|
| Token identity | "Cookie", symbol COOKIE, 18 decimals, verified |
| Contract | LayerZero OFT (omnichain), same address on BNB, Ethereum, Base |
| Primary chain | BNB Smart Chain (origin/primary) |
| On chain supply | ~772 million on BNB, ~228 million on Base, 0 on the Ethereum deployment |
| Max supply | 1,000,000,000 COOKIE (cap) |
| Legacy contracts | Several older Cookie3 contracts exist; the canonical token is the newer omnichain contract |

**Interpretation.** COOKIE is a genuine, verified, capped supply omnichain token. The main on chain caution is the naming and contract ambiguity: several legacy contracts plus the current omnichain contract (identical address across three chains) can confuse holders and auditors, so the canonical address must be checked.

---

## 3. Claim vs Reality: "Decentralized Data Layer for AI Agents"

> Site: "Modular Data Layer for AI Agents"; "the index and data layer for all AI agents"; "operates on a decentralized governance model where token holders gain proportional voting rights."

**Reality: a centralized, company hosted analytics dashboard and API.** The product is built and run by a for profit web3 analytics company with a named executive team. Data collection, indexing, which agents are included and ranked, the API and the frontend are **all company operated**. Even sympathetic descriptions call it "a centralized hub." There is no evidence of decentralized or permissionless data provision, on chain compute, or verifiable data. The "layer" is a centralized backend, and the "DAO / decentralized governance" branding overstates on chain control, token voting over the actual data pipeline is not demonstrated.

---

## 4. Claim vs Reality: "Index for All Agents" and "Mindshare"

- **Curated subset:** coverage is a **company selected set of roughly 1,250 plus agents**; inclusion, ranking and the "mindshare" methodology are operator controlled and opaque, not "all AI agents."
- **"Mindshare" is a social metric:** it reduces to the **percentage of conversation about a token on one social platform**, a share of voice figure, not verifiable on chain data. It is presented alongside "on chain data" in a way that can imply more rigor than a social sentiment share.
- **Token role:** the analytics value accrues to the company; COOKIE's concrete role is token gating and API access plus governance and rewards, a useful product with a bolted on token.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| COOKIE 001 | **MEDIUM** | Centralized analytics company product marketed as a "decentralized data layer" with a "DAO / decentralized governance model"; data pipeline and API company controlled. |
| COOKIE 002 | **MEDIUM** | "Index for all AI agents" is a curated, operator controlled subset (~1,250 plus); "mindshare" is a social share of voice metric, not on chain data. |
| COOKIE 003 | **LOW** | Naming and contract ambiguity: several legacy contracts plus an omnichain contract at the same address on three chains can confuse holders. |
| COOKIE 004 | **LOW** | Token value accrues to the company; COOKIE is a token gate and access token bolted onto a SaaS. |
| COOKIE 005 | **INFO** | Capped 1 billion supply, verified omnichain token (positive). |
| COOKIE 006 | **INFO** | Real, used analytics product with genuine traction (positive). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, capped, real product |
| Supply / minting | Low risk | Capped 1 billion |
| Decentralization | Medium risk | Centralized SaaS, "DAO" overclaim |
| Claim accuracy | Medium risk | Curated subset; "mindshare" is social, not on chain |
| Contract clarity | Low to medium risk | Legacy plus omnichain address ambiguity |
| Transparency | Low to medium risk | Data layer framing over a hosted backend |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Contract (canonical) | `0xc0041ef357b183448b235a8Ea73Ce4E4eC8c265F` (BNB, Ethereum, Base) |
| Decimals | 18 |
| Max supply | 1,000,000,000 COOKIE |
| On chain supply | ~772 million BNB, ~228 million Base, 0 Ethereum |
| Legacy contracts | `0xA90693857b4fE6019512FE80Bb6fA023548Ce920` (one of several) |

---

## 8. Conclusion

Cookie DAO is a real, working analytics product with a verified, capped supply omnichain token and genuine user traction, which keeps it in the MEDIUM band at 60/100. It is held back because it is a centralized analytics company with a token attached, marketed as a "decentralized data layer" with a "DAO / decentralized governance model" it does not demonstrate; its "index for all AI agents" is a curated, operator controlled subset; and its headline "mindshare" is a social share of voice metric rather than verifiable on chain data. The product is real; the caution is the centralized reality behind the decentralized data layer framing, plus the legacy/omnichain contract ambiguity.

---

## 9. Recommendations

**For the Cookie DAO team:**
- Describe the product accurately as a centralized analytics service with a token for access and governance, rather than a "decentralized data layer."
- Disclose the agent inclusion and "mindshare" methodology, and label social share of voice separately from on chain data.
- Prominently document the canonical omnichain contract to prevent confusion with the legacy contract.

**For users:**
- Verify the canonical contract (`0xc0041ef357b183448b235a8Ea73Ce4E4eC8c265F`), not the legacy one.
- Treat "mindshare" as social sentiment share, not on chain data, and the platform as a centralized service.

---

## 10. Verification

- MEFAI on chain analysis: reads of the COOKIE token on BNB Smart Chain and Ethereum (identity, 18 decimals, ~772 million on BNB, the omnichain same address deployment) and confirmation of the capped 1 billion supply and the legacy contract.
- The contract addresses, supply and multi chain deployment are publicly verifiable on the BNB Smart Chain, Ethereum and Base explorers.
- Project statements: the project's own pages (the "Modular Data Layer for AI Agents", "index and data layer for all AI agents", and "decentralized governance model" wording, and the "mindshare" definition).
