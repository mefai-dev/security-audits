# Security Audit Report: Arkham (ARKM) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Arkham Intelligence |
| **Token Symbol** | ARKM |
| **Contract (Ethereum)** | `0x6E2a43be0B1d33b726f0CA3b8de60b3482b8b050` |
| **Chain** | Ethereum |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **54/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Arkham is a real, funded on chain intelligence company with a working product and a verified, fixed supply token. It is not a scam. The MEDIUM rating reflects a mismatch between the "deanonymizing the blockchain, source of truth" branding and the centralized, probabilistic, insider heavy reality:

1. **The product is a centralized corporate service, not a decentralized protocol.** The entity labels, the Ultra AI engine and the Intel Exchange are proprietary and company controlled; a "Foundation treasury" and token incentives dress a fully centralized SaaS.
2. **"Source of truth" overstates probabilistic attribution.** Arkham's entity labels are inferred (pattern, timing and funding heuristics), not verified identity, so misattribution risk falls on labeled parties.
3. **The token is genuinely fixed at 1 billion (no emissions), but roughly 58 percent sits with insiders and the foundation** on multi year unlocks, with a large unlock event ahead.
4. Reputational flags: the "dox to earn" Intel Exchange drew significant privacy criticism, the company reportedly exposed some of its own users' emails in a 2023 referral promotion (an issue it subsequently addressed), and it has moved its own tokens into centralized custody, obscuring the very activity it monetizes exposing in others.

The company and product are real; the caution is centralization, attribution overreach and insider overhang.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | Arkham / ARKM |
| **Contract (Ethereum, verified)** | `0x6E2a43be0B1d33b726f0CA3b8de60b3482b8b050` |
| **Decimals** | 18 |
| **Total supply (verified)** | 1,000,000,000 ARKM (fixed, no emissions) |
| **Circulating** | ~635 million ARKM (on chain unlocked) |
| **Allocation** | Ecosystem ~37.3 percent; core contributors 20 percent; investors 17.5 percent; foundation treasury 17.2 percent; launchpad 5 percent; advisors 3 percent |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified the ARKM token on Ethereum:

| Check | Result |
|-------|--------|
| Token identity | "Arkham", symbol ARKM, 18 decimals, verified |
| Total supply | 1,000,000,000 ARKM (fixed, no planned emissions) |
| Circulating | ~635 million ARKM (rest locked) |
| Insider + foundation allocation | ~57.7 percent (team + investors + advisors + foundation) |
| Unlock overhang | A large linear unlock tranche vesting around September 2026 (not a single cliff) |

**Interpretation.** ARKM is a genuine, fixed supply, non inflationary token, a real positive. The concern is that a majority of supply sits with insiders and the foundation on multi year unlocks, with a large linear 2026 unlock tranche, so the "fixed supply" framing understates the sell side overhang.

---

## 3. Claim vs Reality: "Deanonymizing the Blockchain" / Source of Truth

> Site: "Deanonymizing the blockchain"; the Ultra AI engine synthesizes data into "a single, scalable, amendable source of truth."

**Reality: probabilistic attribution, not verified identity.** Arkham's entity labels are **inferred** by heuristically matching addresses to real world entities through transaction patterns, timing, funding sources and cross chain activity, marketed as a "source of truth" with far more certainty than a heuristic clustering engine warrants. Misattribution risk falls on the labeled parties. The platform is a real, capable product, but "source of truth" overstates inference as fact.

---

## 4. Claim vs Reality: Decentralization and Conduct

- **Centralized corporate product:** Arkham is a company; it unilaterally controls the labels, the Ultra engine and the Intel Exchange rules. A "Foundation treasury" (17.2 percent) and token incentive language do not make the product decentralized.
- **"Dox to earn" criticism:** the Intel Exchange bounty marketplace, which pays users to identify wallet owners, drew significant privacy criticism as enabling smear campaigns and safety risk.
- **Own conduct contradictions:** in 2023 the company reportedly exposed some of its own users' email addresses through a referral promotion (an issue it subsequently addressed), and it has moved its own ARKM into centralized custody, obscuring the very on chain activity it monetizes exposing in others.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| ARKM 001 | **MEDIUM** | Centralized corporate product marketed with decentralization optics (foundation treasury, token incentives); labels, AI and Intel Exchange company controlled. |
| ARKM 002 | **MEDIUM** | "Source of truth" overstates probabilistic attribution (heuristic clustering), with misattribution risk on labeled parties. |
| ARKM 003 | **MEDIUM** | ~57.7 percent insider and foundation allocation on multi year unlocks; a large linear unlock tranche around September 2026. |
| ARKM 004 | **LOW** | Conduct flags: a reported 2023 exposure of some user emails (since addressed); moving its own tokens into centralized custody. |
| ARKM 005 | **INFO** | Fixed 1 billion supply, no emissions (positive on the supply mechanic). |
| ARKM 006 | **INFO** | Real, funded company with a working product (context). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, fixed supply, real company |
| Supply / minting | Low to medium risk | Fixed, but ~58 percent insider unlock overhang |
| Decentralization | Medium to high risk | Fully centralized corporate SaaS |
| Claim accuracy | Medium risk | "Source of truth" vs probabilistic attribution |
| Conduct / reputation | Medium risk | Dox to earn criticism; own data exposure |
| Transparency | Medium risk | Decentralization optics over a centralized product |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Contract (Ethereum) | `0x6E2a43be0B1d33b726f0CA3b8de60b3482b8b050` |
| Decimals | 18 |
| Total supply | 1,000,000,000 ARKM (fixed) |
| Circulating | ~635 million ARKM (on chain unlocked) |
| Insider + foundation | ~57.7 percent |

---

## 8. Conclusion

Arkham is a real, funded on chain intelligence company with a working product and a verified, fixed supply token, so it is not a scam. It scores 54/100 (MEDIUM) because the "deanonymizing the blockchain, source of truth" branding overstates a centralized, probabilistic and insider heavy reality: entity labels are inferred attributions marketed as fact, the product is a fully centralized corporate service dressed in decentralization optics, roughly 58 percent of supply sits with insiders and the foundation with a large linear 2026 unlock tranche, and the company's own conduct (a reported 2023 user email exposure since addressed, moving its own tokens into centralized custody) sits awkwardly against its privacy piercing pitch.

---

## 9. Recommendations

**For the Arkham team:**
- Present entity labels as probabilistic attributions with confidence and correction mechanisms, not a "source of truth."
- Be explicit that Arkham is a centralized company product; disclose the foundation's role and the forward unlock schedule.
- Address the conduct flags (data exposure, own token custody) transparently.

**For users:**
- Treat Arkham labels as inferences, not verified identity.
- Model the ~58 percent insider and foundation overhang and the large 2026 unlock.

---

## 10. Verification

- MEFAI on chain analysis: a read of the ARKM token on Ethereum (identity, 18 decimals, fixed 1 billion supply, no emissions) and review of the allocation and unlock schedule.
- The contract address, supply and allocation are publicly verifiable on the Ethereum explorers.
- Project statements: the project's own pages (the "deanonymizing the blockchain", "source of truth" and Ultra AI wording, and the published tokenomics), and the public record of the Intel Exchange criticism, the reported user email exposure and the token custody reporting.
