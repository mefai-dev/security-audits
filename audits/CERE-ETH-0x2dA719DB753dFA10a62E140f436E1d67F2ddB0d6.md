# Security Audit Report: Cere Network (CERE) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Cere Network |
| **Token Symbol** | CERE |
| **Contract (Ethereum)** | `0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6` |
| **Chain** | Ethereum ERC 20 (also the native gas token of the Cere Substrate Layer 1) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **46/100** |
| **Overall Risk** | **HIGH** |
| **Verdict** | **Flagged** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Cere Network markets itself as a Decentralized Data and Finance Cloud built for the AI data era, but the audit finds a wide gap between that enterprise grade story and what is actually live and used. The token contract is genuinely clean and the base chain is real, yet the product that would justify the data cloud narrative is still beta and testnet, and the market around the token is close to dead.

1. **The token contract is exemplary, and that is the good news.** The Ethereum CERE ERC 20 is a fixed, hard capped, ownerless token with no mint function, no admin key, no upgrade path, and no pause switch. At the token layer there is nothing to inflate, seize, or rug.
2. **The flagship data cloud is marketed as production but runs as a beta.** The Decentralized Data Cloud (DDC) exists as real technology, including a Dragon 1 beta cluster and an SDK playground, but much of it is still framed as testnet or beta, the public DDC statistics dashboard has been coming soon for a long time, and open node participation is still labeled very soon.
3. **The market is near dead.** MEFAI reads a market capitalization of roughly 1.35 million dollars with 24 hour trading volume near 1,170 dollars, and the token sits roughly 99.96 percent below its November 2021 all time high. This is a collapsed, thinly traded market, not a thriving AI data economy.
4. **Traction is unverifiable and utility is thin.** Claimed nodes, clusters, and storage usage lack public verifiable numbers, partnerships mix dated 2021 era names with newer AI framed ties that carry no hard usage evidence, and the project has pivoted repeatedly from enterprise data cloud to consumer brand to AI agents and DePIN. CERE is real chain gas and staking, but broad economic usage is minimal.

The contract is not a scam. The concern is the opposite of a token exploit: a clean token wrapped around a project whose data cloud is not yet the production system the marketing implies, and whose market has largely evaporated. A clean contract does not save a near dead project, and this lands Cere at 46 out of 100, Flagged.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | CERE Network / CERE |
| **Contract (Ethereum)** | `0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6` |
| **Decimals** | 10 (nonstandard) |
| **Total supply** | 10,000,000,000 CERE (hard cap, verified equal to the onchain cap) |
| **History** | The primary Cere network is its own Substrate and Polkadot SDK Layer 1; the Ethereum ERC 20 is the bridged representation of CERE |
| **Contract controls** | None. No owner, no admin role, no minter, no pauser; not a proxy and not upgradeable |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the CERE contract on Ethereum returned:

| Check | Result |
|-------|--------|
| Token identity | CERE Network, CERE, 10 decimals, verified |
| Total supply | 10,000,000,000 CERE, `cap()` returns the identical value |
| Mint authority | None; the `mint(address,uint256)` selector `0x40c10f19` is absent from the deployed bytecode |
| Owner / admin | None; `owner()`, `getOwner()`, and the AccessControl role getters all revert |
| Upgradeable | No; the EIP 1967 implementation and admin slots are both zero, logic is inline, not a proxy |
| Pause authority | None; `paused()` reverts and there is no pause selector |
| Transfer fee | None (standard OpenZeppelin ERC 20) |
| Burn | ERC20Burnable `0x42966c68` present; holders may voluntarily burn their own balances |

**Interpretation.** At the contract level CERE is one of the cleaner ERC 20 profiles a directory can list. It is hard capped with the cap already equal to total supply, so inflation is impossible; it has no owner, no admin, and no minter, so no party can seize or dilute it; and it is neither upgradeable nor pausable, so its behavior cannot be silently changed. The only practical contract level caution is the nonstandard 10 decimal precision, which can trip integrations that assume 18 decimals. Crucially, the risk in this report does not live in the token contract. It lives at the project and claim level below, where a production grade data cloud story meets a beta reality and a near dead market.

---

## 3. Claim vs Reality: "The Decentralized Data & Finance Cloud for Enterprises"

> Site: Cere presents itself as "The Decentralized Data & Finance Cloud for Enterprises," "DESIGNED FOR THE AI DATA ERA," and "ADOPTION READY," with features described as currently operational.

**Reality: a production pitch over a beta and testnet product.** The DDC is real technology and not vaporware, with a Dragon 1 beta cluster, an SDK playground, and storage node HTTP APIs, and the Cerebellum Network GitHub organization is actively maintained in 2026. But much of the data cloud is still framed as beta or testnet, the fully decentralized and productized DDC is only partially realized, and the homepage carries no beta or testnet qualifier while presenting the cloud as enterprise ready. Marketing an adoption ready enterprise cloud while the core product is still a beta cluster is the central gap in this report.

---

## 4. Claim vs Reality: "Enabling True Data Decentralization" and Open Node Participation

> Site: Cere claims it is "ENABLING TRUE DATA DECENTRALIZATION," "Powering and Securing Fully Autonomous & Sovereign Data Clusters," and champions "a truly decentralized network."

**Reality: perpetual coming soon on the pieces that would prove decentralization.** The public DDC statistics dashboard that would let anyone verify decentralization has been described as coming soon for a long time, and open node participation, the mechanism by which outside operators would actually decentralize the cloud, is still labeled very soon. Until the stats dashboard ships and open node participation opens, the decentralization claim rests on the project's word rather than on numbers a reviewer can check. Presenting true decentralization as achieved while the tooling to demonstrate it remains unreleased is a transparency concern.

---

## 5. Claim vs Reality: CERE Utility and Market Health

> Site: CERE is positioned as the fuel of a live data cloud and finance economy, the asset that pays for DDC storage and CDN usage and secures the network.

**Reality: genuine but narrow utility on top of a near dead market.** CERE is the real native gas and staking asset of a live proof of stake chain, and it is designed to pay for DDC storage and CDN usage, so it is more than a pure speculation token. But the demand is unproven at scale, and the market tells the story: MEFAI reads a market capitalization of roughly 1.35 million dollars, 24 hour trading volume near 1,170 dollars, and a price roughly 99.96 percent below the November 2021 all time high. Volume of around a thousand dollars a day is not the signature of a thriving AI data economy, it is the signature of a collapsed and illiquid market. The token utility is real but lightly used, and the fee driven activity the marketing implies is not visible in the numbers.

---

## 6. Claim vs Reality: Traction, Partnerships, and Focus

> Site: Cere highlights an AI data ecosystem, converging Web3 and AI, and a roster of partners and integrations as proof of traction.

**Reality: unverifiable usage, mixed partnerships, and repeated pivots.** Concrete usage numbers for storage nodes, clusters, and data stored are not publicly verifiable because the official DDC dashboard remains unreleased. The partnership list mixes dated 2021 era names, such as Convergence Finance and Lithium Finance, with newer AI framed ties, such as the Aethir AI Unbundled Alliance and a University of Toronto hackathon, none of which come with hard usage evidence. The project has also cycled through strategic identities, from enterprise data cloud to consumer brand to AI agents and DePIN, with key pieces staying perpetually coming soon amid the pivots. Momentum by announcement, without measurable production traffic, is a recurring transparency and delivery gap.

---

## 7. Positive Findings (Credited)

- The Ethereum CERE token contract is exemplary: fixed and hard capped with the cap equal to total supply, fully ownerless with no admin key, no mint function in the bytecode, not upgradeable, not pausable, and no fee on transfer.
- The base Cere network is a genuinely live Substrate and Polkadot SDK mainnet with active governance, staking, a recent runtime upgrade referendum, and a LayerZero integration bringing CERE onto Base.
- The codebase is not abandoned: the Cerebellum Network GitHub organization is actively maintained in 2026, with the blockchain node, ddc api, ddc primitives, and DDC JavaScript SDK all updated within recent weeks.
- The DDC exists as real technology, including a Dragon 1 beta cluster and an SDK playground, rather than being pure vaporware.

---

## 8. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| CERE 001 | **HIGH** | Near dead market: market capitalization around 1.35 million dollars, 24 hour volume near 1,170 dollars, price roughly 99.96 percent below all time high. |
| CERE 002 | **HIGH** | Flagship DDC marketed as an enterprise, adoption ready production cloud while much of it remains beta and testnet with no verifiable usage numbers. |
| CERE 003 | **MEDIUM** | Perpetual coming soon: the public DDC statistics dashboard and open node participation remain unreleased, so decentralization and usage cannot be independently verified. |
| CERE 004 | **MEDIUM** | Unverifiable traction: partnerships mix dated 2021 names with AI framed ties, none with hard usage evidence; no public production metrics. |
| CERE 005 | **MEDIUM** | Repeated strategic pivots from enterprise data cloud to consumer brand to AI agents and DePIN, with key pieces staying coming soon across the pivots. |
| CERE 006 | **LOW** | Nonstandard 10 decimal precision can cause integrations or dashboards that assume 18 decimals to misprice balances by a factor of one hundred million. |
| CERE 007 | **INFO** | Token contract is clean, capped, ownerless, non upgradeable, non pausable, and fee free, and the base chain and 2026 GitHub are live and active (positive). |

---

## 9. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, hard capped, ownerless, no transfer fee |
| Supply / minting | Low risk | No mint function in bytecode, cap equals total supply, inflation impossible |
| Product reality | High risk | DDC still beta and testnet while marketed as production and adoption ready |
| Traction | High risk | Near dead market, thin liquidity, no verifiable usage numbers |
| Transparency | High risk | Perpetual coming soon tooling, unverifiable partnerships, repeated pivots |

---

## 10. Technical Specifications

| Item | Value |
|------|-------|
| Contract | `0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6` |
| Decimals | 10 (nonstandard) |
| Total supply | 10,000,000,000 CERE (hard cap, cap equals supply) |
| Upgradeable | No (EIP 1967 slots zero, not a proxy) |
| Privileged roles | None (owner and AccessControl getters revert) |
| Mint / pause | None (mint selector absent, `paused()` reverts) |
| Transfer fee | None |
| Burn | ERC20Burnable, holders burn own tokens |
| Native chain | Cere Substrate Layer 1 (CERE as gas and staking) |

---

## 11. Conclusion

Cere Network is the mirror image of a typical flagged project. Its Ethereum token contract is nearly ideal, a fixed, ownerless, non mintable, non upgradeable, non pausable ERC 20, so its contract level risk is genuinely low. Yet the project scores 46 out of 100 and is Flagged, because the story it sells runs well ahead of what is live and used. The Decentralized Data Cloud is marketed as an enterprise grade, adoption ready, AI era production cloud, but in reality it is still largely a beta and testnet, its public statistics dashboard and open node participation remain coming soon, its usage numbers are unverifiable, and its market is close to dead, with a capitalization around 1.35 million dollars and roughly a thousand dollars of daily volume against a price down about 99.96 percent from its peak. The base chain is real and the GitHub is active, which is why this is not a zero, but a clean token cannot rescue a near dead project whose flagship remains unproven.

---

## 12. Recommendations

**For the Cere team:**
- Ship the public DDC statistics dashboard and open node participation, or stop describing the network as an adoption ready enterprise cloud until they are live.
- Label the DDC honestly as beta and testnet where that is true, rather than presenting it as a production system on the homepage.
- Publish verifiable usage numbers for nodes, clusters, and data stored, and mark partnerships as intent versus shipped and in use.
- Reduce strategic churn; repeated pivots from enterprise cloud to consumer brand to AI and DePIN erode trust in the roadmap.

**For users:**
- Recognize that the clean, ownerless token does not make the underlying project healthy; the data cloud was still beta and testnet at review time.
- Understand that the market is near dead, with roughly 1.35 million dollars capitalization and about 1,170 dollars of daily volume, which means very thin liquidity and high price impact.
- Note the nonstandard 10 decimal precision and never assume 18 decimals when integrating or valuing balances.
- Verify any claimed node count, storage figure, or partnership independently before relying on it.

---

## 13. Verification

- MEFAI onchain analysis: a direct Ethereum read of the CERE contract (identity, 10 decimals, total supply of 10,000,000,000 with `cap()` equal to supply, reverting owner and AccessControl getters, absence of the mint selector `0x40c10f19`, zero EIP 1967 slots confirming no proxy, reverting `paused()`, no transfer fee, and the ERC20Burnable burn selector `0x42966c68`).
- Project and product checks: the live Cere Substrate mainnet (governance, staking, runtime upgrade, LayerZero to Base), the actively maintained Cerebellum Network GitHub in 2026, and the DDC status (Dragon 1 beta cluster and SDK playground, with the public statistics dashboard and open node participation still unreleased).
- Market data: market capitalization around 1.35 million dollars, 24 hour trading volume near 1,170 dollars, and a price roughly 99.96 percent below the November 2021 all time high.
- Project statements: the cere.network website and marketing (Decentralized Data and Finance Cloud for Enterprises, Designed for the AI Data Era, Converging Web3 x AI, Enabling True Data Decentralization, and Adoption Ready positioning).
