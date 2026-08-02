# Security Audit Report: Freysa (FAI) on Base

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 2, 2026 |
| **Project** | Freysa |
| **Token Symbol** | FAI |
| **Contract / Program** | `0xb33Ff54b9F7242EF1593d2C9Bcd8f9df46c77935` |
| **Chain** | Base |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **80/100** |
| **Overall Risk** | **LOW** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Claim vs reality audit of Freysa (FAI). An autonomous AI agent token on Base whose contract is one of the cleanest in this set, an immutable, ownerless, fixed supply OpenZeppelin ERC20 with no mint function, no proxy, no pause, and no transfer fee, so the only material risk is market and narrative volatility around a single AI agent rather than any contract control.

Freysa (FAI) is the token of an autonomous AI agent project that began as an adversarial prompting game and now frames itself around personal AI and decentralized ownership. The token is a plain ERC20 on Base at 0xb33Ff54b9F7242EF1593d2C9Bcd8f9df46c77935, confirmed live over the Base RPC where it returns the name FAI, symbol FAI, eighteen decimals, and a total supply of 8,189,700,000. The onchain contract is a stock OpenZeppelin ERC20 version five with a minimal wrapper that mints the full supply once in the constructor and adds nothing else.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 1 |
| Informational | 3 |
| **Total** | **4** |

### Overall Risk Assessment: LOW

MEFAI onchain analysis places Freysa at 80 out of 100 (LOW risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Freysa / FAI |
| **Contract or program** | `0xb33Ff54b9F7242EF1593d2C9Bcd8f9df46c77935` |
| **Chain** | Base |
| **Tags** | ERC 20, AI Agent, Fixed Supply No Owner, Base, Passed |

The contract has no owner and no admin. The owner call reverts, there is no ownership role, and the implementation and admin storage slots are both zero, so it is not a proxy and cannot be upgraded. There is no pause function, no blacklist, no maximum wallet logic, and the transfer path is the unmodified OpenZeppelin update, meaning there is no buy or sell tax and no fee recipient. In short the token is immutable, fixed in supply, and free of transfer taxes, which is a strong technical safety profile. The main residual risk is market and narrative risk around a single volatile AI agent asset rather than contract control risk.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- FAI on Base (0xb33Ff54b9F7242EF1593d2C9Bcd8f9df46c77935, 18 decimals) verified live: total supply exactly 8,189,700,000, minted once in the constructor with no external mint function. The owner call reverts (no ownership role), the proxy implementation and admin slots are both zero (not upgradeable), there is no pause, and the transfer path is the unmodified OpenZeppelin update with no fee.

---

## 3. Claim versus Reality

- "A fixed supply fair launch AI agent token with no hidden inflation" / Reality: confirmed on chain, since the total matches the advertised 8,189,700,000, no mint path exists, and circulating equals maximum. The reported burned liquidity was not re traced on chain in this pass.
- Value depends on a single AI agent narrative, so price is highly volatile.

The public claims of a fixed 8,189,700,000 supply, one token per living human at launch, and no hidden inflation align cleanly with the chain. The measured total supply matches the advertised figure to the token, and the source code exposes no external mint function, so the supply cannot grow. Circulating supply equals max supply, which is consistent with a full launch with no locked allocations or future unlocks. The reported fair launch with burned liquidity tokens was not re traced onchain in this review, so that specific claim carries slightly lower certainty than the supply facts.

---

## 4. Website and Frontend Integrity

The official website was reviewed and is reachable. It presents as a standard project site with no unbacked audit or certification badges detected in the page. The token contract address is not printed in the static page and loads dynamically, so a visitor should always confirm the official contract address from the project documentation before connecting a wallet or trading.


---

## 5. Findings by Severity

- LOW: single agent concentration and high market volatility; liquidity burn not independently re traced. INFO: immutable, ownerless, fixed supply with no mint, no proxy, no pause, and no fee (one of the cleanest contract profiles in this batch).

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 15 |
| Supply and minting | 15 |
| Liquidity and market | 12 |
| Code safety | 14 |
| Transfer neutrality | 15 |
| Transparency | 9 |
| **Total** | **80/100** |

---

## 7. Conclusion

Claim vs reality audit of Freysa (FAI). An autonomous AI agent token on Base whose contract is one of the cleanest in this set, an immutable, ownerless, fixed supply OpenZeppelin ERC20 with no mint function, no proxy, no pause, and no transfer fee, so the only material risk is market and narrative volatility around a single AI agent rather than any contract control. On the MEFAI scale this token scores 80 out of 100 and is classified Passed.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Base, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `basescan 0xb33ff54...`
  - `base.blockscout 0xb33ff...`
  - `coingecko freysa-ai`
  - `coincarp fai`
  - `phemex freysa`
  - `mainnet.base.org RPC.`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
