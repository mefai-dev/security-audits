# Security Audit Report: Render Network (RENDER) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Render Network |
| **Token Symbol** | RENDER (formerly RNDR) |
| **Mint (Solana)** | `rndrizKT3MK1iimdxRdWabcF7Zg7AR5T4nud4EkHBof` |
| **Chain** | Solana (canonical); legacy ERC 20 on Ethereum deprecated |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **66/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Render Network is one of the more genuine "decentralized compute" projects: it runs a real, used GPU rendering marketplace with a verified, capped supply token. It is not vaporware. The MEDIUM rating reflects a gap between the "decentralized, near unlimited scale" branding and a more curated, vendor dependent reality:

1. **The network is real and used**, with a large frame render count and high profile production work, a genuine positive.
2. **But it is a curated marketplace, not a permissionless network.** Node onboarding is waitlisted and manually approved, and jobs are routed through a Foundation run reputation and tier system, so both admission and work allocation are centrally arbitrated.
3. **Deep single vendor dependency:** the rendering stack is OTOY's OctaneRender, and the project's founder is also OTOY's chief executive, so the nominally independent Foundation and the founding company share leadership.
4. **On chain, the token's mint and freeze authorities are still active** (used for the scheduled Burn and Mint emission), a centralization and dilution point, and the network is still net emitting.

The product is real; the caution is curated control, vendor concentration and active token authorities, not legitimacy.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Mint (Solana)** | `rndrizKT3MK1iimdxRdWabcF7Zg7AR5T4nud4EkHBof` |
| **Decimals** | 8 |
| **Max supply** | 644,245,094 RENDER (the original 536,870,912 RNDR cap plus a 20 percent Burn and Mint emissions pool) |
| **Circulating (verified)** | ~484 million RENDER on Solana (total circulating ~519 million across chains) |
| **Supply model** | Burn and Mint Equilibrium (BME): jobs burn RENDER, protocol mints operator rewards on a declining schedule |
| **Migration** | RNDR (Ethereum ERC 20) to RENDER (Solana); began November 2023 and is ongoing (some RNDR remains on Ethereum) |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana read of the RENDER mint returned:

| Check | Result |
|-------|--------|
| Mint identity | RENDER (Solana SPL), 8 decimals, verified |
| Circulating supply | ~484 million on Solana (~519 million total across chains; cap 644,245,094) |
| Mint authority | **ACTIVE** (used for the BME emission schedule) |
| Freeze authority | **ACTIVE** |
| Supply model | Net emitting today: Year 1 emission on the order of ~9.1 million, Year 2 ~5.9 million RENDER, on a declining schedule |

**Interpretation.** RENDER has a genuine supply cap, a real positive. But unlike a fair launch memecoin, its mint and freeze authorities are **still live** (by design, to run the BME emission), which means new supply can be minted and accounts can in principle be frozen. Net deflation depends on burn volume from paid jobs exceeding the scheduled mint, a demand assumption, not a guarantee.

---

## 3. Claim vs Reality: "Decentralized, Near Unlimited Scale"

> Site: "the world's first decentralized GPU rendering platform"; "Decentralized supply provides near unlimited scale"; "harness idle global GPU power."

**Reality: distributed hardware, centrally arbitrated marketplace.** Compute is genuinely contributed by many independent operators, but:
- **Node onboarding is gated**, operators submit an interest form, enter a waitlist and are manually admitted by the team.
- **Job allocation is planned around a reputation and multi tier pricing system**: higher rated tiers are intended to receive priority work, while the full multi tier structure (including the top tier) is still rolling out.

So Render is closer to a **permissioned marketplace with distributed hardware** than a permissionless decentralized network. "Near unlimited scale" describes aggregate contributed GPU, mediated by a Foundation run allocation layer.

---

## 4. Claim vs Reality: Vendor Dependency

The rendering engine is **OTOY's OctaneRender**, and the founder of Render is also **OTOY's chief executive**. OTOY transferred the repository and brand to the Render Network Foundation only in early 2023, and the founder of Render is also OTOY's chief executive. Governance runs through on chain proposals with token holder votes (some pass, some fail), but the core software stack and the Foundation remain **OTOY adjacent**, a real single vendor and founder concentration behind a nominally independent foundation.

---

## 5. Claim vs Reality: "AI Compute"

Render's mature, revenue generating business is **GPU rendering** (Octane, Redshift, Blender), reflected in its real frame render metrics. The newer "AI compute" positioning (and a dispersed AI compute initiative) is a **less proven expansion** than the established rendering marketplace. The AI framing is forward looking relative to the demonstrated rendering traction.

---

## 6. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| RENDER 001 | **MEDIUM** | Mint and freeze authorities ACTIVE (BME emission): new supply can be minted; network is net emitting today. |
| RENDER 002 | **MEDIUM** | "Decentralized" network is a curated marketplace: waitlisted node admission and Foundation run reputation/tier job routing. |
| RENDER 003 | **LOW** | Deep OTOY / OctaneRender dependency; founder is also OTOY chief executive; Foundation is OTOY adjacent. |
| RENDER 004 | **LOW** | "AI compute" positioning is a newer, less proven expansion versus the mature rendering business. |
| RENDER 005 | **INFO** | Genuine supply cap of 644,245,094 (positive). |
| RENDER 006 | **INFO** | Real, used product with large frame render count and high profile production work (positive). |

---

## 7. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, capped, real usage |
| Supply / minting | Medium risk | Active mint authority, net emitting BME |
| Decentralization | Medium risk | Curated onboarding, tiered job routing |
| Vendor concentration | Medium risk | OTOY / OctaneRender and founder overlap |
| Product reality | Low risk | Real rendering marketplace |
| Transparency | Low to medium risk | AI framing ahead of demonstrated traction |

---

## 8. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `rndrizKT3MK1iimdxRdWabcF7Zg7AR5T4nud4EkHBof` |
| Decimals | 8 |
| Max supply | 644,245,094 RENDER |
| Circulating | ~484 million on Solana (~519 million total) |
| Mint / freeze authority | Active / Active |
| Supply model | Burn and Mint Equilibrium (net emitting) |

---

## 9. Conclusion

Render Network is a genuinely real, used GPU rendering marketplace with a verified, capped supply token, which keeps it in the MEDIUM band at 66/100. It is held back because the "decentralized, near unlimited scale" branding overstates a curated marketplace (waitlisted node admission, Foundation run tiered job routing), because of deep OTOY and OctaneRender dependency with founder overlap, and because the token's mint and freeze authorities are still active with the network net emitting. The product is real; the caution is curated control, vendor concentration and live token authorities.

---

## 10. Recommendations

**For the Render team:**
- Move node admission and job routing toward permissionless, transparent, on chain criteria, or stop calling the network "decentralized" without qualification.
- Disclose the OTOY relationship and the Foundation's independence clearly, and continue on chain governance.
- Publish the live emission versus burn balance so the net inflation reality is transparent.

**For users:**
- Note that RENDER supply is not fixed in practice (active mint authority, scheduled emission) and the network is net emitting.
- Treat the network as a curated marketplace with distributed hardware, and value the real rendering usage on its own merits.

---

## 11. Verification

- MEFAI on chain analysis: a direct Solana read of the RENDER mint (identity, 8 decimals, circulating supply, and active mint and freeze authorities).
- The mint address, supply and authorities are publicly verifiable by anyone on the Solana explorers.
- Project statements: the project's website and knowledge base (the "decentralized GPU rendering platform" and "near unlimited scale" wording, the Burn and Mint Equilibrium documentation, node onboarding and tier descriptions, and the RNDR to RENDER migration record).
