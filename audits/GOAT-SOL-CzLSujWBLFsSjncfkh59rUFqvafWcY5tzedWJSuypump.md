# Security Audit Report: Goatseus Maximus (GOAT) - Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | Goatseus Maximus |
| **Token Symbol** | GOAT |
| **Contract (mint)** | `CzLSujWBLFsSjncfkh59rUFqvafWcY5tzedWJSuypump` |
| **Chain** | Solana |
| **Audit Type** | Token (memecoin) + Origin Claim |
| **Mefai Security Score** | **68/100** |
| **Overall Risk** | **LOW rug risk / MEDIUM narrative risk** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own on-chain metadata, and data verified through MEFAI's on-chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. Memecoins are speculative by nature; this report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report.

---

## Executive Summary

GOAT is a technically clean pump.fun memecoin. MEFAI's on-chain analysis confirms the strongest possible token-mechanics profile: mint and freeze authorities revoked, immutable metadata, liquidity locked, and a very broad holder base with no rug flags. The rug risk is genuinely LOW.

The reason this token is flagged is a **narrative integrity** problem, not a contract problem. The popular claim that GOAT was "created by an autonomous AI" does not survive scrutiny:

- A human deployed the token on pump.fun; the associated AI persona (Truth Terminal) only **endorsed** it afterward.
- That AI persona is itself human-reviewed (its operator approves posts before they go live), so "autonomous" is overstated even for the persona.

The token is safe on mechanics; the caution is that the story that drove its valuation is misleading, and it has since lost roughly 99 percent of its peak value.

---

## 1. Contract Overview

| Field | Value |
|-------|-------|
| **Token** | GOAT |
| **Mint** | `CzLSujWBLFsSjncfkh59rUFqvafWcY5tzedWJSuypump` (pump.fun launch) |
| **Decimals** | 6 |
| **Supply** | ~1 billion (~999.98 million) |
| **Mint authority** | `null` (revoked) |
| **Freeze authority** | `null` (revoked) |
| **Metadata** | Immutable (cannot be changed) |
| **Creator balance** | 0 |

---

## 2. On-chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana reads and on-chain scan returned a clean, low-rug profile:

| Check | Result |
|-------|--------|
| Mint authority | **Revoked** - no dilution possible |
| Freeze authority | **Revoked** - accounts cannot be frozen |
| Metadata | **Immutable** |
| Liquidity | **~99.74 percent locked** (main Raydium pool, ~1.14 million dollars) |
| Holders | ~247,561 (very broad distribution) |
| MEFAI rug scan | No risk flags, not rugged |
| Largest single wallet | ~18.6 percent (unlabeled; consistent with exchange custody, UNVERIFIED) |

**Interpretation.** On pure token mechanics this is a fair-ish, non-insider-heavy launch: revoked authorities, immutable metadata, locked liquidity and roughly a quarter of a million holders. The only concentration caveat is a single ~18.6 percent wallet that MEFAI could not positively label (likely exchange custody).

---

## 3. Claim vs Reality: "Created by an Autonomous AI"

> On-chain metadata / popular framing: "First meme created by @truth_terminal", widely amplified as "an autonomous AI created a token".

**Reality: MISLEADING.** The AI did not create or deploy the token. It was publicly documented, including in a direct interview with the persona's operator, that the AI chatbot "did not, physically, create a memecoin"; a **human follower** deployed the GOAT token on pump.fun, and the AI persona endorsed it afterward. The deploying wallet is a human key, and the token was created by an anonymous developer via pump.fun (deploy cost is negligible, on the order of a couple of dollars). The correct description is: **human deployed, AI endorsed after** - not "AI created".

---

## 4. Claim vs Reality: "Autonomous AI Agent"

> Media framing: "autonomous AI agent" / "first AI agent millionaire".

**Reality: OVERSTATED.** The persona (Truth Terminal) is **not fully autonomous**: its operator reviews the posts before they go live and makes the wallet decisions through discussion with the model. It is a curated large-language-model account whose crypto actions are executed by a human, not an on-chain agent controlling its own keys. The persona's own operator has publicly said it is disingenuous to call it an autonomous agent. The genuine, honest claim, that the AI **endorsed** a meme, was collapsed by downstream media into "an AI created a billion-dollar token".

---

## 5. Value Context

- All-time-high market cap: near **1 billion dollars**, reached around mid-November 2024 (the token launched on 10 October 2024, so the peak was the November run, not the launch).
- Current: approximately **12.7 million dollars** - a decline of roughly **99 percent**.
- This is ordinary memecoin volatility, not a contract exploit.

---

## 6. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| GOAT-001 | **MEDIUM** | The "autonomous AI created this token" origin narrative is false: a human deployed it; the AI endorsed it afterward. |
| GOAT-002 | **LOW** | The AI persona is human-reviewed, so even "autonomous agent" is overstated. |
| GOAT-003 | **LOW** | A single unlabeled wallet holds ~18.6 percent (likely exchange custody, UNVERIFIED). |
| GOAT-004 | **INFO** | ~99 percent decline from peak: high memecoin volatility. |
| GOAT-005 | **INFO** | Mint and freeze authorities revoked, metadata immutable, LP ~99.74 percent locked, ~247k holders (strong, positive). |

---

## 7. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Ownership / mint | Low risk | Mint and freeze revoked, metadata immutable |
| Liquidity security | Low risk | ~99.74 percent LP locked |
| Holder distribution | Low to medium risk | ~247k holders; one ~18.6 percent wallet |
| Narrative integrity | Medium risk | False "AI created it" origin story |
| Volatility | High | ~99 percent decline from peak |

---

## 8. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `CzLSujWBLFsSjncfkh59rUFqvafWcY5tzedWJSuypump` |
| Decimals / supply | 6 / ~999.98M |
| Mint / freeze authority | Revoked / Revoked |
| Metadata | Immutable |
| LP locked | ~99.74 percent (~1.14M dollars) |
| Holders | ~247,561 |
| ATH / current | ~1B / ~12.7M dollars |

---

## 9. Conclusion

GOAT is one of the cleanest memecoins by mechanics: revoked authorities, immutable metadata, locked liquidity, and a very broad holder base, so its rug risk is genuinely LOW. It is flagged for narrative integrity: the story that "an autonomous AI created" the token is false (a human deployed it; the AI endorsed it afterward, and the AI itself is human-reviewed). The score of 68/100 reflects a clean contract paired with a misleading origin claim and extreme (~99 percent) drawdown from peak.

---

## 10. Recommendations

**For promoters and holders:**
- Describe the origin honestly: an AI persona endorsed a human-deployed memecoin; it did not autonomously create it.
- Understand this is a speculative memecoin with ~99 percent drawdown from peak.

**For users:**
- The contract mechanics are clean; the risk is volatility and a misleading origin story, not a rug.
- Verify the exact mint address; treat the "AI created it" narrative as marketing.

---

## 11. Verification

- MEFAI on-chain analysis: direct Solana reads of the GOAT mint (mint authority, freeze authority, supply, immutable metadata), a MEFAI on-chain rug scan (holders, locked LP, risk flags).
- The mint address and its state are publicly verifiable by anyone on the Solana explorers.
- Origin facts: the token's own on-chain metadata, the pump.fun deployment record (a human deployer wallet), and public interviews with the AI persona's operator confirming the AI "did not, physically, create a memecoin".
