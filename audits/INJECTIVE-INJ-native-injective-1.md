# Security Audit Report: Injective (INJ) on Injective

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Injective |
| **Token Symbol** | INJ |
| **Native chain** | Injective (Cosmos SDK L1, chain id `injective-1`) |
| **Also on** | Ethereum ERC 20 `0xe28b3B32B6c345A34Ff64674606124Dd5Aceca30` |
| **Chain** | Injective (and Ethereum) |
| **Audit Type** | Project + Network (Claim vs Reality) |
| **Mefai Security Score** | **72/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Injective is a real, established, well backed Cosmos based Layer 1 with a genuine on chain order book and a verified token. It is not a scam. The MEDIUM rating reflects a gap between the "fully decentralized, deflationary AI blockchain" framing and a more permissioned, net inflationary, thinly used reality:

1. **The core technology is real:** MEFAI confirms a working on chain central limit order book with a frequent batch auction design that genuinely mitigates ordering MEV, a legitimate technical differentiator.
2. **But the validator set is small and concentrated.** The active set is hard capped at 45 validators; MEFAI's read shows the top 5 sit just above the one third chain halt threshold (about 33.6 percent) and the Injective Foundation itself is the second largest validator. "Fully decentralized" overstates this.
3. **The token is net inflationary, not deflationary.** MEFAI's read of the on chain supply shows about 121.9 million INJ against a 100 million genesis, so despite the burn auction and "deflationary" branding, issuance currently exceeds burns.
4. **The "AI blockchain" framing is mostly marketing,** and on chain usage and value locked are a small fraction of the token's valuation.

The technology and backers are real; the caution is validator concentration, net inflation dressed as deflation, an AI narrative ahead of substance, and modest on chain usage.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token** | INJ (staking, gas, governance) |
| **Native chain** | Injective, Cosmos SDK L1, chain id `injective-1`, 18 decimals |
| **On chain total supply** | ~121.9 million INJ (genesis 100 million; net minted higher) |
| **Ethereum representation** | `0xe28b3B32B6c345A34Ff64674606124Dd5Aceca30`, 18 decimals, a fixed legacy peg at 100 million |
| **Inflation** | Dynamic band 2.2 to 4.4 percent (goal bonded 60 percent); currently pinned near the 4.4 percent maximum |
| **Burn** | Weekly burn auction of protocol and dApp fees, plus a newer community buy back |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified INJ on the native chain and on Ethereum:

| Check | Result |
|-------|--------|
| Native identity | `inj`, 18 decimals, Cosmos SDK L1 (CometBFT), verified |
| On chain total supply | ~121.9 million INJ (up from a 100 million genesis) |
| Ethereum token | `0xe28b3B32B6c345A34Ff64674606124Dd5Aceca30`, fixed at 100 million (legacy bridge peg) |
| Inflation | Band 2.2 to 4.4 percent; currently at the 4.4 percent maximum (bonded ratio below the 60 percent goal) |
| Validators | Hard cap 45, all slots filled; top 5 just above one third (~33.6 percent); the Foundation is the second largest |
| Governance | Standard on chain staker weighted governance, active |

**Interpretation.** INJ is a genuine native staking and governance token with a real on chain order book. The two supply figures (about 121.9 million native versus a fixed 100 million on Ethereum) are worth noting. The main cautions are a 45 validator cap where a handful of entities could halt the chain, and net inflation that runs against the "deflationary" branding.

---

## 3. Claim vs Reality: "The Blockchain Built for Finance" / Decentralization

> Site: "the blockchain built for finance", a fully decentralized network with a native, MEV resistant on chain order book.

**Reality: real order book, small permissioned validator core.** The on chain central limit order book with a frequent batch auction (uniform clearing per block) is substantive and genuinely reduces front running and sandwich MEV, a real strength. But decentralization at the validator layer is limited: the active set is capped at 45, MEFAI's read shows the top 5 validators sit just above the one third chain halt threshold (about 33.6 percent) and the top 10 hold a majority of stake, and the Foundation itself is the second largest validator. This is typical for a Cosmos chain but is not "fully decentralized."

---

## 4. Claim vs Reality: "AI Blockchain" and Usage

- **AI framing is mostly marketing:** the AI agent tooling is a real, open source off chain language model wrapper that turns natural language into ordinary transactions (place orders, check balances, send payments). The chain performs no on chain AI inference, so "AI blockchain" is a useful developer tool dressed as fundamental AI infrastructure.
- **Usage versus valuation:** on chain value locked is on the order of ten million dollars, down heavily from a roughly 71 million dollar peak in 2024, and on chain decentralized exchange volume is small in absolute terms; headline token trading volume is mostly centralized exchange activity in INJ itself, not dApp usage. Economic activity is a small fraction of the token's valuation and "built for finance" positioning.

---

## 5. Claim vs Reality: Deflation and Supply

- **"Deflationary" is aspirational:** MEFAI's on chain read shows supply grew from a 100 million genesis to about 121.9 million, and current issuance (roughly 5 million INJ per year at the 4.4 percent maximum) exceeds cumulative burns. Only a sustained fee and burn regime above issuance would make INJ net deflationary; today it is net inflationary.
- **Unlock overhang is minimal (positive):** genesis allocation to insiders and investors was high (roughly 40 to 45 percent), but the launch was in 2021 and vesting cliffs are complete, so present day supply growth is inflation, not unlocks.
- **Value:** the token is down roughly 91 percent from its 2024 peak.

---

## 6. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| INJ 001 | **MEDIUM** | Validator set hard capped at 45; the top 5 sit just above the one third chain halt threshold (~33.6 percent) and the Foundation is the second largest validator. |
| INJ 002 | **MEDIUM** | Net inflationary supply (about 121.9 million versus a 100 million genesis) despite "deflationary" branding; issuance exceeds burns today. |
| INJ 003 | **LOW** | On chain usage and value locked are a small fraction of the token's valuation; "AI blockchain" framing is largely marketing. |
| INJ 004 | **LOW** | Two supply figures across chains (native ~121.9 million versus a fixed 100 million on Ethereum) can confuse holders. |
| INJ 005 | **INFO** | Real on chain order book with frequent batch auction MEV mitigation; active governance; minimal unlock overhang (positives). |

---

## 7. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Established, well backed, verified |
| Supply / minting | Medium risk | Net inflationary, "deflationary" overclaim |
| Decentralization | Medium risk | 45 validator cap, top 5 can halt, Foundation is second |
| Usage / demand | Medium risk | Modest on chain volume and value locked |
| Claim accuracy | Medium risk | AI blockchain framing ahead of substance |
| Value / volatility | High risk | ~91 percent drawdown from peak |

---

## 8. Technical Specifications

| Item | Value |
|------|-------|
| Native chain | Injective, Cosmos SDK L1, chain id `injective-1`, 18 decimals |
| On chain total supply | ~121.9 million INJ |
| Ethereum token | `0xe28b3B32B6c345A34Ff64674606124Dd5Aceca30` (fixed 100 million) |
| Inflation | 2.2 to 4.4 percent band, at maximum today |
| Validators | 45 (capped), top 5 exceed one third |

---

## 9. Conclusion

Injective is a real, established, well backed Cosmos Layer 1 with a genuine on chain order book, frequent batch auction MEV mitigation, active governance and minimal unlock overhang, which keeps it in the MEDIUM band at 72/100. It is held back because the validator set is capped at 45 with the top 5 sitting just above the one third halt threshold and the Foundation as the second largest validator, because the supply is net inflationary despite "deflationary" branding, because the "AI blockchain" framing is largely marketing, and because on chain usage is a small fraction of the valuation. The technology is legitimate; the caution is validator concentration, net inflation and an AI narrative ahead of substance.

---

## 10. Recommendations

**For the Injective team:**
- Present INJ accurately as net inflationary today, and only describe deflation once burns durably exceed issuance.
- Continue widening and decentralizing the validator set beyond 45 and reduce Foundation validator prominence.
- Distinguish the off chain AI agent tooling from on chain capability in messaging.

**For users:**
- Understand INJ is net inflationary and that a small validator set governs the chain.
- Note the real on chain order book as a genuine strength, and model the drawdown and modest on chain usage.

---

## 11. Verification

- MEFAI on chain analysis: reads of INJ on the native chain (identity, 18 decimals, ~121.9 million supply, 2.2 to 4.4 percent inflation band at maximum, 45 validator cap and concentration) and the Ethereum representation (fixed 100 million).
- The supply, inflation, validators and governance parameters are publicly verifiable on the Injective and Ethereum explorers.
- Project statements: the project's website and documentation (the "built for finance" and on chain order book claims, the burn auction and deflation framing, and the AI agent tooling).
