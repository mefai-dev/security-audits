# Security Audit Report: Cere Network (CERE) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 3, 2026 |
| **Project** | Cere Network |
| **Token Symbol** | CERE |
| **Contract / Program** | `0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6` |
| **Chain** | Ethereum |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **78/100** |
| **Overall Risk** | **LOW** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

CERE Network (CERE) verifies live on Ethereum at 0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6 as a fixed supply ERC20 with 10 decimals and a total supply of ten billion tokens. The contract exposes no owner, no admin roles, and no public mint function, and its cap equals the current supply, so no additional tokens can ever be created. It is not a proxy and cannot be upgraded or paused, while holders may voluntarily burn their own balances. The website markets a decentralized data cloud and AI data infrastructure that live on the separate Cere Substrate chain, and the Ethereum ERC20 is simply the bridged representation, which stays clean and trustless at the token layer. Overall this is a low risk profile, with the only practical caution being the nonstandard 10 decimal precision.

The Ethereum CERE token is one of the cleaner ERC20 profiles a directory can list. Every core fact verified live on public RPC: the name is CERE Network, the symbol is CERE, decimals are 10, and total supply is ten billion tokens. There is no owner, no minter, and no admin role, and the cap already equals the circulating and total supply, so inflation is impossible. The contract is a standalone token with no proxy, it cannot be upgraded or paused, and the only holder facing power is voluntary burning. MEFAI rates this a PASS with the single caveat that the 10 decimal precision is unusual and integrators should never assume 18.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 1 |
| Informational | 5 |
| **Total** | **6** |

### Overall Risk Assessment: LOW

MEFAI onchain analysis places Cere Network at 78 out of 100 (LOW risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Cere Network / CERE |
| **Contract or program** | `0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6` |
| **Chain** | Ethereum |
| **Tags** | ERC 20, Decentralized Data Cloud, Ethereum, Passed |

Reading the contract at 0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6 on Ethereum mainnet returns name CERE Network, symbol CERE, decimals 10, and totalSupply of 100000000000000000000 raw units, which equals ten billion CERE. A cap() call returns the identical raw value, proving an ERC20Capped design whose ceiling is already reached. Calls to owner(), getOwner(), and the AccessControl role getters all revert, and the mint selector 0x40c10f19 does not appear in the 2974 byte runtime, so no party can create new supply. The EIP1967 implementation and admin storage slots are both zero and the ERC20 logic is inline, confirming a standalone contract with no proxy and no pause function. The bytecode does contain the ERC20Burnable burn selector 0x42966c68, matching the project public token burn program, and shows no fee on transfer logic.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- Total supply is 10,000,000,000 CERE (raw 100000000000000000000 at 10 decimals) and cap() returns the identical value, so the supply is hard capped and cannot grow.
- No privileged key exists: owner(), getOwner(), DEFAULT_ADMIN_ROLE(), MINTER_ROLE(), PAUSER_ROLE(), and getRoleMemberCount() all revert, so there is no owner, admin, or AccessControl role.
- No emission: the mint(address,uint256) selector 0x40c10f19 is absent from the deployed bytecode, the entire supply was created at deployment, and further minting is impossible.
- Not upgradeable (EIP1967 implementation and admin slots are both zero, logic is inline, not a proxy), not pausable (paused() reverts, no pause selector), no fee on transfer (standard OpenZeppelin ERC20), decimals is 10, and ERC20Burnable burn 0x42966c68 lets holders burn their own tokens.

---

## 3. Claim versus Reality

- "Enabling True Data Decentralization" and "trustless data automation" / Reality: at the Ethereum token layer this holds, since the ERC20 has no admin, no mint, and no upgrade path, so no party can inflate or seize it; the data cloud itself is off chain and not verifiable from Ethereum.
- "Decentralized Data Cloud (DDC), Designed for the AI Data Era" / Reality: the DDC and AI features run on the separate Cere Substrate chain and off chain infrastructure, and the Ethereum contract is only a token that carries none of that logic.
- Max supply is ten billion CERE / Reality: confirmed onchain because cap() equals totalSupply; note that some trackers display max supply as unlimited, which is more alarming than the onchain truth of a hard cap.
- Governance token via OpenGov on the Cere chain / Reality: not verifiable from the Ethereum ERC20, which is a plain balance token with no voting logic.

The cere.network site positions the project as a Decentralized Data Cloud and finance cloud for enterprises, with taglines such as Designed for the AI Data Era, Converging Web3 x AI, and Enabling True Data Decentralization. It describes validators acting as data cluster inspectors and frames the network as sovereign and trustless. Governance is presented through OpenGov and Polkassembly, which are Polkadot and Substrate tooling, confirming that the primary Cere network is its own chain rather than Ethereum. The landing page does not publish detailed tokenomics or the max supply, deferring those to a separate document. None of these product claims can be proven or disproven from the Ethereum ERC20, which is only the bridged token.

---

## 4. Website and Frontend Integrity

VERDICT: CLEAN | CONFIDENCE: high
The Cere Network homepage is a standard Webflow marketing site that contains no Ethereum addresses at all in its HTML or JavaScript, so there is no lookalike or mismatched CERE contract address to worry about, and the official 0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6 token is simply not surfaced here. There is no wallet connect flow, no window.ethereum usage, and none of the drainer signatures such as eth_requestAccounts, personal_sign, signTypedData, approve, permit, setApprovalForAll, or transferFrom anywhere in its own bundles. The only scripts load from reputable hosts (Webflow CDN, jQuery via Cloudfront, Google reCAPTCHA, Brevo/sibforms newsletter forms, and Humblytics analytics), the single eval is Webflow's benign IX2 interaction expression engine and the single new Function is a routine globalThis polyfill, with no atob, base64 payload decoding, or untrusted remote code. The only form is a legitimate Brevo email signup posting to sibforms.com, there are no live token price or metrics widgets that could be faked or hardcoded, and no audit or security badges are claimed so none are unbacked. Note that wallet enabled subdomains like staking.cere.network are out of scope for this homepage only review.


---

## 5. Findings by Severity

- HIGH: none. MEDIUM: none. LOW: the nonstandard 10 decimal precision can cause integrations or dashboards that assume 18 decimals to misprice balances by a factor of one hundred million. INFO: no owner or admin key, no mint function, a hard cap equal to supply, not upgradeable, not pausable, and no fee on transfer are all strong positives.

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 14 |
| Supply and minting | 15 |
| Liquidity and market | 11 |
| Code safety | 13 |
| Transfer neutrality | 15 |
| Transparency | 10 |
| **Total** | **78/100** |

---

## 7. Conclusion

CERE Network (CERE) verifies live on Ethereum at 0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6 as a fixed supply ERC20 with 10 decimals and a total supply of ten billion tokens. The contract exposes no owner, no admin roles, and no public mint function, and its cap equals the current supply, so no additional tokens can ever be created. It is not a proxy and cannot be upgraded or paused, while holders may voluntarily burn their own balances. The website markets a decentralized data cloud and AI data infrastructure that live on the separate Cere Substrate chain, and the Ethereum ERC20 is simply the bridged representation, which stays clean and trustless at the token layer. Overall this is a low risk profile, with the only practical caution being the nonstandard 10 decimal precision. On the MEFAI scale this token scores 78 out of 100 and is classified Passed.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Ethereum, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `https://etherscan.io/token/0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6`
  - `https://www.coingecko.com/en/coins/cere-network`
  - `https://cere.network`
  - `https://www.gate.com/learn/articles/what-is-cere-network-all-you-need-to-know-about-cere/3289`
  - `https://www.cere.network/blog/important-announcement-cere-token-burn-and-validator-rewards-update`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
