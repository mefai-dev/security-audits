# Security Audit Report: Nillion (NIL) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 1, 2026 |
| **Project** | Nillion |
| **Token Symbol** | NIL |
| **Contract (Ethereum)** | `0x7Cf9a80db3B29eE8efE3710AadB7b95270572d47` |
| **Chain** | Ethereum (migrated from the former nilChain) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **50/100** |
| **Overall Risk** | **HIGH (for current holders)** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Nillion is a credible, well backed privacy computation project with real cryptography, but its token and network have just undergone a disruptive change: the original chain was abandoned and NIL is now a plain Ethereum token with no native staking utility. The rating is driven by that reality, not by a malicious contract:

1. **The contract is legitimate:** MEFAI recognizes a genuine Ethereum ERC 20 for NIL; it is not a honeypot.
2. **But the original chain was abandoned.** The Cosmos based nilChain was halted around March 2026, barely a year after launch, and NIL migrated to an Ethereum token. Staking is live again through the Blacklight program, but the model changed from open delegation to a higher barrier node operator model requiring a large minimum stake, with the full Layer 2 still rolling out.
3. **The decentralization is curated and the flagship AI is not what the marketing implies.** The validator set was closed, compute and storage nodes are run by a handful of large corporates, and the flagship private AI is hardware trusted execution based rather than the pure math based blind compute the marketing suggests.
4. **Uncapped inflationary supply, heavy insider unlocks and a collapse to all time lows** compound the holder risk.

The team and cryptography are real; the caution, and the HIGH risk for current holders, is a just abandoned chain, curated decentralization, a hardware trust model and uncapped supply with insider unlocks flowing.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | Nillion / NIL |
| **Contract (Ethereum, verified)** | `0x7Cf9a80db3B29eE8efE3710AadB7b95270572d47` |
| **Decimals** | 6 (both the Ethereum token and the former nilChain use 6 decimals) |
| **Supply** | ~1.01 billion NIL, uncapped and inflationary (Blacklight rewards on the order of 0.5 percent of supply per year, governance adjustable) |
| **Status** | Migrated from the halted nilChain to an Ethereum token; staking live through Blacklight under a changed, higher barrier model |
| **Contract type** | An upgradeable proxy, so the token logic is admin upgradeable (a centralization consideration) |
| **Insider allocation** | Team and backers ~41 percent, on multi year vesting after a one year cliff, now flowing |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified the NIL token:

| Check | Result |
|-------|--------|
| Token identity | NIL, a legitimate Ethereum ERC 20, 6 decimals (not a honeypot); an upgradeable proxy, verified |
| Supply | ~1.01 billion NIL, uncapped and inflationary |
| Origin chain | The former nilChain (a Cosmos based Layer 1) was halted around March 2026 |
| Migration | One to one migration to an Ethereum token via a permissionless claim contract (completed early 2026) |
| Staking | Live again through the Blacklight program, but under a higher barrier node operator model (a large minimum stake to run a verifier node); the full Layer 2 is still rolling out |
| Validators (former chain) | Permissioned and closed (genesis validators only); bonded stake and staking rewards were minimal at deprecation |

**Interpretation.** The NIL contract is legitimate (though an upgradeable proxy), but the network context is the critical fact: the original chain was abandoned barely a year after launch, and NIL migrated to an Ethereum token with a changed staking model (from open delegation to a higher barrier node operator model), with the full Layer 2 still rolling out. That, combined with uncapped supply and insider unlocks flowing, is a real holder risk despite a credible team.

---

## 3. Claim vs Reality: "The Blind Computer" and Decentralization

> Site: "the blind computer", blind computation via multi party computation and secret sharing so data is used without being exposed, and privacy preserving AI and storage on a permissionless, decentralized network.

**Reality: real cryptography, a curated network, a hardware trust model.** The research and products are genuine, but the flagship private AI inference is based on hardware trusted execution environments, not the pure information theoretic multi party computation the "math based blind compute" marketing implies, so it carries hardware and side channel trust assumptions the marketing glosses over. The "permissionless and decentralized" framing collides with a closed validator set and compute and storage nodes run by a handful of large corporate partners, and secret sharing clusters are small. Real usage is thin, with an unaudited marketing document counter and minimal on chain economics.

---

## 4. Claim vs Reality: The Chain Abandonment, Supply and Value

- **Abandoned chain and changed staking:** the Cosmos based nilChain was live barely a year before the team halted it and migrated to an Ethereum token, a fast pivot that signals failed traction for the original chain. Staking is live again through the Blacklight program, but the model changed from open delegation (any holder could delegate) to a higher barrier node operator model requiring a large minimum stake to run a verifier node, with the full Layer 2 still rolling out.
- **Uncapped inflationary supply with insider overhang:** supply is uncapped and inflationary, insiders (team and backers) hold roughly 41 percent, their cliff opened around March 2026, and circulating supply has grown sharply from launch, so sell side overhang is high.
- **Backers (positive):** the project has reputable venture and angel backing, and the migration was transparent and one to one, so this is a high uncertainty legitimate project, not a scam.
- **Value:** the token is down roughly 97 percent from its launch peak and printed a fresh all time low around the report date.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| NIL 001 | **HIGH** | The original nilChain was abandoned around March 2026 (barely a year after launch); NIL migrated to an Ethereum token with a changed, higher barrier staking model, the full Layer 2 still rolling out (holder risk). |
| NIL 002 | **MEDIUM** | Curated decentralization: a closed validator set and compute and storage nodes run by a handful of large corporates, against the permissionless framing. |
| NIL 003 | **MEDIUM** | The flagship private AI is hardware trusted execution based, not the pure math based blind compute the marketing implies; uncapped inflationary supply with ~41 percent insider unlocks flowing. |
| NIL 004 | **LOW** | ~97 percent drawdown to fresh all time lows; thin real usage. |
| NIL 005 | **INFO** | The NIL contract is a legitimate ERC 20 (not a honeypot); reputable backers and a transparent one to one migration (positives). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Legitimate ERC 20, reputable backers |
| Network status | High risk | Original chain abandoned, staking model changed |
| Decentralization | Medium to high risk | Closed validators, corporate run nodes |
| Claim accuracy | Medium risk | TEE based AI marketed as pure blind compute |
| Supply / unlocks | Medium to high risk | Uncapped inflation, ~41 percent insider unlocks |
| Value / volatility | High risk | ~97 percent drawdown to fresh all time lows |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Contract (Ethereum) | `0x7Cf9a80db3B29eE8efE3710AadB7b95270572d47` (an upgradeable proxy) |
| Decimals | 6 |
| Supply | ~1.01 billion NIL, uncapped and inflationary |
| Status | Migrated from the halted nilChain; staking live via Blacklight under a higher barrier model |
| Insider allocation | ~41 percent (team and backers), unlocks now flowing |

---

## 8. Conclusion

Nillion is a credible, well backed privacy computation project with real cryptography, but its NIL token scores 50/100 because of a disruptive network reality, not a malicious contract. The original Cosmos based chain was abandoned barely a year after launch, and NIL migrated to an Ethereum token with a changed, higher barrier staking model (staking is live through Blacklight but requires running a node with a large minimum stake), the full Layer 2 still rolling out. Decentralization is curated (a closed validator set and corporate run nodes), the flagship private AI is hardware trusted execution based rather than the pure blind compute the marketing implies, and uncapped inflationary supply with roughly 41 percent insider unlocks now flowing compounds the risk, against a roughly 97 percent drawdown to fresh all time lows. The team and cryptography are real; the caution, and the HIGH risk for current holders, is the abandoned chain, curated decentralization, a hardware trust model and uncapped supply with insider unlocks.

---

## 9. Recommendations

**For the Nillion team:**
- Deliver and clearly communicate the full Layer 2 rollout on a firm timeline, and make staking accessible beyond the higher barrier node operator model.
- Reframe the private AI honestly as hardware trusted execution based where applicable, not pure information theoretic blind compute.
- Present the uncapped inflation and the insider unlock schedule prominently.

**For users:**
- Understand NIL migrated to Ethereum after its original chain was abandoned, that staking now requires a higher barrier node operator model, and that supply is uncapped with insider unlocks flowing.
- Treat the private AI as hardware trust based, and model the heavy overhang and the drawdown; the credible team does not offset the current network and supply risk.

---

## 10. Verification

- MEFAI on chain analysis: a read of the NIL token on Ethereum (identity, legitimate ERC 20, ~1.007 billion uncapped inflationary supply) and review of the nilChain halt and one to one migration, the closed validator set and the insider unlock schedule.
- The contract address, supply and migration are publicly verifiable on the Ethereum explorers and the project's migration guide.
- Project statements: the project's own pages and migration guide (the blind computer, multi party computation and privacy preserving AI framing, the chain halt and Ethereum migration), and the public record of the corporate run node partners and the trusted execution based AI.
