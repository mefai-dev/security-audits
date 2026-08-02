# Security Audit Report: Roam (ROAM) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 2, 2026 |
| **Project** | Roam |
| **Token Symbol** | ROAM |
| **Contract / Program** | `RoamA1USA8xjvpTJZ6RvvxyDRzNh6GCA1zVGKSiMVkn` |
| **Chain** | Solana |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **50/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Flagged** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Claim vs reality audit of Roam (ROAM). A legitimate decentralized wireless DePIN, formerly MetaBlox, with a real product and broad exchange listings on a clean standard SPL mint with no transfer fee, but both the mint authority and the freeze authority remain active and not renounced, so the advertised one billion cap is a policy statement rather than an enforced onchain limit and the issuer can also freeze individual accounts.

Roam (ROAM) is a decentralized wireless DePIN project, formerly known as MetaBlox, that operates a global WiFi and OpenRoaming network and migrated its token onto Solana Mainnet. The ROAM SPL token uses the mint address RoamA1USA8xjvpTJZ6RvvxyDRzNh6GCA1zVGKSiMVkn, which was confirmed live through a direct Solana RPC query and matches the address published by CoinGecko, Solana Compass, CoinCarp, and multiple exchanges. The project has a real working product with a stated network of more than 100,000 nodes and broad listings across major venues including Bybit, Bitget, Gate, KuCoin, and MEXC.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 1 |
| Medium | 1 |
| Low | 1 |
| Informational | 1 |
| **Total** | **4** |

### Overall Risk Assessment: MEDIUM

MEFAI onchain analysis places Roam at 50 out of 100 (MEDIUM risk, Flagged).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Roam / ROAM |
| **Contract or program** | `RoamA1USA8xjvpTJZ6RvvxyDRzNh6GCA1zVGKSiMVkn` |
| **Chain** | Solana |
| **Tags** | SPL, Wireless DePIN, Mint Freeze Active, Solana, Flagged |

The direct RPC read shows the mint is owned by the classic SPL Token program, not the newer Token 2022 standard, and the account uses the standard mint layout with no extensions, so there is no transfer fee and no transfer hook. Decimals are set to six and the reported supply is about 995.63 million ROAM. Both authorities are still present, since the mint authority and the freeze authority are both populated, which means the issuer retains the ability both to mint additional tokens and to freeze individual holder accounts.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- ROAM on Solana (mint RoamA1USA8xjvpTJZ6RvvxyDRzNh6GCA1zVGKSiMVkn, 6 decimals) verified live: supply about 995.63 million, a classic SPL token with no transfer fee and no Token 2022 extensions. Both authorities are still populated, the mint authority is DqeBtBQ5Ue4Ms7kjJ9kienccL8piCbiLJRfY3Dp7dRzJ and the freeze authority is 6oSNcJSzSr7UcAcJDKdD3N2tiu2TL6KGVC2etFS3HaM1.

---

## 3. Claim versus Reality

- "A fixed one billion supply wireless DePIN" / Reality: the one billion figure and the roughly 996 million minted match trackers, but the cap is a policy statement rather than an enforced onchain limit because the mint authority is still live and could exceed it.
- The project is real, with a working WiFi and OpenRoaming network and listings on major venues.

The website and documentation describe a fixed maximum supply of 1,000,000,000 ROAM, split roughly as sixty percent to growth, mining, and community, twenty eight percent to investors, and twelve percent to the team under a six year vesting plan, plus an emission curve that starts near 0.6 percent monthly. Public chain data lines up closely with these claims on the headline numbers: the total minted supply reads about 995.63 million, which sits just under the stated one billion cap, and public trackers report circulating supply near 358 million, or roughly 36 percent. The one important caveat is that the one billion cap is a policy statement rather than an enforced onchain limit, because the mint authority remains active and could in principle issue tokens beyond the advertised ceiling.

---

## 4. Website and Frontend Integrity

The address roam.xyz did not resolve from our node during this review. Roam presents mainly through roam.network and its mobile apps. Because the mint is not exposed on a single canonical page, confirm the official ROAM mint from the project docs before trading.


---

## 5. Findings by Severity

- HIGH: the mint authority is not renounced (supply is not cryptographically fixed at the advertised cap). MEDIUM: the freeze authority is not renounced (the issuer can freeze individual holder accounts). LOW: circulating and unlock figures vary across trackers. INFO: a real DePIN product with a clean standard SPL mint and no transfer fee or hook.

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 6 |
| Supply and minting | 6 |
| Liquidity and market | 10 |
| Code safety | 12 |
| Transfer neutrality | 12 |
| Transparency | 4 |
| **Total** | **50/100** |

---

## 7. Conclusion

Claim vs reality audit of Roam (ROAM). A legitimate decentralized wireless DePIN, formerly MetaBlox, with a real product and broad exchange listings on a clean standard SPL mint with no transfer fee, but both the mint authority and the freeze authority remain active and not renounced, so the advertised one billion cap is a policy statement rather than an enforced onchain limit and the issuer can also freeze individual accounts. On the MEFAI scale this token scores 50 out of 100 and is classified Flagged.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Solana, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `solana RPC`
  - `coingecko roam-token`
  - `solanacompass roam`
  - `coincarp roam`
  - `roamnetwork medium migration`
  - `gate.com roam supply.`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
