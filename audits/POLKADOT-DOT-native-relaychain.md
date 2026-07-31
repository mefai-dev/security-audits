# Security Audit Report: Polkadot (DOT) - Polkadot Relay Chain

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-31 |
| **Project** | Polkadot |
| **Token Symbol** | DOT |
| **Native token** | DOT (Polkadot Relay Chain, native, non-EVM) |
| **Chain** | Polkadot |
| **Audit Type** | Network Native Token (Claim vs Reality) |
| **Mefai Security Score** | **68/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on-chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Polkadot is a real, technically serious shared-security network with a genuine token, a large nominator base and credible engineering. It is a blue-chip network. The MEDIUM rating reflects model churn, an adoption decline, and a governance/treasury credibility gap, partly offset by a major and genuine 2026 monetary reform:

1. **Three architectures in five years.** The parachain slot-auction model that early marketing pitched as the core value proposition was replaced by "Agile Coretime" (live September 2024) and is now being aimed at a further redesign, "JAM," which is on testnet (January 2026) but not on mainnet. Heavy forward-looking narrative sits around unshipped work.
2. **Parachain adoption has thinned.** Of 200-plus registered parachains, MEFAI's review finds only roughly 30 maintain consistent block production, and ecosystem value locked has fallen to a low single-digit share of DeFi from its 2021 peak. "Shared security" is real but demand for it has softened.
3. **A community-treasury credibility gap.** A 2024 spending surge (roughly 87 million dollars in six months, much on marketing including sports sponsorships) drew a runway warning that leadership disputed.

To its credit, in **March 2026 Polkadot cut inflation from about 10 percent to about 3.11 percent and, reversing years of "no hard cap" messaging, encoded a roughly 2.1 billion DOT cap**, a real, positive reform. MEFAI notes the official wiki still described the old model at review time.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token** | DOT (staking, governance, coretime) |
| **Decimals (verified)** | 10 (1 DOT = 10^10 Planck) |
| **Supply** | ~1.70 billion total, ~1.68 to 1.78 billion circulating |
| **Inflation** | Cut from ~10 percent to ~3.11 percent (March 2026) |
| **Hard cap** | ~2.1 billion DOT, encoded March 2026 (previously none) |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI verified DOT's chain parameters and reviewed the 2026 monetary reform:

| Check | Result |
|-------|--------|
| Token identity / decimals | DOT, 10 decimals, verified via chain properties |
| Supply | ~1.70 billion total (~1.68 to 1.78 billion circulating) |
| Inflation | ~3.11 percent (reduced from ~10 percent, March 2026) |
| Hard cap | ~2.1 billion DOT (encoded March 2026); ~13 percent issuance reduction every two years |
| Validators | ~900 to 990 active; nominators ~44,000 |

**Interpretation.** DOT is a genuine, liquid staking-and-governance token. The March 2026 reform (inflation cut plus a newly-encoded hard cap) materially improves its monetary profile and addresses the long-standing high-inflation critique, a real positive. The caution moves to architecture churn and adoption.

---

## 3. Claim vs Reality: "Shared Security" and Model Churn

> Wiki: "Shared security... is one of the unique value propositions for chains considering becoming a parachain"; parachains "inherit the security of the entire network." Brand: "the blockspace ecosystem for boundless innovation."

**Reality: real shared security, shifting delivery model.** The pooled-security design is genuine: parachains do inherit relay-chain validator security. But the **delivery model has changed twice**: slot auctions (which early marketing pitched as the core mechanism, and which the project now frames as inefficient) gave way to Agile Coretime (blockspace sold as a commodity, live September 2024), with a further "JAM" redesign announced and on testnet but **not on mainnet**. A prospective buyer is aiming at a moving target, and much of the current narrative is forward-looking.

---

## 4. Claim vs Reality: Adoption and Treasury

- **Adoption:** of 200-plus registered parachains, only roughly **30 maintain consistent block production and volume**; ecosystem value locked (~1.2 billion dollars) has fallen to a low single-digit share of DeFi from roughly 3.5 percent in 2021. Shared security is real but under-demanded.
- **Treasury:** the "community-governed treasury" narrative collided with a 2024 backlash over roughly **87 million dollars spent in six months** (about 37 million on marketing, including sports sponsorships), prompting a roughly two-year-runway warning that leadership countered as "at least five years." Governance is genuinely on-chain (OpenGov), but spending discipline is contested.
- **Decentralization design (positive):** equal per-validator rewards regardless of stake, nominators selecting up to 16 validators, a 256-nominator oversubscription cap and progressive slashing are genuine anti-centralization mechanisms; the small active-validator set is nonetheless a concentration ceiling relative to tens of thousands of nominators.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| DOT-001 | **MEDIUM** | Architecture churn: auctions to Agile Coretime (2024) to JAM (testnet, not mainnet); heavy forward-looking narrative. |
| DOT-002 | **MEDIUM** | Parachain adoption thinned: ~30 of 200-plus consistently active; ecosystem TVL down to a low single-digit DeFi share. |
| DOT-003 | **LOW** | 2024 treasury spending surge (~87 million dollars/6 months) and runway concern; spending discipline contested. |
| DOT-004 | **LOW** | Active validator set (~900 to 990) is a concentration ceiling relative to ~44,000 nominators. |
| DOT-005 | **INFO** | Official wiki lagged the live protocol (still described the old inflation model at review). |
| DOT-006 | **INFO** | March 2026 reform: inflation ~10 percent to ~3.11 percent plus a ~2.1 billion hard cap (genuine positive). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, liquid, large nominator base |
| Supply / minting | Low risk | 2026 hard cap + inflation cut |
| Roadmap delivery | Medium risk | Auctions to coretime to JAM churn; JAM unshipped |
| Adoption | Medium risk | Parachain activity and TVL declined |
| Governance / treasury | Medium risk | Spending discipline contested |
| Transparency | Low to medium risk | Docs lagged the live protocol |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Token / decimals | DOT / 10 (1 DOT = 10^10 Planck) |
| Supply | ~1.70 billion total (~1.68 to 1.78 billion circulating) |
| Inflation | ~3.11 percent (from ~10 percent, March 2026) |
| Hard cap | ~2.1 billion DOT (encoded March 2026) |
| Validators / nominators | ~900 to 990 / ~44,000 |

---

## 8. Conclusion

Polkadot is a real, technically serious shared-security network, and its March 2026 monetary reform (a large inflation cut plus a newly-encoded ~2.1 billion hard cap) genuinely strengthens its tokenomics, keeping it mid-band at 68/100 (MEDIUM). It is not higher because the delivery model has changed twice in five years with a further redesign still unshipped, parachain adoption and ecosystem value have thinned, and the community-treasury narrative met a real spending-discipline backlash. The engineering and shared security are real; the caution is model churn, softening adoption and governance discipline.

---

## 9. Recommendations

**For the Polkadot community:**
- Update official documentation to match the live post-March-2026 monetary policy, and clearly separate shipped Agile Coretime from the forward-looking JAM redesign.
- Publish transparent treasury spend-versus-runway data and demonstrate spending discipline.

**For users:**
- Note the genuine 2026 inflation cut and hard cap as positives, but weigh architecture churn and softened parachain adoption.
- Treat JAM as unshipped and the value proposition as still evolving.

---

## 10. Verification

- MEFAI on-chain analysis: verification of DOT's chain properties (10 decimals) and review of the March 2026 monetary reform (inflation reduction to ~3.11 percent and the ~2.1 billion hard cap), supply (~1.70 billion) and validator/nominator counts.
- The token parameters, supply, validators and treasury are publicly verifiable on the Polkadot explorers and on-chain governance.
- Project statements: the Polkadot wiki (the "shared security" and parachain wording, and the older inflation model that lagged the live protocol), the Agile Coretime documentation, and the public record of the JAM testnet and the 2024 treasury-spending debate.
