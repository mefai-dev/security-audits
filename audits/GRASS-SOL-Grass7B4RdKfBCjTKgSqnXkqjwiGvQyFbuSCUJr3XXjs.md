# Security Audit Report: Grass (GRASS) - Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | Grass (Wynd Network) |
| **Token Symbol** | GRASS |
| **Mint** | `Grass7B4RdKfBCjTKgSqnXkqjwiGvQyFbuSCUJr3XXjs` |
| **Chain** | Solana |
| **Audit Type** | Token + Project (Claim vs Reality) |
| **Mefai Security Score** | **58/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on-chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Grass is a real, well-known "DePIN" project (users share residential bandwidth for web-data collection, in exchange for the GRASS token). The token is genuinely traded and the product exists. The MEDIUM rating is driven by two things MEFAI verified on-chain plus one honest admission in the project's own documentation:

1. **The mint authority is still ACTIVE.** MEFAI's Solana read shows the GRASS mint authority is a live single keypair (`31rYartQwHeBMjAe2MgGpffGV57fQY3kug4BDN8tLGqQ`), meaning new supply can be minted. This same address is also the largest holder.
2. **A single keypair controls both minting and the largest balance.** That is a material centralization and dilution risk.
3. The project's marketing leans on "decentralized network", but the project's **own documentation admits the validation/network layer is run by a "singular, centralized entity"** during this phase.

The token is not a honeypot and freeze authority is revoked, but the live mint authority under a single key, which is also the top holder, is the reason for the MEDIUM rating.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token** | GRASS |
| **Mint** | `Grass7B4RdKfBCjTKgSqnXkqjwiGvQyFbuSCUJr3XXjs` |
| **Decimals** | 9 |
| **Freeze authority** | `null` (revoked) |
| **Mint authority** | **ACTIVE** - `31rYartQwHeBMjAe2MgGpffGV57fQY3kug4BDN8tLGqQ` |
| **Notable** | The mint authority address is also the largest holder |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana reads returned:

| Check | Result |
|-------|--------|
| Freeze authority | **Revoked** (`null`) - accounts cannot be frozen |
| Mint authority | **ACTIVE** - single keypair can mint new supply |
| Mint authority address | `31rYartQwHeBMjAe2MgGpffGV57fQY3kug4BDN8tLGqQ` |
| Largest holder | The **same** address as the mint authority (~26 percent of the read balance set at review time) |
| Honeypot / transfer restriction | None observed (freeze revoked) |

**Interpretation.** The good news: freeze authority is revoked, so no one can freeze user accounts. The concern: the mint authority is **not** revoked. A single keypair can issue new GRASS, and that same keypair is the largest holder. This concentrates both dilution power and supply in one place. For a token marketed as "decentralized", a live single-key mint authority is the central finding.

---

## 3. Claim vs Reality: "Decentralized Network"

> Marketing: a "decentralized" web-data / AI-data network powered by its community.

**Reality: partly true, honestly qualified by the project itself.** The user-side bandwidth-sharing network is genuinely distributed across many participants. However, the project's **own documentation acknowledges** that the validation and data-verification layer is, in the current phase, operated by a **"singular, centralized entity"** rather than a permissionless validator set. Node and participant counts are **self-reported** by the project and are not independently verifiable on-chain. So "decentralized" is accurate for the edge (users) but overstated for the core (validation), by the project's own admission.

---

## 4. Supply and Concentration

- The mint authority keypair is also the **largest single holder** (~26 percent of the balances read at review time), combining dilution power with supply concentration.
- Because mint authority is live, the effective maximum supply is not cryptographically fixed; it depends on the keyholder's restraint and any published emission schedule.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| GRASS-001 | **MEDIUM** | Mint authority ACTIVE under a single keypair: new supply can be minted (dilution risk). |
| GRASS-002 | **MEDIUM** | The mint-authority keypair is also the largest holder: combined mint + supply concentration. |
| GRASS-003 | **MEDIUM** | "Decentralized network" overstated; project's own docs admit a "singular, centralized entity" validation layer. |
| GRASS-004 | **LOW** | Node / participant counts are self-reported and not independently verifiable on-chain. |
| GRASS-005 | **INFO** | Freeze authority revoked; no honeypot / transfer restriction (positive). |
| GRASS-006 | **INFO** | Real, widely used bandwidth-sharing product (positive). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Ownership / mint | Medium to high risk | Mint authority ACTIVE (single keypair) |
| Freeze / transfer | Low risk | Freeze revoked |
| Concentration | Medium to high risk | Mint authority = largest holder |
| Decentralization | Medium risk | Centralized validation layer (per own docs) |
| Transparency | Medium risk | Self-reported node counts |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `Grass7B4RdKfBCjTKgSqnXkqjwiGvQyFbuSCUJr3XXjs` |
| Decimals | 9 |
| Freeze authority | Revoked (`null`) |
| Mint authority | ACTIVE - `31rYartQwHeBMjAe2MgGpffGV57fQY3kug4BDN8tLGqQ` |
| Largest holder | Same as mint authority (~26 percent at review) |

---

## 8. Conclusion

Grass is a real, widely used bandwidth-sharing / web-data project with a genuinely traded token, and freeze authority is revoked, which keeps it out of the high-risk band. The MEDIUM rating (58/100) is driven by an active mint authority held by a single keypair that is also the largest holder, i.e. one key holds both the power to dilute and the largest slice of supply, and by a "decentralized" claim that the project's own documentation qualifies with a "singular, centralized entity" validation layer. Not a scam; the caution is the live single-key mint authority and validation-layer centralization.

---

## 9. Recommendations

**For the Grass team:**
- Revoke the mint authority, or move it to a transparent multisig / on-chain emission contract with a published schedule.
- Separate the mint authority from the largest-holder wallet.
- Continue to disclose, as the docs already do, that the validation layer is currently centralized, and publish a decentralization roadmap with verifiable milestones.

**For users:**
- Understand that GRASS supply is not cryptographically fixed while mint authority is live.
- Treat self-reported node counts as marketing until independently verifiable.

---

## 10. Verification

- MEFAI on-chain analysis: direct Solana reads of the GRASS mint (freeze authority `null`, mint authority active and its address) and holder reads showing the mint-authority address as the largest holder.
- The mint address and its authorities are publicly verifiable by anyone on the Solana explorers.
- Project statements: the project's website and its own documentation (the "singular, centralized entity" wording for the validation layer).
