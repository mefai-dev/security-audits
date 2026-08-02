# Security Audit Report: Space and Time (SXT) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 2, 2026 |
| **Project** | Space and Time |
| **Token Symbol** | SXT |
| **Contract / Program** | `0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195` |
| **Chain** | Ethereum |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **74/100** |
| **Overall Risk** | **LOW** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Claim vs reality audit of Space and Time (SXT). A verifiable data layer token for apps and AI whose fixed 5 billion supply and absence of a mint function are confirmed on chain across three independent sources, on a non proxy, fee free contract, the main considerations being an active pauser and admin role and a large team and investor unlock schedule over four years.

Space and Time is the native utility and governance token of a verifiable data layer built for apps and AI. The token is a standard ERC20 on Ethereum at 0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195, with a mirror on Base. Public read only calls confirm the token identity on chain, name Space and Time, symbol SXT, eighteen decimals, and a fixed total supply of exactly five billion tokens, matching the official tokenomics. The contract carries no mint function, is not a proxy, and applies no transfer fee, but it is Pausable and retains active admin roles, which is the primary control consideration.

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

MEFAI onchain analysis places Space and Time at 74 out of 100 (LOW risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Space and Time / SXT |
| **Contract or program** | `0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195` |
| **Chain** | Ethereum |
| **Tags** | ERC 20, Verifiable Data AI, Governance Pausable, Ethereum, Passed |

The bytecode is a direct non proxy OpenZeppelin style ERC20 that includes Permit for gasless approvals and Votes for governance delegation. There is no mint, no burn, and no fee logic, so circulating supply cannot be inflated and transfers are untaxed. Control is role based, since owner reverts while the admin role and pauser role are present, and paused currently returns false, meaning transfers are live but a privileged role holder could pause them. Circulating supply is roughly 2.6 billion, about 52 percent of max, with the large team and investor tranches still vesting over four years, which is the main ongoing supply overhang to watch.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- SXT on Ethereum (0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195, 18 decimals) verified live: fixed total supply of exactly 5 billion with no mint function, a direct non proxy deployment, and no transfer fee. Control is role based, with an active admin role and pauser role present (not renounced) and the token currently unpaused; it also supports permit and governance voting extensions.

---

## 3. Claim versus Reality

- "A 5 billion fixed supply data and AI token" / Reality: confirmed precisely on chain, with the blog address matching both the RPC identity and the aggregator listing. The marketing underplays that transfers can be paused and admin roles remain in place, so controls are not renounced.

The official blog publishes a five billion maximum supply and a four year vesting outline with a fifteen percent cliff at month twelve for team and investors. The live chain data corroborates the headline supply precisely at five billion with eighteen decimals and shows no mint capability, so the fixed supply promise is verifiable rather than merely stated. The contract address on the official blog agrees with both the RPC identity and the aggregator listed address, giving three independent confirmations. The one thing the marketing underplays is that transfers can be paused and admin roles remain in place, so the token is not immutable or fully decentralized in its controls.

---

## 4. Website and Frontend Integrity

The official site is reachable and references audits, so confirm that any audit claim links to a real auditor. The token address is not printed in the static page, so confirm the official SXT address from the docs before interacting.


---

## 5. Findings by Severity

- MEDIUM: an active pauser and admin role can halt transfers and manage roles (a centralization vector). LOW: a large team and investor unlock schedule over four years (overhang); role holders not enumerable on chain. INFO: a fixed supply with no mint function on a non proxy, fee free contract (a strong positive).

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 10 |
| Supply and minting | 15 |
| Liquidity and market | 12 |
| Code safety | 13 |
| Transfer neutrality | 15 |
| Transparency | 9 |
| **Total** | **74/100** |

---

## 7. Conclusion

Claim vs reality audit of Space and Time (SXT). A verifiable data layer token for apps and AI whose fixed 5 billion supply and absence of a mint function are confirmed on chain across three independent sources, on a non proxy, fee free contract, the main considerations being an active pauser and admin role and a large team and investor unlock schedule over four years. On the MEFAI scale this token scores 74 out of 100 and is classified Passed.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Ethereum, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `spaceandtime.io/blog SXT token`
  - `docs.spaceandtime.io stake`
  - `etherscan 0xe6bfd3...`
  - `coingecko space-and-time`
  - `ethereum-rpc.publicnode.com`
  - `l2beat sxt.`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
