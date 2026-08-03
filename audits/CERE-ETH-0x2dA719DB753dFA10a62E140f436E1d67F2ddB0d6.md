# Security Audit Report: Cere Network (CERE) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 3, 2026 |
| **Project** | Cere Network |
| **Website** | https://cere.network |
| **Audit Type** | Whole Project (Claim versus Reality) |
| **Methodology** | Product and Traction Review + Website Frontend Review + Onchain Analysis |
| **Mefai Security Score** | **46/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Flagged** |
| **Token / Contract** | CERE `0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6` (Ethereum) |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

CERE Network (CERE) verifies live on Ethereum at 0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6 as a fixed supply ERC20 with 10 decimals and a total supply of ten billion tokens. The contract exposes no owner, no admin roles, and no public mint function, and its cap equals the current supply, so no additional tokens can ever be created. It is not a proxy and cannot be upgraded or paused, while holders may voluntarily burn their own balances. The website markets a decentralized data cloud and AI data infrastructure that live on the separate Cere Substrate chain, and the Ethereum ERC20 is simply the bridged representation, which stays clean and trustless at the token layer. Overall this is a low risk profile, with the only practical caution being the nonstandard 10 decimal precision.

**Product reality:** PARTIAL the Cere blockchain is live mainnet but the DDC remains beta and testnet in large part  |  **Traction:** UNVERIFIED claimed nodes clusters and storage usage lack public verifiable numbers  |  **Token utility:** LIMITED CERE is genuine chain gas and staking but broad economic usage is thin  |  **Delivery:** PARTIAL_GAP core infrastructure ships yet key pieces stay perpetually coming soon amid pivots

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 1 |
| Informational | 5 |
| **Total** | **6** |

MEFAI whole project analysis places Cere Network at 46 out of 100 (MEDIUM risk, Flagged).

---

## 1. Project and Product Reality

The Cere blockchain is a genuinely live Substrate and Polkadot SDK mainnet with active governance, staking, and a recent runtime upgrade referendum plus a LayerZero integration bringing CERE onto BASE. The GitHub organization Cerebellum Network is actively maintained in 2026, with the blockchain node, ddc api, ddc primitives, and DDC JavaScript SDK all updated within recent weeks, so this is not an abandoned codebase. The Decentralized Data Cloud exists as real technology, including a Dragon 1 beta cluster, an SDK playground, and storage node HTTP APIs, but much of it is still framed as testnet or beta. The public DDC statistics dashboard is described as coming soon and open node participation as very soon, so the fully decentralized productized DDC is only partially realized. Net assessment is a working chain paired with a still maturing data cloud layer.

**Project level flags:** very low trading volume and liquidity; DDC public stats dashboard still coming soon after years; open node participation still labeled very soon; multiple strategic pivots enterprise data cloud to consumer brand to AI agents DePIN; traction metrics unverifiable; token roughly 99.96 percent below all time high

---

## 2. Website and Frontend Integrity

VERDICT: CLEAN | CONFIDENCE: high
The Cere Network homepage is a standard Webflow marketing site that contains no Ethereum addresses at all in its HTML or JavaScript, so there is no lookalike or mismatched CERE contract address to worry about, and the official 0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6 token is simply not surfaced here. There is no wallet connect flow, no window.ethereum usage, and none of the drainer signatures such as eth_requestAccounts, personal_sign, signTypedData, approve, permit, setApprovalForAll, or transferFrom anywhere in its own bundles. The only scripts load from reputable hosts (Webflow CDN, jQuery via Cloudfront, Google reCAPTCHA, Brevo/sibforms newsletter forms, and Humblytics analytics), the single eval is Webflow's benign IX2 interaction expression engine and the single new Function is a routine globalThis polyfill, with no atob, base64 payload decoding, or untrusted remote code. The only form is a legitimate Brevo email signup posting to sibforms.com, there are no live token price or metrics widgets that could be faked or hardcoded, and no audit or security badges are claimed so none are unbacked. Note that wallet enabled subdomains like staking.cere.network are out of scope for this homepage only review.

---

## 3. Traction and Claims

Concrete usage numbers for storage nodes, clusters, and data stored are not publicly verifiable because the official DDC dashboard remains unreleased. Partnerships are a mix of dated 2021 era names such as Convergence Finance and Lithium Finance and newer AI framed ties like the Aethir AI Unbundled Alliance and a University of Toronto hackathon, none of which come with hard usage evidence. Market signals are weak, with a market cap around 1.35 million dollars and 24 hour trading volume near 1,170 dollars, indicating very thin real world adoption and liquidity. Marketing language around agentic AI data stacks outpaces any demonstrated production traffic. Overall the traction claims read as aspirational rather than measured.

---

## 4. Token Utility and Economics

CERE functions as the real native gas and staking asset of a live proof of stake chain, where validators and nominators stake and governance referenda execute, so it is more than a pure speculation token. It is also designed to pay for DDC storage and CDN usage, but that payment demand is unproven at scale and the tiny trading volume suggests minimal fee driven economic activity. The token sits roughly 99.96 percent below its November 2021 all time high, reflecting a collapsed market despite continued technical work. Utility is therefore genuine but narrow and lightly used in practice.

The cere.network site positions the project as a Decentralized Data Cloud and finance cloud for enterprises, with taglines such as Designed for the AI Data Era, Converging Web3 x AI, and Enabling True Data Decentralization. It describes validators acting as data cluster inspectors and frames the network as sovereign and trustless. Governance is presented through OpenGov and Polkassembly, which are Polkadot and Substrate tooling, confirming that the primary Cere network is its own chain rather than Ethereum. The landing page does not publish detailed tokenomics or the max supply, deferring those to a separate document. None of these product claims can be proven or disproven from the Ethereum ERC20, which is only the bridged token.

---

## 5. Claim versus Reality

- "Enabling True Data Decentralization" and "trustless data automation" / Reality: at the Ethereum token layer this holds, since the ERC20 has no admin, no mint, and no upgrade path, so no party can inflate or seize it; the data cloud itself is off chain and not verifiable from Ethereum.
- "Decentralized Data Cloud (DDC), Designed for the AI Data Era" / Reality: the DDC and AI features run on the separate Cere Substrate chain and off chain infrastructure, and the Ethereum contract is only a token that carries none of that logic.
- Max supply is ten billion CERE / Reality: confirmed onchain because cap() equals totalSupply; note that some trackers display max supply as unlimited, which is more alarming than the onchain truth of a hard cap.
- Governance token via OpenGov on the Cere chain / Reality: not verifiable from the Ethereum ERC20, which is a plain balance token with no voting logic.

---

## 6. Contract Security Check (supporting)

- Total supply is 10,000,000,000 CERE (raw 100000000000000000000 at 10 decimals) and cap() returns the identical value, so the supply is hard capped and cannot grow.
- No privileged key exists: owner(), getOwner(), DEFAULT_ADMIN_ROLE(), MINTER_ROLE(), PAUSER_ROLE(), and getRoleMemberCount() all revert, so there is no owner, admin, or AccessControl role.
- No emission: the mint(address,uint256) selector 0x40c10f19 is absent from the deployed bytecode, the entire supply was created at deployment, and further minting is impossible.
- Not upgradeable (EIP1967 implementation and admin slots are both zero, logic is inline, not a proxy), not pausable (paused() reverts, no pause selector), no fee on transfer (standard OpenZeppelin ERC20), decimals is 10, and ERC20Burnable burn 0x42966c68 lets holders burn their own tokens.

Reading the contract at 0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6 on Ethereum mainnet returns name CERE Network, symbol CERE, decimals 10, and totalSupply of 100000000000000000000 raw units, which equals ten billion CERE. A cap() call returns the identical raw value, proving an ERC20Capped design whose ceiling is already reached. Calls to owner(), getOwner(), and the AccessControl role getters all revert, and the mint selector 0x40c10f19 does not appear in the 2974 byte runtime, so no party can create new supply. The EIP1967 implementation and admin storage slots are both zero and the ERC20 logic is inline, confirming a standalone contract with no proxy and no pause function. The bytecode does contain the ERC20Burnable burn selector 0x42966c68, matching the project public token burn program, and shows no fee on transfer logic.

### Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 10 |
| Supply and minting | 8 |
| Liquidity and market | 2 |
| Code safety | 11 |
| Transfer neutrality | 13 |
| Transparency | 2 |
| **Total** | **46/100** |

---

## 7. Findings by Severity

- HIGH: none. MEDIUM: none. LOW: the nonstandard 10 decimal precision can cause integrations or dashboards that assume 18 decimals to misprice balances by a factor of one hundred million. INFO: no owner or admin key, no mint function, a hard cap equal to supply, not upgradeable, not pausable, and no fee on transfer are all strong positives.

---

## 8. Conclusion

CERE Network (CERE) verifies live on Ethereum at 0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6 as a fixed supply ERC20 with 10 decimals and a total supply of ten billion tokens. The contract exposes no owner, no admin roles, and no public mint function, and its cap equals the current supply, so no additional tokens can ever be created. It is not a proxy and cannot be upgraded or paused, while holders may voluntarily burn their own balances. The website markets a decentralized data cloud and AI data infrastructure that live on the separate Cere Substrate chain, and the Ethereum ERC20 is simply the bridged representation, which stays clean and trustless at the token layer. Overall this is a low risk profile, with the only practical caution being the nonstandard 10 decimal precision. On the MEFAI whole project scale this token scores 46 out of 100 and is classified Flagged.

---

## 9. Verification

- Methodology: product and traction review of the live project, a review of the project website frontend against its stated claims, and onchain analysis using read only public RPC on Ethereum.
- Sources:
  - `https://etherscan.io/token/0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6`
  - `https://www.coingecko.com/en/coins/cere-network`
  - `https://cere.network`
  - `https://www.gate.com/learn/articles/what-is-cere-network-all-you-need-to-know-about-cere/3289`
  - `https://www.cere.network/blog/important-announcement-cere-token-burn-and-validator-rewards-update`
  - `https://www.cere.network/`
  - `https://docs.cere.network/`
  - `https://github.com/Cerebellum-Network`
  - `https://coinmarketcap.com/currencies/cere-network/`
  - `https://t.me/s/cerenetwork`
  - `https://www.cere.network/hub/ddc`
  - `https://www.cere.network/blog/revolutionizing-cloud-storage-the-launch-of-ceres-dragon-1-beta-cluster-and-the-future-of-decentralized-data-management`
  - `https://ddc.cere.network/`

---

*Mefai Security Research. Independent whole project claim versus reality assessment.*
