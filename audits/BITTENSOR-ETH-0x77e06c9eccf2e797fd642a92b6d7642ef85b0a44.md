# Security Audit Report: Bittensor (wTAO wrapper) - Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | Bittensor (TAO) / Wrapped TAO on Ethereum |
| **Token Symbol** | wTAO (wrapper) / TAO (native) |
| **Wrapper Contract (Ethereum)** | `0x77e06c9eccf2e797fd642a92b6d7642ef85b0a44` |
| **Native chain** | Subtensor (Bittensor L1) |
| **Audit Type** | Token + Network (Claim vs Reality) |
| **Mefai Security Score** | **63/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on-chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Bittensor is a real, substantial decentralized-AI network with genuine economics: a native token (TAO) on its own L1 (Subtensor), a fixed 21 million cap with Bitcoin-style halvings, and real subnet activity. It is one of the most credible projects in this set, which is why it sits at the top of the MEDIUM band (63/100). Two things keep it from a higher score:

1. **The "decentralized" claim is materially overstated at the stake/validator layer.** MEFAI's on-chain stake-distribution analysis finds extreme concentration: a stake Gini near **0.98** and a very small number of large validators controlling the majority of stake and therefore of emissions. The security narrative ("thousands of participants") does not match where the control actually sits.
2. **This Ethereum contract is a wrapper (wTAO), not native TAO.** It is a **custodial bridge wrapper**: holders trust the operator to hold the backing native TAO. This adds a bridge/custody trust layer that native TAO does not have.

The network is real and economically active; the caution is stake centralization behind a "decentralized" label and the custodial nature of the Ethereum wrapper specifically.

---

## 1. Token / Network Overview

| Field | Value |
|-------|-------|
| **Native token** | TAO (on Subtensor L1) |
| **Ethereum representation** | wTAO wrapper `0x77e06c9eccf2e797fd642a92b6d7642ef85b0a44` |
| **Max supply** | 21,000,000 (fixed cap, Bitcoin-style) |
| **Emission** | Halving schedule (first halving expected around late 2025) |
| **Wrapper type** | Custodial bridge wrapper (native TAO held by the operator) |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI assessed both the native network economics and the Ethereum wrapper:

**Native TAO (Subtensor):**
- Fixed **21 million** cap with halvings, verifiable in the network's emission rules. This is a genuine, credible monetary policy.
- Real subnet activity and emissions; the token has real utility inside the network (registration, staking, validation).

**Stake / validator concentration (MEFAI on-chain stake-distribution analysis):**
- Stake distribution Gini near **0.98** (extreme inequality).
- A very small validator cohort controls the **majority of total stake** and therefore the majority of emissions and consensus weight (top validators well over half of stake).

**wTAO wrapper (Ethereum):**
- The Ethereum token is a **wrapper backed by custodied native TAO**. Holders depend on a bridge operator to hold and honor redemptions. This is a custody/bridge trust layer distinct from holding native TAO.

**Interpretation.** The monetary policy is real and strong. The decentralization of control is not: emissions and consensus are concentrated in a small validator set. On Ethereum specifically, wTAO adds custodial bridge risk on top.

---

## 3. Claim vs Reality: "Decentralized AI Network"

> Marketing: a permissionless, decentralized network of many miners and validators producing machine intelligence.

**Reality: real network, overstated decentralization.** Bittensor genuinely runs subnets where participants compete and are rewarded in TAO, this is not fabricated. But MEFAI's stake-distribution analysis shows control (stake, emissions, consensus weight) is concentrated in a small number of large validators (Gini near 0.98). The "thousands of decentralized participants" framing describes participation breadth, not control distribution; control is concentrated. Independent commentary in the decentralized-AI field has made the same point, that the effective governance is far more centralized than the branding implies.

---

## 4. Claim vs Reality: wTAO Backing (Ethereum)

The Ethereum contract audited here is **not** native TAO; it is a wrapped representation. Its value depends entirely on an operator custodying native TAO one-for-one and honoring redemptions. A holder of wTAO is therefore exposed to:
- **Custody risk** (the operator holding the backing TAO),
- **Bridge/redemption risk** (the wrapper honoring conversions),
which native TAO holders do not face. This is the reason the wrapper is called out separately from the network.

---

## 5. Positive Findings (Credited)

- **Genuine fixed monetary policy:** 21 million cap with halvings, real and verifiable.
- **Real network economics:** live subnets, real emissions and utility for TAO inside the network.
- **Not a rug:** this is an established network with real activity, not a fabricated project.

---

## 6. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| TAO-001 | **MEDIUM** | Stake/validator concentration is extreme (Gini ~0.98; small validator set controls majority of emissions), contradicting the "decentralized" framing. |
| TAO-002 | **MEDIUM** | This Ethereum token is a custodial wrapper (wTAO): custody + bridge/redemption trust on top of native TAO. |
| TAO-003 | **LOW** | Some network activity/participant metrics are self-reported and hard to independently verify. |
| TAO-004 | **INFO** | Fixed 21M cap with halvings; real subnet economics (positive; not a rug). |

---

## 7. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Supply / monetary policy | Low risk | Fixed 21M, halvings, verifiable |
| Network reality | Low risk | Real subnets and emissions |
| Decentralization of control | Medium to high risk | Stake Gini ~0.98, small validator cohort |
| Wrapper (Ethereum) | Medium risk | Custodial bridge, redemption trust |
| Transparency | Medium risk | "Decentralized" overstated vs control reality |

---

## 8. Technical Specifications

| Item | Value |
|------|-------|
| Native token | TAO on Subtensor L1 |
| Ethereum wrapper | `0x77e06c9eccf2e797fd642a92b6d7642ef85b0a44` (wTAO) |
| Max supply | 21,000,000 (fixed) |
| Emission | Halving schedule |
| Stake Gini | ~0.98 (MEFAI on-chain stake-distribution analysis) |
| Wrapper type | Custodial bridge |

---

## 9. Conclusion

Bittensor is among the most credible projects reviewed in this set: a real decentralized-AI network with genuine, Bitcoin-style monetary policy (21 million cap, halvings) and real subnet economics. That earns it the top of the MEDIUM band at 63/100. It is not higher because the "decentralized" claim overstates reality at the layer that matters, control: MEFAI's stake-distribution analysis shows a Gini near 0.98 with a small validator cohort controlling the majority of emissions. And the specific Ethereum token here, wTAO, is a custodial wrapper that adds bridge and redemption trust on top of native TAO. Real and substantial, but more centralized in control than the branding suggests, and custodial on Ethereum.

---

## 10. Recommendations

**For the Bittensor community:**
- Publish transparent, ongoing stake-decentralization metrics and pursue mechanisms that broaden validator control, not just participation.
- Clearly distinguish native TAO from custodial wrappers in official communications.

**For users:**
- Prefer native TAO where possible; understand that wTAO carries custodial/bridge risk.
- Treat "decentralized" as describing participation, not control; control is concentrated (Gini ~0.98).

---

## 11. Verification

- MEFAI on-chain analysis: the native emission/cap rules (21 million, halvings), an on-chain stake-distribution analysis of Subtensor validators (Gini ~0.98, majority-of-stake concentration), and inspection of the Ethereum wrapper contract confirming it is a custodial wrapped representation of native TAO.
- The wrapper contract address on Ethereum and the native stake distribution are publicly verifiable on the respective explorers.
- Project statements: the project's website and documentation describing the network and its monetary policy.
