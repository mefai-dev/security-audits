# Security Audit Report: Hivemapper (HONEY) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Hivemapper |
| **Token Symbol** | HONEY |
| **Mint (Solana)** | `4vMsoUT2BWatFweudnQM1xedRLfJgJ7hswhcpz4xgBTy` |
| **Chain** | Solana |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **54/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Hivemapper is a real decentralized mapping network with a working product, real business customers and reputable backers, but the HONEY token is operationally centralized with live mint and freeze keys. The MEDIUM rating reflects that tension:

1. **The product and customers are real:** MEFAI recognizes a genuine dashcam based mapping network with real map data sold to business customers and a real feature extraction capability.
2. **But both the mint and freeze authorities are live.** MEFAI's on chain read shows the mint authority is a live, off program account, so HONEY can be minted at will and the advertised cap is not enforced on chain, and the freeze authority is an active 2 of 3 multisig, so any holder account can be frozen. The advertised supply cap is a policy, not an on chain hard cap.
3. **The economy is emission driven, not revenue sustained,** with continuous weekly minting to contributors creating structural sell pressure, and the demand side burn is capped and small.
4. **The "decentralized" framing overstates a single company that holds the keys, owns the map database and controls all sales,** and the token is down roughly 99.8 percent from its peak.

The fundamentals are real; the caution is live mint and freeze keys, an unenforced cap, structural inflation and operational centralization.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Mint (Solana)** | `4vMsoUT2BWatFweudnQM1xedRLfJgJ7hswhcpz4xgBTy` |
| **Decimals** | 9 |
| **Total supply** | ~6.57 billion HONEY minted (~66 percent of a 10 billion policy cap) |
| **Mint authority** | **ACTIVE** (a live off program account; new HONEY can be minted at will, so the cap is not enforced on chain) |
| **Freeze authority** | **ACTIVE** (a 2 of 3 multisig; any token account can be frozen) |
| **Cap** | 10 billion is a tokenomics policy only, not an on chain hard cap |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana read of the HONEY mint returned:

| Check | Result |
|-------|--------|
| Mint identity | HONEY (Solana SPL, standard token program), 9 decimals, verified |
| Total supply | ~6.57 billion minted (~66 percent of the 10 billion policy cap) |
| Mint authority | **ACTIVE**, a live off program account, so supply can be minted at will and the cap is not enforced on chain |
| Freeze authority | **ACTIVE** (a 2 of 3 multisig), so any HONEY token account can be frozen (censorship capability) |
| Cap | No on chain hard cap; the 10 billion figure is policy, not cryptographically enforced |
| Emissions | Continuous weekly minting to contributors on a declining curve; the reissue of burned fees to contributors is capped at a small weekly amount |

**Interpretation.** Unlike a fair launch token with revoked authorities, HONEY has both mint and freeze authorities live. This is expected for a rewards token that mints weekly, but it is a real centralization and inflation vector: the mint authority can issue supply at will (so the advertised cap is not enforced on chain) and the freeze authority, a 2 of 3 multisig, can freeze accounts.

---

## 3. Claim vs Reality: "Decentralized Map of the World"

> Site: a decentralized network of people, cameras and apps mapping the world in real time, a decentralized alternative to the mapping oligopoly, with contributors earning HONEY by driving.

**Reality: a real but operationally centralized network.** A single company runs the network, holds both an active mint authority and an active freeze authority (the freeze authority is a 2 of 3 multisig), owns the map database and controls all business sales. Contributors supply data but do not govern token issuance, and the advertised supply cap is policy rather than code, so unilateral inflation and account freezing are both technically possible today. The mapping product and coverage are genuinely real, but "decentralized" overstates a company controlled network.

---

## 4. Claim vs Reality: Coverage, Revenue and AI

- **Coverage is real but uneven:** the network has mapped a large share of the world's roads, but coverage is concentrated in a few regions, so "global map" overstates uniformity, and crowdsourced imagery freshness varies by region.
- **Emissions dominate revenue:** real business to business map data revenue exists, but the reissue of burned fees to contributors is capped at a small weekly amount and demand side burn is small versus continuous weekly minting of new contributor rewards, so the economy is still primarily token emissions subsidizing the supply side, not self sustaining map data revenue; no audited public revenue figure is disclosed.
- **AI is genuine but proprietary:** the computer vision feature extraction and mapping data products are real and commercially oriented, but the models and pipeline are closed and run by the company, so the AI is not decentralized.
- **Structural sell pressure:** continuous weekly minting to contributors, who frequently sell HONEY to offset hardware and data costs, creates persistent sell pressure; roughly 66 percent of the policy cap is already minted with emissions continuing for years. Investor and team unlocks have concluded.
- **Value:** the token is down roughly 99.8 percent from its 2023 peak, with a small market capitalization and thin liquidity, despite reputable backers.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| HONEY 001 | **MEDIUM** | Mint authority is ACTIVE (a live off program account), so supply can be minted at will; the 10 billion cap is policy, not on chain enforced. |
| HONEY 002 | **MEDIUM** | Freeze authority is ACTIVE (a 2 of 3 multisig), so any holder account can be frozen (censorship capability). |
| HONEY 003 | **MEDIUM** | Operationally centralized: one company holds the keys, owns the map database and controls all sales; emissions subsidize the supply side over revenue. |
| HONEY 004 | **LOW** | Structural sell pressure from continuous minting; ~99.8 percent drawdown from peak. |
| HONEY 005 | **INFO** | Real mapping product, real business customers and a genuine feature extraction capability; standard non malicious SPL token (positives). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token mechanics | Medium to high risk | Active mint and freeze authorities, unenforced cap |
| Supply / minting | Medium to high risk | Arbitrary mint possible, structural emissions |
| Decentralization | Medium to high risk | Company holds keys, database and sales |
| Product reality | Low risk | Real mapping product and business customers |
| Transparency | Medium risk | No audited revenue; cap is policy only |
| Value / volatility | High risk | ~99.8 percent drawdown |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `4vMsoUT2BWatFweudnQM1xedRLfJgJ7hswhcpz4xgBTy` |
| Decimals | 9 |
| Total supply | ~6.57 billion HONEY (~66 percent of a 10 billion policy cap) |
| Mint / freeze authority | Active (off program) / Active (2 of 3 multisig) |
| Cap | Policy only, not on chain enforced |

---

## 8. Conclusion

Hivemapper is a real decentralized mapping network with a working product, real business customers, a genuine feature extraction capability and reputable backers, which keeps it in the MEDIUM band at 54/100. It is held back because both the mint and freeze authorities are live, so supply can be minted at will (the cap is not enforced on chain) and any account can be frozen (the freeze authority is a 2 of 3 multisig), because the advertised 10 billion cap is policy rather than an on chain limit, because the economy is emission driven rather than revenue sustained with structural sell pressure, and because the network is operationally centralized under one company, against a roughly 99.8 percent drawdown. The fundamentals are real; the caution is live mint and freeze keys, an unenforced cap and operational centralization.

---

## 9. Recommendations

**For the Hivemapper team:**
- Move the mint authority to a transparent, timelocked or program controlled process so issuance is not off program key controlled, and document the freeze authority multisig and its policy.
- Enforce the supply cap on chain, or state clearly that the cap is policy only.
- Publish audited map data revenue so holders can judge revenue versus emissions.

**For users:**
- Understand that the mint authority can issue HONEY at will today and a multisig can freeze accounts, and that the cap is not enforced on chain.
- Note the emission driven economy and structural sell pressure, and model the drawdown; the real product does not offset the token centralization risk.

---

## 10. Verification

- MEFAI on chain analysis: a direct Solana read of the HONEY mint (identity, 9 decimals, ~6.57 billion minted, mint authority active and key controlled by an ordinary wallet, freeze authority active, no on chain hard cap).
- The mint address, supply and authorities are publicly verifiable by anyone on the Solana explorers.
- Project statements: the project's own pages (the decentralized mapping, drive to earn, coverage, feature extraction and business to business framing) and the published tokenomics and emission schedule.
