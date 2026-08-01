# Security Audit Report: Morpheus (MOR) on Arbitrum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 1, 2026 |
| **Project** | Morpheus |
| **Token Symbol** | MOR |
| **Contract (Arbitrum)** | `0x092baadb7def4c3981454dd9c0a0d7ff07bcfc86` |
| **Also on** | Ethereum and Base (a LayerZero omnichain token) |
| **Chain** | Arbitrum (and Ethereum, Base) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **62/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Morpheus (MOR) is a genuine, audited fair launch decentralized AI project with no premine, no venture allocation and no rug vectors. It is not a scam. The MEDIUM rating reflects real centralization and economic risks behind the fair launch:

1. **The fair launch is real and verifiable:** MEFAI confirms there is no premine or venture allocation; supply is emission earned, and the contracts have been audited.
2. **But two claims do not hold on chain.** The "42 million capped supply" is not enforced in the token code (there is no on chain cap, and minting is uncapped and fully controlled by the owner multisig, which can grant or revoke minter roles at will), and the "no central control" claim understates a 5 of 9 multisig that owns the token controls, with the deposited assets held under a separate 5 of 9 multisig.
3. **Capital and price collapsed together:** the deposited value is down roughly 97 percent from peak, mirroring the token drawdown, and real revenue is near zero.
4. **Decentralized inference is real but very early and concentrated** among a handful of compute providers, while the personal device agent vision is largely aspirational.

The fair launch is a genuine strength; the caution is an uncapped in code mint, a controlling multisig, a capital and price collapse and early, concentrated usage.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | MOR / MOR |
| **Contract (Arbitrum)** | `0x092baadb7def4c3981454dd9c0a0d7ff07bcfc86` |
| **Decimals** | 18 |
| **Supply** | Aggregate ~9 million MOR on chain across chains (~8.5 million circulating; a LayerZero omnichain token) |
| **Advertised cap** | 42 million (a social and emission schedule convention, NOT enforced in the token code) |
| **Owner** | A 5 of 9 multisig owns the token controls and can grant or revoke minter roles at will; minting is uncapped on chain |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified MOR on Arbitrum:

| Check | Result |
|-------|--------|
| Token identity | MOR, 18 decimals, verified; a LayerZero omnichain token (Arbitrum, Ethereum, Base) |
| Supply | Aggregate ~9 million MOR on chain (~8.5 million circulating); advertised max 42 million |
| Cap enforcement | **No hard cap in the token code**; the audit flagged the "capped" description as not enforced on chain |
| Mint authority | Uncapped on chain; minter roles are granted and revoked by the owner multisig via an updateMinter control |
| Owner | A 5 of 9 multisig owns the token controls and holds significant power over deposited assets |
| Launch | Fair launch (no premine, no venture allocation, nothing raised); contracts audited |

**Interpretation.** MOR is a genuine, audited fair launch token with no premine or venture allocation, a real strength. But the "42 million cap" is not enforced in code and minting is uncapped and fully controlled by the owner multisig (which can grant or revoke minter roles), and that 5 of 9 multisig holds meaningful power, so the "capped" and "no central control" claims overstate the on chain reality.

---

## 3. Claim vs Reality: "A Peer to Peer Network of Personal AIs"

> Site: a peer to peer network of personal, general purpose AIs, permissionless AI inference, a fair launch with no premine or venture capital, and no company, foundation or DAO.

**Reality: real fair launch, aspirational agents, understated control.** The fair launch is true and verifiable, and multiple audits exist, which genuinely distinguishes Morpheus from typical scams. But the "no central control" framing understates a 5 of 9 multisig that owns the token controls (and can grant or revoke minters), with the deposited assets held under a separate 5 of 9 multisig, and full on chain governance not yet live. The decentralized inference marketplace is real and in production, but very early: on chain activity shows only around a dozen unique compute providers and a small set of active ones, with traffic concentrated among a few large callers. There is no published count of live personal AI agents, so the personal device agent vision is largely aspirational versus a small hosted provider marketplace.

---

## 4. Claim vs Reality: Capital, Revenue and Value

- **Capital collapse:** the deposited value peaked in the hundreds of millions of dollars and is now down roughly 97 percent, mirroring the token drawdown, so the staking base has largely evaporated.
- **Near zero revenue:** the protocol's only material revenue is yield captured on deposits (a low single digit million dollars lifetime, a small annual run rate now), and compute and inference fees are still negligible, so emissions outpace burns.
- **Value:** the token is down roughly 97 percent from its 2024 peak, with thin, decentralized exchange primary liquidity that faces heavy slippage on sizable positions.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| MOR 001 | **MEDIUM** | The "42 million cap" is NOT enforced in the token code; minting is uncapped and fully controlled by the owner multisig, which can grant or revoke minter roles at will (the strongest claim versus reality gap). |
| MOR 002 | **MEDIUM** | "No central control" understates a 5 of 9 multisig that owns the token controls (and can grant or revoke minters), with the deposited assets under a separate 5 of 9 multisig; full on chain governance not yet live. |
| MOR 003 | **MEDIUM** | Capital and price collapse (~97 percent); near zero real revenue; decentralized inference real but very early and concentrated. |
| MOR 004 | **LOW** | Thin, decentralized exchange primary liquidity with heavy slippage risk. |
| MOR 005 | **INFO** | Genuine, audited fair launch with no premine, no venture allocation and nothing raised (a real positive versus typical projects). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Audited, genuine fair launch |
| Supply / minting | Medium risk | Cap not enforced in code, minter unrevocable |
| Decentralization | Medium risk | 5 of 9 multisig power; governance early |
| Usage reality | Medium risk | Inference early and concentrated |
| Revenue / capital | Medium to high risk | Near zero revenue, ~97 percent capital collapse |
| Value / volatility | High risk | ~97 percent drawdown, thin liquidity |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Contract (Arbitrum) | `0x092baadb7def4c3981454dd9c0a0d7ff07bcfc86` |
| Decimals | 18 |
| Supply | Aggregate ~9 million on chain (~8.5 million circulating; advertised max 42 million, not enforced) |
| Owner | 5 of 9 multisig (can grant or revoke minters); minting uncapped on chain |
| Launch | Fair launch (no premine, no venture allocation) |

---

## 8. Conclusion

Morpheus (MOR) is a genuine, audited fair launch decentralized AI project with no premine, no venture allocation and no rug vectors, which is a real positive and keeps it in the MEDIUM band at 62/100. It is held back because the advertised 42 million cap is not enforced in the token code and minting is uncapped and fully controlled by the owner multisig, because the "no central control" claim understates a 5 of 9 multisig (with the deposited assets under a separate 5 of 9 multisig), because the deposited value and price collapsed roughly 97 percent with near zero real revenue, and because decentralized inference is real but very early and concentrated. The fair launch is genuine; the caution is the uncapped in code mint, a controlling multisig and a capital collapse.

---

## 9. Recommendations

**For the Morpheus team:**
- Either enforce the 42 million cap on chain or stop describing the supply as capped, and clarify that minting is uncapped and controlled by the owner multisig.
- Disclose the 5 of 9 multisig control and its power over deposits, and deliver full on chain governance.
- Report real inference usage and revenue separately from emission driven activity.

**For users:**
- Understand the "cap" is not enforced in code, minting is uncapped, and a 5 of 9 multisig holds significant power today, despite the "no central control" framing.
- Value the genuine fair launch on its merits, and model the capital collapse, near zero revenue and thin liquidity.

---

## 10. Verification

- MEFAI on chain analysis: a direct read of the MOR token on Arbitrum (identity, 18 decimals, the 5 of 9 multisig owner, the aggregate supply and the absence of an on chain cap on the minter) across its omnichain deployments.
- The contract address, owner multisig and supply are publicly verifiable on the Arbitrum and other explorers.
- Project statements: the project's own pages (the peer to peer personal AI, permissionless inference and fair launch framing) and the published tokenomics and third party audits.
