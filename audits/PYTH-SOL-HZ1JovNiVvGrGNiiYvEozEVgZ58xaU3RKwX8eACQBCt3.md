# Security Audit Report: Pyth Network (PYTH) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 1, 2026 |
| **Project** | Pyth Network |
| **Token Symbol** | PYTH |
| **Mint (Solana)** | `HZ1JovNiVvGrGNiiYvEozEVgZ58xaU3RKwX8eACQBCt3` |
| **Chain** | Solana (feeds delivered to 100+ chains) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **66/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Pyth Network (PYTH) is a real, widely integrated first party financial oracle with an unusually clean, non mintable Solana token. It is not a scam. The MEDIUM rating reflects a curated and permissioned reality behind the decentralized branding:

1. **The token is clean and fixed:** MEFAI confirms a fixed 10 billion supply with both the mint authority and the freeze authority revoked, so there is no inflation and no account freeze vector.
2. **But the model is curated and permissioned.** The data publishers are a whitelisted set of institutions, the Pythnet aggregation chain is a permissioned proof of authority Solana fork, and governance is concentrated in a foundation and small councils.
3. **The float is still unlock heavy.** Only around four fifths of supply circulates, with a large scheduled unlock still ahead, a persistent overhang.
4. **Cross chain delivery depends on a permissioned attestation network** (a guardian set), inheriting that trust model.

The product and usage are real and best in class for first party data; the caution is a curated publisher and governance model, a remaining unlock overhang, and a cross chain trust dependency.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | Pyth Network / PYTH |
| **Mint (Solana, verified)** | `HZ1JovNiVvGrGNiiYvEozEVgZ58xaU3RKwX8eACQBCt3` |
| **Decimals** | 6 |
| **Supply** | ~10,000,000,000 PYTH, fixed (mint authority and freeze authority both revoked) |
| **Circulating** | Roughly four fifths of supply, with a large scheduled unlock still ahead |
| **Allocation** | Ecosystem growth and publisher rewards heavy, with private sale and protocol development tranches |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified PYTH on Solana:

| Check | Result |
|-------|--------|
| Token identity | PYTH, a Solana SPL token, 6 decimals, verified |
| Supply | ~10,000,000,000 PYTH |
| Mint authority | **Revoked (null)**, so the supply is non mintable and fixed, a strong positive |
| Freeze authority | **Revoked (null)**, so no account can be frozen, a strong positive |
| Publishers | A whitelisted set of first party institutional data providers (permissioned) |
| Aggregation | Pythnet, a permissioned proof of authority Solana fork where publishers are effectively the validators |

**Interpretation.** The PYTH token contract is clean: fixed supply, no mint, no freeze, so there is no inflation or freeze backdoor, a genuine strength. The cautions are entirely at the product and governance layer: a curated publisher set, a permissioned aggregation chain, concentrated governance, and a remaining unlock overhang.

---

## 3. Claim vs Reality: "A Decentralized First Party Financial Oracle"

> Site: a decentralized first party oracle where institutions publish their own proprietary prices, a pull oracle delivering 1,000+ price feeds to 100+ chains.

**Reality: real, best in class first party data, curated and permissioned.** The first party model is genuine and differentiated: over a hundred named institutional publishers submit their own prices, feeding a large, widely integrated set of price feeds across many chains, with an oracle integrity staking layer that adds real accountability. But the decentralization is curated: only whitelisted institutions may publish (admission runs through governance), the Pythnet aggregation chain is a permissioned proof of authority fork where the same publishers effectively secure consensus, and governance power sits with a foundation and two small councils. It is more accurately a permissioned consortium oracle with a governance wrapper than a permissionless network.

---

## 4. Claim vs Reality: Float, Governance and Value

- **Unlock overhang:** only around four fifths of supply circulates, and a large scheduled unlock is still ahead, which has already coincided with fresh lows, a persistent sell side overhang.
- **Curated and concentrated control:** the publisher set is whitelisted, the aggregation chain is permissioned, and governance is concentrated, so control is more centralized than the decentralized framing implies.
- **Supply integrity (positive):** the 10 billion supply is fixed with mint and freeze both revoked, so the overhang is unlock driven, not emission driven.
- **Cross chain trust:** delivery to many chains rides a permissioned attestation guardian set, inheriting that trust model.
- **Value:** the token is down roughly 96 percent from its 2024 peak, near all time lows.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| PYTH 001 | **MEDIUM** | Unlock overhang: only around four fifths of supply circulates with a large scheduled unlock still ahead (persistent sell pressure). |
| PYTH 002 | **MEDIUM** | Curated and concentrated control: whitelisted publishers, a permissioned proof of authority aggregation chain, and concentrated governance, against the decentralized framing. |
| PYTH 003 | **LOW** | The permissioned proof of authority aggregation chain means price aggregation is not trustless. |
| PYTH 004 | **LOW** | Cross chain delivery depends on a permissioned attestation guardian set (a cross chain trust dependency). |
| PYTH 005 | **INFO** | Fixed 10 billion supply with mint and freeze authorities both revoked (a strong contract level positive); real, broad first party usage. |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Fixed, non mintable, no freeze, real usage |
| Supply / minting | Low risk | Mint and freeze revoked |
| Supply / unlocks | Medium risk | Around a fifth still locked with a large unlock ahead |
| Decentralization | Medium risk | Curated publishers, permissioned chain, concentrated governance |
| Usage reality | Low risk | Real, broad first party oracle usage |
| Value / volatility | High risk | ~96 percent drawdown near all time lows |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Mint (Solana) | `HZ1JovNiVvGrGNiiYvEozEVgZ58xaU3RKwX8eACQBCt3` |
| Decimals | 6 |
| Supply | ~10,000,000,000 PYTH (fixed) |
| Mint / freeze | Both revoked |
| Circulating | Roughly four fifths, large unlock ahead |

---

## 8. Conclusion

Pyth Network (PYTH) is a real, widely integrated first party financial oracle with a fixed 10 billion Solana token whose mint and freeze authorities are both revoked, which is a strong contract level positive and keeps it in the MEDIUM band at 66/100. It is held back because only around four fifths of supply circulates with a large scheduled unlock still ahead, because the model is curated and permissioned (whitelisted publishers, a permissioned proof of authority aggregation chain and concentrated governance), because cross chain delivery depends on a permissioned guardian set, and because the token is down roughly 96 percent from its 2024 peak. The product and usage are real and best in class; the caution is the curated publisher and governance model, the unlock overhang and the cross chain trust dependency.

---

## 9. Recommendations

**For the Pyth team:**
- Present the remaining unlock schedule prominently, so the overhang is clear.
- Broaden and clearly document the path toward permissionless publishing and less concentrated governance.
- Report real oracle usage and revenue separately from incentive driven ecosystem activity.

**For users:**
- Value the fixed, non mintable, freeze free token and the genuine first party usage on their merits.
- Understand the publisher set and aggregation chain are permissioned, model the remaining unlock overhang and the cross chain trust dependency, and note the drawdown.

---

## 10. Verification

- MEFAI on chain analysis: a direct read of the PYTH mint on Solana (identity, 6 decimals, the ~10 billion supply, and both the mint authority and freeze authority revoked) and review of the whitelisted publisher model, the permissioned Pythnet aggregation chain and the unlock schedule.
- The mint, supply and revoked authorities are publicly verifiable on the Solana explorers.
- Project statements: the project's own pages (the first party decentralized oracle and pull oracle framing) and the published tokenomics, publisher application process and governance model.
