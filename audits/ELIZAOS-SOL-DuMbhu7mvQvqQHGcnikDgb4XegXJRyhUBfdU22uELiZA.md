# Security Audit Report: ElizaOS (ELIZA / ai16z) - Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | ElizaOS (formerly ai16z) |
| **Flagship Token (elizaOS)** | `DuMbhu7mvQvqQHGcnikDgb4XegXJRyhUBfdU22uELiZA` |
| **Legacy Token (ai16z)** | `HeLp6NuQkmYB4pYWo2zYs22mESHXPQYzXbB8n4V98jwC` |
| **Separate memecoin (ELIZA)** | `5voS9evDjxF589WuEub5i4ti7FWQmZCsAsyD5ucbuRqM` (do not confuse) |
| **Chain** | Solana |
| **Audit Type** | Token + Project (Claim vs Reality) |
| **Mefai Security Score** | **30/100** |
| **Overall Risk** | **HIGH** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research. It represents a point in time analysis based on public information, the project's own published statements, and onchain data verified through MEFAI's on-chain analysis. The lines quoting the project quote its own public marketing statements; the assessments beside them are Mefai Security Research's analysis and opinion, not statements about the private intentions of any team. Findings drawn from litigation are attributed and treated as unproven allegations, not court findings. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Executive Summary

ElizaOS presents a genuine strength and a detached token in the same package. The open source agent framework (elizaOS/eliza) is real, permissively licensed and heavily used, and it deserves full credit. The **token**, however, is largely detached speculation:

1. The flagship narrative, an "autonomous AI venture fund" operated by an AI persona ("pmairca" / "Marc AIndreessen"), was publicly reported and is alleged in active litigation to have been **human operated**, not autonomous.
2. MEFAI's on-chain analysis confirms the elizaOS mint authority is **active and controlled by a single key** (a 1 of 2 multisig where one signer suffices), so the advertised "hard cap" is a policy promise, not a protocol enforced limit.
3. The token has collapsed roughly **99.9 percent** from a peak near 2.6 billion dollars to about 3 million dollars, on razor thin liquidity (~15 thousand dollars for elizaOS).
4. The token **gates nothing** in the actual software: the framework is free and open source, so the token is not required to build or run agents.

The HIGH rating reflects a token whose core value proposition (autonomy, a capped ecosystem asset) is not supported by the on-chain and public evidence, offset only slightly by the genuine, credited open source framework.

---

## 1. Contract Overview

MEFAI's on-chain analysis identified three distinct on-chain identities in this lineage. They must not be confused.

| Token | Mint address | Notes |
|-------|--------------|-------|
| **elizaOS** (flagship, formerly ai16z) | `DuMbhu7mvQvqQHGcnikDgb4XegXJRyhUBfdU22uELiZA` | Standard SPL, 9 decimals. This is the migration target token. |
| **ai16z** (legacy) | `HeLp6NuQkmYB4pYWo2zYs22mESHXPQYzXbB8n4V98jwC` | Token-2022 with embedded metadata (name/symbol "ai16z"), being deprecated. |
| **ELIZA** (unrelated memecoin) | `5voS9evDjxF589WuEub5i4ti7FWQmZCsAsyD5ucbuRqM` | A separate pump.fun memecoin (name "Eliza"), not the DAO/ecosystem token. |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana reads (`getAccountInfo`, `getTokenSupply`) returned:

| Token | Freeze authority | Mint authority | Supply |
|-------|------------------|----------------|--------|
| elizaOS `DuMbhu...LiZA` | `null` (revoked, good) | **ACTIVE** = `D4MYCaoyT5XZFBke16JwaNJa6TWDeCTuZMYErukMGerU` (a **1 of 2 multisig**, one signer suffices) | ~9.33 billion |
| ai16z `HeLp6...jwC` | `null` (revoked) | Active, held by a program owned account (better custody than a raw key) | ~1.1 billion |
| ELIZA `5voS...RqM` | n/a | revoked | ~999.9 million |

**Interpretation.** The freeze authority is revoked on both DAO tokens (users cannot be frozen, which is good). But the flagship elizaOS mint authority is **active** and requires only a single signature to mint. New elizaOS can be created toward (and the mechanism does not on its own stop at) the advertised cap. This is a supply/inflation risk that the "hard cap, no emissions" marketing does not disclose.

---

## 3. The "Hard Cap" Claim vs Reality

> Marketing: "Hard cap of 11,000,000,000 tokens with no emissions."

**Reality (MEFAI on-chain analysis):** the elizaOS mint authority is active and is a 1 of 2 multisig, so a single key can mint. Roughly 9.33 billion of the 11 billion cap is minted so far, and nothing at the protocol level enforces the cap; it is a stated policy. A "hard cap" that depends on an active human controlled mint key is a promise, not a guarantee.

---

## 4. The Autonomous AI Fund Claim vs Reality

The lineage's flagship narrative was an autonomous AI venture fund persona ("pmairca" / "Marc AIndreessen").

**Reality:** it was publicly reported (October 2024) that no fund of this type on the relevant launchpad is actually operated by an AI, and that the assets were the human creator's own token. This is further the subject of an active class action, **Pikabea v. Walters et al., U.S. District Court for the Southern District of New York, case number 1:26-cv-03238, filed April 20, 2026**, which alleges that humans actually operated the AI agent and that it was not autonomous as advertised. **These are reporting and unproven allegations, not court findings.** MEFAI takes no position on the ultimate legal outcome; the point for a trader is that the "autonomous" premise is contested by both public reporting and a live lawsuit.

---

## 5. The "ai16z" Branding

The project originally used the name "ai16z", echoing the venture firm Andreessen Horowitz (a16z). Following a request from an a16z partner, the project rebranded to **elizaOS on January 28, 2025**. The class action additionally alleges the "ai16z" name and a "Marc AIndreessen" agent were built to mimic a16z without authorization (again, an unproven allegation).

---

## 6. Token Utility

> Marketing: the token is the flagship ecosystem asset with governance and staking utility.

**Reality:** the framework's own quickstart runs agents with a few CLI commands, with **no token purchase or payment required**. The framework is permissively (MIT) licensed and its plugins are free packages. The token therefore gates nothing in the real software; governance historically ran off-chain. This makes the token an economic/speculative instrument rather than a required protocol asset.

---

## 7. Value and Liquidity

- All time high market cap: approximately **2.6 billion dollars** (early January 2025).
- Current combined lineage (elizaOS plus legacy ai16z): approximately **3 million dollars** = a decline of about **99.9 percent**.
- On-chain liquidity: approximately **15 thousand dollars** for elizaOS (about 41 thousand dollars combined with legacy ai16z).

An asset that has lost 99.9 percent of its peak value on near-zero on-chain liquidity carries severe exit risk regardless of the framework's technical merit.

---

## 8. Positive Findings (Credited)

- **The open source framework is real and excellent.** The public `elizaOS/eliza` repository is genuinely open source (permissive MIT license), large (tens of thousands of stars, thousands of forks) and actively developed (daily commits at the time of review). An academic paper describing the architecture also exists. This is a legitimate contribution and is why the score is 30 rather than near zero.
- **Freeze authority is revoked** on both DAO tokens.

---

## 9. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| ELIZ-001 | **HIGH** | Flagship "autonomous AI fund" premise is contested by public reporting and an active class action alleging human operation (unproven allegations). |
| ELIZ-002 | **HIGH** | elizaOS mint authority is active and single-signer capable; the advertised hard cap is not protocol enforced. |
| ELIZ-003 | **HIGH** | ~99.9 percent value collapse on ~15 thousand dollars of on-chain liquidity: severe exit risk. |
| ELIZ-004 | **MEDIUM** | Token gates nothing in the free, open source software; utility is weak and detached. |
| ELIZ-005 | **MEDIUM** | Three separate on-chain tokens share the Eliza brand, creating impersonation and confusion risk. |
| ELIZ-006 | **LOW** | "ai16z" branding required an enforced rebrand after an a16z request. |
| ELIZ-007 | **INFO** | Genuine, actively maintained, permissively licensed open source framework (positive). |
| ELIZ-008 | **INFO** | Freeze authority revoked on both DAO tokens (positive). |

---

## 10. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Ownership control | Medium risk | Freeze revoked; mint authority active |
| Supply / minting | High risk | Active 1 of 2 mint authority; cap not enforced |
| Liquidity security | High risk | ~15k on-chain, ~99.9 percent collapse |
| Code safety | Low risk | Real, open source framework |
| Narrative integrity | High risk | Autonomy contested by reporting and litigation |
| Transparency | Medium risk | Undisclosed insider allocation (alleged), token/brand confusion |

---

## 11. Technical Specifications

| Item | Value |
|------|-------|
| elizaOS mint | `DuMbhu7mvQvqQHGcnikDgb4XegXJRyhUBfdU22uELiZA` (9 decimals, ~9.33B) |
| elizaOS mint authority | `D4MYCaoyT5XZFBke16JwaNJa6TWDeCTuZMYErukMGerU` (1 of 2 multisig) |
| Legacy ai16z mint | `HeLp6NuQkmYB4pYWo2zYs22mESHXPQYzXbB8n4V98jwC` (~1.1B) |
| Migration | 1 ai16z to 6 elizaOS; supply expanded toward an 11B cap |
| ATH / current | ~2.6B / ~3M dollars (~99.9 percent decline) |
| On-chain liquidity | ~15k (elizaOS) / ~41k (combined) |

---

## 12. Conclusion

ElizaOS is unusual: a genuinely excellent open source framework attached to a token whose headline claims do not survive scrutiny. The autonomy story is contested by public reporting and active litigation, the "hard cap" is undermined by an active single-signer mint authority, and the market value has collapsed ~99.9 percent on near-zero on-chain liquidity while the token gates nothing in the free software. A score of 30/100 (HIGH) reflects a high risk token, credited only by the real framework behind it.

---

## 13. Recommendations

**For the ElizaOS team:**
- Revoke or move the elizaOS mint authority to a timelocked, multi-party structure, or enforce the cap on-chain, so "hard cap, no emissions" is provable.
- Clearly separate the open source framework (which stands on its own) from the token's marketing, and stop implying token-gated access the software does not require.
- Publish the exact team/insider allocation percentages to address the dilution allegations.
- Provide an official token registry so the three Eliza-branded tokens cannot be confused.

**For users:**
- Treat the elizaOS token as mint capable and understand the cap is not enforced on-chain.
- Confirm the exact mint address before trading; a separate ELIZA memecoin exists.
- Size positions for ~15 thousand dollars of on-chain exit liquidity.
- Separate the framework's merit from the token's speculative profile; the software does not require the token.

---

## 14. Verification

- MEFAI on-chain analysis: direct Solana reads of the mint accounts and authorities (`getAccountInfo`, `getTokenSupply`, multisig resolution) for the three addresses above.
- Contract and authority addresses are publicly verifiable by anyone on the Solana explorers.
- Project statements: elizaos.ai and the project's own documentation and the public `elizaOS/eliza` repository.
- Litigation: Pikabea v. Walters et al., SDNY, case number 1:26-cv-03238 (a public court record); treated as an unproven allegation.
- The autonomous-fund and rebrand events were publicly reported at the time.
