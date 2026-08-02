# Security Audit Report: Oasis Network (ROSE) on Oasis

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 2, 2026 |
| **Project** | Oasis Network |
| **Token Symbol** | ROSE |
| **Contract / Program** | `native-oasis` |
| **Chain** | Oasis |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **67/100** |
| **Overall Risk** | **LOW** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Claim vs reality audit of Oasis Network (ROSE). A legitimate privacy focused Layer 1 with a fixed 10 billion native token, no owner key, no arbitrary mint, and no transfer fee, whose confidential compute and AI data products are real and shipping, but whose privacy relies on Intel trusted execution hardware rather than pure cryptography, and whose history includes a large insider allocation and a heavily backloaded emission.

Oasis Network is a privacy focused Layer 1 that launched its mainnet in 2020 with a fixed maximum supply of ten billion ROSE. ROSE is a native consensus layer token, not an ERC20, so there is no issuer contract, no owner key, and no arbitrary mint function. New ROSE enters circulation only as bounded staking rewards, capped near 2.3 billion tokens and paid to validators and delegators who secure the proof of stake network. Governance runs on chain through validator voting and is stewarded by the Oasis Protocol Foundation, as shown by the Damask upgrade that passed with more than 88 percent validator support and by later proposals that adjusted reward parameters. The token distribution allocated roughly 43 percent to backers and core contributors and released only about 6.79 percent in the first year, so the ten year schedule was heavily backloaded and created years of unlock overhang, most of which is now behind the market with circulating supply near 78 percent.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 2 |
| Low | 3 |
| Informational | 2 |
| **Total** | **7** |

### Overall Risk Assessment: LOW

MEFAI onchain analysis places Oasis Network at 67 out of 100 (LOW risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Oasis Network / ROSE |
| **Contract or program** | `native-oasis` |
| **Chain** | Oasis |
| **Tags** | Native, Privacy L1, Confidential Compute, Oasis, Passed |

The canonical asset is native ROSE, which we treat as the object of audit; it exposes no owner, no pause, no freeze, and no fee on transfer, and its only emission path is the capped staking reward mechanism controlled by consensus parameters and governance. Because ROSE also circulates on the EVM ParaTimes, we verified the wrapped forms live over RPC. The canonical Sapphire WrappedROSE at 0x8Bc2B030b299964eEfb5e1e0b36991352E56D2D3 returned name Wrapped ROSE, symbol wROSE, eighteen decimals, and a total supply near 14.42 million, and its source is a minimal WETH style wrapper built on ERC20 and ERC20Burnable with only deposit and withdraw, no admin, no mint authority, no upgrade path, and no transfer fee. The Ethereum Wormhole representation at 0x26B80FBfC01b71495f477d5237071242e0d959d7 verified with the same name, symbol, and decimals and a small total supply near 828 thousand, and it is a thinly used bridge asset whose mint authority sits with the bridge rather than with Oasis. Net assessment is a legitimate fixed supply Layer 1 with sound token mechanics, whose main residual risks are hardware trust for privacy, historical insider and unlock concentration, modest liquidity, and third party bridge exposure on the wrapped versions.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- ROSE is a native consensus layer token with a fixed 10,000,000,000 cap, no issuer contract, no owner key, and no arbitrary mint; new supply enters only as capped staking rewards near 2.3 billion. The canonical Sapphire Wrapped ROSE (0x8Bc2B030b299964eEfb5e1e0b36991352E56D2D3) verified live as a minimal WETH style wrapper with no admin, no mint authority, no upgrade path, and no fee.
- Governance runs on chain through validator voting stewarded by the Oasis Foundation; the Damask upgrade passed with more than 88 percent validator support.

---

## 3. Claim versus Reality

- "A privacy first Layer 1 for confidential compute and responsible AI" / Reality: Sapphire is a production confidential EVM and the ROFL mainnet adds verifiable offchain compute for AI, but confidentiality rests on Intel trusted execution hardware, a real centralized trust and side channel assumption, which Oasis backs with an open bounty.
- The token allocated roughly 43 percent to backers and core contributors and released only about 6.79 percent in year one, so the schedule was heavily backloaded.

Oasis presents itself as infrastructure for confidential compute and responsible AI, and the substance largely matches the marketing. Sapphire is a production confidential EVM that keeps smart contract state private using trusted execution environments, and the ROFL mainnet launched in July 2025 extends this to verifiable offchain compute for AI workloads such as model training and inference on sensitive data. The honest caveat is that this privacy is enforced by Intel SGX and TDX hardware rather than by cryptography alone, which introduces a hardware trust and side channel assumption. Oasis addresses this openly with a public TEE Break Challenge bounty. The positioning is credible and the products are shipping, but a reader should understand that the privacy guarantee depends on enclave security.

---

## 4. Website and Frontend Integrity

VERDICT: CLEAN | CONFIDENCE: high
The official Oasis site is a static marketing brochure that resolves at oasis.net after a clean redirect from oasisprotocol.org to the foundation's own current domain, which is expected rebranding rather than any takeover. The frontend contains no wallet or contract addresses at all, so there is no target for a clipboard or address swap attack, and no drainer signatures, no obfuscated or remotely injected code, and none of the approval, permit, or transaction signing calls a wallet drainer would need were found. The only crypto touch point is an optional helper that offers to add the Sapphire network to MetaMask, which is a read only network configuration prompt that cannot move or approve any funds. Every third party script loads from a canonical trusted host such as the analytics provider, the animation library, the form provider, Cloudflare Turnstile, and the site builder network, and the page shows no live metrics dashboards or unbacked audit badges that could mislead visitors. What the marketing says and what the code does line up cleanly, so from a website and frontend integrity standpoint this reads as safe for end users.


---

## 5. Findings by Severity

- MEDIUM: privacy depends on trusted execution hardware integrity rather than cryptography alone; historical insider concentration and backloaded emission overhang. LOW: modest liquidity; third party bridge custody risk on wrapped forms. INFO: a fixed 10 billion supply with no owner, no arbitrary mint, and no transfer fee (a strong positive) and shipping confidential compute products.

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 11 |
| Supply and minting | 13 |
| Liquidity and market | 9 |
| Code safety | 11 |
| Transfer neutrality | 15 |
| Transparency | 8 |
| **Total** | **67/100** |

---

## 7. Conclusion

Claim vs reality audit of Oasis Network (ROSE). A legitimate privacy focused Layer 1 with a fixed 10 billion native token, no owner key, no arbitrary mint, and no transfer fee, whose confidential compute and AI data products are real and shipping, but whose privacy relies on Intel trusted execution hardware rather than pure cryptography, and whose history includes a large insider allocation and a heavily backloaded emission. On the MEFAI scale this token scores 67 out of 100 and is classified Passed.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Oasis, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `docs.oasis.io/token-metrics`
  - `oasis.net Damask blog`
  - `github WrappedROSE.sol`
  - `docs.oasis.io/sapphire/addresses`
  - `etherscan 0x26B80...`
  - `thedefiant ROFL mainnet`
  - `cryptorank vesting`
  - `coinmarketcap.`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
