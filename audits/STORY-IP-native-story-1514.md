# Security Audit Report: Story Protocol (IP) on Story

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 1, 2026 |
| **Project** | Story Protocol |
| **Token Symbol** | IP |
| **Native token** | IP (Story Layer 1, chain id 1514); canonical wrapped form WIP `0x1514000000000000000000000000000000000000` on Story |
| **Chain** | Story |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **64/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Story Protocol is a real, top tier venture backed intellectual property Layer 1 with a working chain and audited core contracts. It is not a scam. The MEDIUM rating reflects a gap between the "IP layer for AI" narrative and a concentrated, unlock heavy, early usage reality:

1. **The chain and backing are real:** MEFAI recognizes a working BFT Layer 1 with an EVM execution layer and strong venture backing, and a native IP token.
2. **But a large insider allocation is unlocking now.** Roughly 42 percent of supply is held by investors and the team, only about a third is circulating, and the six month delayed unlock cliff opens around the report date, creating heavy supply overhang.
3. **The "IP layer for AI" is largely narrative.** On chain, Story registers IP and runs licensing and royalty smart contracts, but it does not train models or stop off chain scraping; enforcement still depends on real world legal systems.
4. **Impostor IP tokens exist on other chains,** and the token is down roughly 98 percent from its peak.

The backing and chain are real; the caution is a heavy insider unlock cliff, an AI narrative ahead of substance, impostor token risk and a deep drawdown.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token** | IP (native gas, staking and governance on the Story Layer 1) |
| **Chain** | Story, a BFT Layer 1 with an EVM execution layer, chain id 1514 |
| **Decimals** | 18 |
| **Canonical wrapped form** | WIP `0x1514000000000000000000000000000000000000` on Story (used in DeFi) |
| **Supply** | Genesis 1,000,000,000 IP; ~1.01 billion now (grown via staking rewards); circulating roughly a third (about 31 to 35 percent by data source) |
| **Insider allocation** | ~41.6 percent (investors ~21.6 percent, core contributors 20 percent), unlock cliff opening around the report date |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI reviewed the IP token and Story chain:

| Check | Result |
|-------|--------|
| Chain identity | Story Layer 1, BFT consensus with an EVM execution layer, chain id 1514, verified |
| Native token | IP, 18 decimals; canonical wrapped form is WIP at `0x1514...0000` on Story |
| Supply | Genesis 1 billion, ~1.01 billion now, inflationary staking rewards (~1.5 to 2 percent per year) |
| Circulating | roughly a third (about 31 to 35 percent by data source); ~41.6 percent to insiders on multi year vesting with a cliff opening now |
| Validators | Small active set (recently reduced from 80 toward 21 by a network upgrade); permissionless entry |
| Impostor risk | Non canonical IP tokens exist on Ethereum (for example a token reporting a 1.5 trillion supply); only the native IP and WIP are genuine |

**Interpretation.** IP is a genuine native token on a working, audited Layer 1 with strong backing. The concerns are a concentrated insider allocation whose cliff is opening now, a small and recently reduced active validator set (from 80 toward 21), and impostor tokens on other chains that holders must avoid.

---

## 3. Claim vs Reality: "The IP Layer for AI"

> Site: the world's first blockchain for intellectual property, "the IP layer for AI", with programmable IP licensing, a proof of creativity framework, and AI training data provenance and automated royalties.

**Reality: real IP tooling, an AI narrative ahead of substance.** On chain, Story does IP registration, license terms smart contracts and royalty splits, which are genuine. But the "IP layer for AI" framing is largely narrative: the chain does not train models or enforce infringement off chain, and on chain registration cannot stop off chain AI scraping, so enforcement still depends on real world legal systems. Verifiable adoption is modest (on the order of a couple of hundred thousand monthly active users and low millions of IP transfers), earlier testnet figures were inflated by airdrop farming, and a transparent cumulative registered IP asset count and real licensing revenue are not clearly disclosed, a notable transparency gap for a project whose thesis is IP registration and licensing.

---

## 4. Claim vs Reality: Allocation, Decentralization and Value

- **Heavy insider unlock overhang:** roughly 41.6 percent to insiders with only about a third circulating, and a six month delayed unlock cliff opening around the report date, on top of mild staking inflation, into a token already down heavily.
- **Limited decentralization:** a small active validator set (recently reduced from 80 toward 21 by a network upgrade) with BFT means a small share of bonded stake can halt the chain; permissionless entry does not offset a thin active set, and the reduction concentrates it further.
- **Strong backing (positive):** genuinely backed by a top tier venture round at a multi billion dollar valuation, a real credibility signal versus anonymous projects.
- **Value:** the token is down roughly 98 percent from its 2025 peak.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| IP 001 | **MEDIUM** | ~41.6 percent insider allocation with only roughly a third circulating and an unlock cliff opening around the report date (heavy overhang). |
| IP 002 | **MEDIUM** | The "IP layer for AI" is largely narrative: on chain registration cannot stop off chain scraping; enforcement depends on real world law; usage is modest with limited transparency. |
| IP 003 | **LOW** | Small active validator set (recently reduced from 80 toward 21); mild staking inflation; ~98 percent drawdown. |
| IP 004 | **LOW** | Impostor IP tokens exist on other chains; only the native IP and WIP `0x1514...0000` are genuine. |
| IP 005 | **INFO** | Working, audited BFT Layer 1 with strong top tier venture backing (positives). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Working chain, audited, strong backing |
| Supply / unlocks | Medium to high risk | ~41.6 percent insider, cliff opening now |
| Decentralization | Medium risk | Small active validator set (reduced toward 21) |
| Claim accuracy | Medium risk | AI provenance narrative ahead of substance |
| Contract safety | Low to medium risk | Impostor tokens on other chains |
| Value / volatility | High risk | ~98 percent drawdown from peak |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Chain | Story Layer 1, chain id 1514, BFT with EVM execution |
| Native token | IP, 18 decimals |
| Canonical wrapped | WIP `0x1514000000000000000000000000000000000000` on Story |
| Circulating | roughly a third of ~1.01 billion |
| Validators | Small active set (reduced from 80 toward 21) |

---

## 8. Conclusion

Story Protocol is a real, top tier venture backed intellectual property Layer 1 with a working, audited BFT chain and a native IP token, which keeps it in the MEDIUM band at 64/100. It is held back because roughly 41.6 percent of supply sits with insiders and the six month delayed unlock cliff is opening around the report date with only about a third circulating, because the "IP layer for AI" framing is largely narrative (on chain registration cannot stop off chain scraping and usage is modest with limited transparency), because the validator set is small and recently reduced (from 80 toward 21), and because impostor IP tokens exist on other chains, against a roughly 98 percent drawdown. The backing and chain are real; the caution is the insider unlock cliff, an AI narrative ahead of substance and impostor token risk.

---

## 9. Recommendations

**For the Story team:**
- Publish transparent cumulative registered IP asset counts and real licensing and royalty volume, so the IP thesis is grounded in data.
- Present the unlock schedule and the insider allocation prominently.
- Clearly document the canonical token to protect holders from impostor IP tokens on other chains.

**For users:**
- Transact only the native IP and the WIP `0x1514000000000000000000000000000000000000`, never impostor IP tokens on Ethereum.
- Model the heavy insider unlock cliff and understand the AI provenance framing is ahead of demonstrated substance.

---

## 10. Verification

- MEFAI on chain analysis: review of the Story chain identity (chain id 1514, BFT with EVM execution), the native IP token and canonical WIP, the ~1.03 billion supply and staking inflation, the ~41.6 percent insider allocation and unlock schedule, and the small, recently reduced active validator set.
- The chain parameters, token and allocation are publicly verifiable on the Story explorers.
- Project statements: the project's website and documentation (the IP layer for AI, proof of creativity and programmable licensing framing) and the published tokenomics and unlock schedule.
