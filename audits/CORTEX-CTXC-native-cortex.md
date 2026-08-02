# Security Audit Report: Cortex (CTXC) on Cortex

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 2, 2026 |
| **Project** | Cortex |
| **Token Symbol** | CTXC |
| **Contract / Program** | `native-cortex` |
| **Chain** | Cortex |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis (read only public RPC) |
| **Mefai Security Score** | **73/100** |
| **Overall Risk** | **LOW** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Claim vs reality audit of Cortex (CTXC). A mature AI inference Layer 1 whose native coin has a fixed 299.79 million cap and no on chain mint, with a deprecated Ethereum mirror that is non upgradeable and fee free, the main considerations being a pause capable mirror owner and markedly reduced activity since the 2018 to 2020 peak.

Cortex is an AI focused layer one blockchain whose native coin CTXC pays for transactions and rewards miners and model contributors. The network went live with its Arnold mainnet on 26 June 2019 and holders of the original Ethereum token were told to convert to the native coin. The supply is capped at 299,792,458 units, a deliberate nod to the speed of light in metres per second, split between 150 million mined on a four year halving schedule and the remainder preallocated.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 1 |
| Low | 2 |
| Informational | 2 |
| **Total** | **5** |

### Overall Risk Assessment: LOW

MEFAI onchain analysis places Cortex at 73 out of 100 (LOW risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Cortex / CTXC |
| **Contract or program** | `native-cortex` |
| **Chain** | Cortex |
| **Tags** | Native, AI Inference Chain, Fixed Cap, Cortex, Passed |

Read read only over the public Ethereum RPC, the legacy mirror at 0xea11755ae41d889ceec39a63e6ff75a02bc1c00d returns the name Cortex Coin, symbol CTXC, eighteen decimals and a total supply of exactly 299,792,458 tokens. It has no mint function and does not inherit any mintable module, so that supply is fixed. The implementation slot is empty, meaning the contract is not an upgradeable proxy, and there is no fee on transfer. It does inherit the pausable module, so the owner, an externally owned account, can freeze transfers on the mirror, though the token currently reads as not paused.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- CTXC is the native coin of the Cortex chain (mainnet live since June 2019) with a fixed 299,792,458 hard cap (a speed of light reference) and emission split between mining on a four year halving and a preallocation. The legacy Ethereum mirror (0xea11755ae41d889ceec39a63e6ff75a02bc1c00d, 18 decimals) verified live as name Cortex Coin with the full 299,792,458 supply, no mint function, no proxy, and no transfer fee.

---

## 3. Claim versus Reality

- "The native asset of an AI inference public chain" / Reality: confirmed, with a mandatory swap from the Ethereum mirror to the native coin at the 2019 mainnet launch and supply figures that match exactly. The caveat is activity, since development pace and volume are far quieter than at the peak.

The project describes CTXC as the native asset of its own public chain, and independent public data agrees. Public announcements place the mainnet launch in June 2019 and document the mandatory swap away from the Ethereum token. Circulating supply reported by aggregators sits near 238 million, roughly 79 percent of the hard cap, which is consistent with a mining emission that is still ongoing. The only caveat is activity, since the codebase and trading volume are far quieter now than at the 2018 to 2020 peak.

---

## 4. Findings by Severity

- MEDIUM: the deprecated Ethereum mirror is pausable by an externally owned owner (a transfer freeze capability on the mirror, currently unpaused). LOW: reduced project activity and liquidity; the mirror should not be sent to a native chain address. INFO: a fixed, non mintable cap with no fee and a non proxy contract, plus open source code and a public explorer.

---

## 5. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 12 |
| Supply and minting | 14 |
| Liquidity and market | 11 |
| Code safety | 12 |
| Transfer neutrality | 15 |
| Transparency | 9 |
| **Total** | **73/100** |

---

## 6. Conclusion

Claim vs reality audit of Cortex (CTXC). A mature AI inference Layer 1 whose native coin has a fixed 299.79 million cap and no on chain mint, with a deprecated Ethereum mirror that is non upgradeable and fee free, the main considerations being a pause capable mirror owner and markedly reduced activity since the 2018 to 2020 peak. On the MEFAI scale this token scores 73 out of 100 and is classified Passed.

---

## 7. Verification

- Methodology: manual review plus onchain analysis using read only public RPC on Cortex.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `github CortexFoundation/tech-doc`
  - `etherscan 0xea11755...`
  - `medium mainnet token swap`
  - `medium Arnold launch`
  - `coingecko cortex`
  - `cerebro.cortexlabs.ai`
  - `ethereum-rpc.publicnode.com.`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
