# Security Audit Report: NATIX Network (NATIX) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 2, 2026 |
| **Project** | NATIX Network |
| **Token Symbol** | NATIX |
| **Contract / Program** | `FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX` |
| **Chain** | Solana |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **72/100** |
| **Overall Risk** | **LOW** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Claim vs reality audit of NATIX Network (NATIX). A real Solana mapping DePIN whose SPL token has both mint and freeze authorities renounced, so supply is permanently fixed at the roughly 99.29 billion already minted against a 100 billion cap, with no transfer fee, the main consideration being a large unlock schedule that leaves only about 40 percent circulating today.

NATIX Network is a decentralized physical infrastructure project that turns dashcam and phone camera footage from its Drive and app into fresh mapping and AI training data, rewarding contributors in the NATIX token. The token is a native Solana SPL asset at mint FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX, which is the same address published on the official site and confirmed live against the mainnet RPC. It trades on centralized venues such as Gate, KuCoin and MEXC and on Solana pools including Raydium and Orca.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 1 |
| Low | 2 |
| Informational | 1 |
| **Total** | **4** |

### Overall Risk Assessment: LOW

MEFAI onchain analysis places NATIX Network at 72 out of 100 (LOW risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | NATIX Network / NATIX |
| **Contract or program** | `FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX` |
| **Chain** | Solana |
| **Tags** | SPL, Mapping DePIN, Authorities Renounced, Solana, Passed |

Direct RPC reads show the mint uses the classic SPL Token program with six decimals, a null mint authority and a null freeze authority, so supply is fixed and no wallet can be frozen. Because it is a classic SPL mint and not a Token 2022 mint, it carries no transfer fee extension and no transfer hooks, so transfers are ordinary and predictable. The contract level risk is therefore low, and the residual risks are economic rather than technical, namely the large scheduled unlocks and liquidity depth that this read only review did not verify from chain.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- NATIX on Solana (mint FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX, 6 decimals) verified live: both the mint authority and the freeze authority are null, so no new tokens can be created and no wallet can be frozen. On chain supply is about 99.29 billion against a stated 100 billion cap, on a classic SPL token with no transfer fee.

---

## 3. Claim versus Reality

- "A 100 billion fixed supply DePIN token" / Reality: accurate and slightly conservative, since roughly 99.29 billion is already minted and the mint authority is renounced. The gap the marketing understates is float, since only about 40 percent circulates and close to 60 percent unlocks on a schedule into 2028.

The site advertises a 100 billion max supply, and the chain shows roughly 99.29 billion actually minted, so the public claim is accurate and slightly conservative rather than inflated. Because the mint authority is null, no new tokens can ever be created, so the practical ceiling is the amount already in existence. The one area where readers should look past the marketing is float, since independent trackers put circulating supply near 40 percent, meaning close to 60 percent still unlocks on a vesting schedule that stretches into 2028, which is a meaningful dilution consideration.

---

## 4. Website and Frontend Integrity

The official site is reachable and references audits, so confirm that any audit claim links to a real auditor. The token address is not printed in the static page, so confirm the official NATIX mint from the docs before interacting.


---

## 5. Findings by Severity

- MEDIUM: a large unlock overhang into 2028 (dilution). LOW: liquidity depth and pool locks were not verifiable on chain. INFO: both authorities renounced on a standard fee free SPL token (a strong contract level positive) and a real dashcam mapping product with exchange listings.

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 14 |
| Supply and minting | 11 |
| Liquidity and market | 10 |
| Code safety | 14 |
| Transfer neutrality | 15 |
| Transparency | 8 |
| **Total** | **72/100** |

---

## 7. Conclusion

Claim vs reality audit of NATIX Network (NATIX). A real Solana mapping DePIN whose SPL token has both mint and freeze authorities renounced, so supply is permanently fixed at the roughly 99.29 billion already minted against a 100 billion cap, with no transfer fee, the main consideration being a large unlock schedule that leaves only about 40 percent circulating today. On the MEFAI scale this token scores 72 out of 100 and is classified Passed.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Solana, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `explorer.solana FRySi...`
  - `natix.network/tokens`
  - `natix token launch blog`
  - `tokenomist.ai`
  - `cryptorank vesting`
  - `solana RPC.`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
