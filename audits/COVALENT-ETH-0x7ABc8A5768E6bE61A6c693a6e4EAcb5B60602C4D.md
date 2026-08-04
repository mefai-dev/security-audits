# Security Audit Report: Covalent (CXT) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Covalent |
| **Token Symbol** | CXT (Covalent X Token, migrated 1 to 1 from CQT) |
| **Contract (Ethereum)** | `0x7ABc8A5768E6bE61A6c693a6e4EAcb5B60602C4D` |
| **Chain** | Ethereum ERC 20 (also a bridged deployment on Base) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **65/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Passed** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Covalent is one of the more genuine data infrastructure projects MEFAI reviews. It runs a real, paid, revenue generating blockchain data business behind the GoldRush API, and its migrated token, CXT, has consumptive utility that is actually exercised by usage rather than merely promised. The MEDIUM rating reflects a gap between the exchange facing "no inflation" story and what the verified contract actually allows, plus the usual retained multisig control, not a question of legitimacy.

1. **The data product is real and used.** GoldRush, formerly the Unified API, serves processed data across 100 plus blockchains. Covalent reports on the order of 17 billion cumulative API calls, roughly 471 million in a single recent quarter, over 95 percent of them paid and revenue generating, and more than 3,000 organizations served. This is a working business, not vaporware.
2. **The token utility is genuine, if partly forward looking.** CXT is used for operator staking with slashing and for query settlement, where usage denominated in a stablecoin is converted into a market buy of CXT that pays network operators, with settlement recorded on Moonbeam. Over 5 million CXT has been bought back, fueled by real API usage.
3. **The contract contradicts the "no inflation" marketing.** The fixed one billion supply and the honest 1 to 1 CQT to CXT migration both check out onchain, but the verified contract contains a multisig controllable, rate limited emission function with no hard cap. It is dormant only because the emission role currently sits at the zero address, a condition the admin multisig can change at any time.
4. **Control is retained, not renounced.** A 3 of 5 Gnosis Safe holds the admin, cap manager and permit revoker roles. This is disclosed and is a caution rather than a red flag on its own.

The contract is clean and the business is real, which keeps this out of scam territory. The gap between the "no inflationary mechanics" framing and a contract that can be switched to inflate, together with retained multisig control, is what holds it in the middle band. This lands Covalent at 65 out of 100, Passed, at MEDIUM overall risk.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Covalent X Token / CXT |
| **Contract (Ethereum)** | `0x7ABc8A5768E6bE61A6c693a6e4EAcb5B60602C4D` |
| **Decimals** | 18 |
| **Total supply** | 1,000,000,000 CXT (fixed today; no hard cap encoded in the contract) |
| **Circulating (verified)** | ~967.1 million CXT (~96.7 percent) |
| **History** | Migrated 1 to 1 from CQT (`0xD417144312DbF50465b1C641d016962017Ef6240`); constructor minted exactly one billion to the migrator with no expansion |
| **Contract controls** | No Ownable. AccessControlEnumerable with DEFAULT_ADMIN_ROLE, CAP_MANAGER_ROLE and PERMIT2_REVOKER_ROLE held behind a 3 of 5 Gnosis Safe; not renounced; non upgradeable |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the CXT contract on Ethereum returned:

| Check | Result |
|-------|--------|
| Token identity | Covalent X Token, CXT, 18 decimals, Sourcify exact match verified |
| Total supply | 1,000,000,000 CXT, matches the marketed one billion; ~96.7 percent circulating |
| Mint authority | Present as EMISSION_ROLE, rate limited by mintPerSecondCap (set by CAP_MANAGER_ROLE); currently held by the zero address, so no live minter today (dormant); no hard cap |
| Admin authority | DEFAULT_ADMIN_ROLE, CAP_MANAGER_ROLE and PERMIT2_REVOKER_ROLE held by a 3 of 5 Gnosis Safe (`0x381225fa2dffa29c01b52214656077f8550f819e`), not renounced |
| Upgradeable | No, the contract is not a proxy (Solidity 0.8.21, direct implementation) |
| Pausable | No |
| Transfer fee | None (OpenZeppelin ERC20 with Permit and Permit2) |

**Interpretation.** At the contract level CXT is competent and transparent: a fixed supply today, non upgradeable, non pausable ERC20 with no transfer fee, verified by a Sourcify exact source match, with Permit and Permit2 support. The residual contract risks are twofold. First, control is centralized, since a single 3 of 5 multisig holds the admin, cap and permit revoker roles and can grant the emission role at any time. Second, the token is inflation capable by design: the mint path exists and is rate limited but has no hard cap, and it is switched off today only because the emission role sits at the zero address, not because the code forbids it. Both are disclosed conditions and cautions, not exploits. The project level story below is where the real assessment sits.

---

## 3. Claim vs Reality: "Verified Data Powering AI and the Onchain Economy"

> Site: Covalent positions GoldRush, formerly the Unified API, as the source of unified, processed blockchain data across many chains, powering AI, DeFi, GameFi and applications with onchain data from over 100 blockchains.

**Reality: this claim holds.** The GoldRush data business is genuinely real and used. Covalent reports on the order of 17 billion cumulative API calls, roughly 471 million in a single recent quarter, coverage of 100 plus blockchains, more than 95 percent of calls paid and revenue generating, and more than 3,000 organizations served, with named consumers such as OpenSea and Fidelity. The frontend is a clean, read only marketing and data site whose live figures come from the official Covalent API endpoints rather than being fabricated, with no wallet connection and no signing surface. Unlike many projects that lead with an AI narrative, Covalent's data infrastructure is a working, revenue generating product. This is a credited positive.

---

## 4. Claim vs Reality: "No Inflationary Mechanics" and Fixed One Billion Supply

> Site and exchange listings: CXT is presented as a fixed one billion supply token with no inflationary or deflationary mechanics.

**Reality: true in practice today, but not guaranteed by the code.** The fixed one billion total and the honest 1 to 1 migration from CQT both verify onchain, and there is no live minter at present. The contradiction is that the verified contract does contain a multisig controllable, rate limited emission function, the mint path gated by EMISSION_ROLE, with no hard cap. The token is therefore inflation capable by design, and the only reason no new tokens can be created right now is that the emission role currently sits at the zero address, a condition the 3 of 5 admin multisig can change whenever it chooses. Marketing that states there are "no inflationary mechanics" understates a real, dormant capability, and a security minded reader should treat the fixed supply as a present state rather than a code enforced guarantee.

---

## 5. Claim vs Reality: CXT Utility, Staking and Query Settlement

> Site and docs: CXT is described as the network token used for operator staking and governance, and as the settlement asset behind API queries, with API usage creating buy pressure for CXT.

**Reality: genuine consumptive utility, partly forward looking, settled off Ethereum.** The documented model is real and partly demonstrated. Network operators must meet a minimum staking requirement, earn CXT for honestly indexing data and answering queries, and can be slashed for misbehavior, while holders can delegate. On the payment side, developers deposit a US denominated stablecoin such as USDC, and when a query is answered the network performs a market buy of CXT with that stablecoin and settles it to operator and validator wallets, with consumption recorded on a Moonbeam ledger. The consumptive loop is not merely theoretical: with over 95 percent of API calls paid and a reported 5 million plus CXT already bought back, usage is genuinely translating into demand for the token. The fair cautions are that settlement occurs on Moonbeam rather than on the Ethereum contract audited here, that the stablecoin to CXT conversion is the actual payment mechanism (CXT is a settlement unit, not a direct payment token), and that the Ethereum emission path that would fund operator rewards over time is dormant. On balance the token utility is real and among the stronger examples in this category, which is credited.

---

## 6. Claim vs Reality: CQT to CXT Migration and Frontend Integrity

> Site: CXT is the successor network token, replacing CQT at a 1 to 1 ratio following a governance vote.

**Reality: an honest migration and a clean frontend.** The migration checks out onchain: the constructor minted exactly one billion CXT to the migrator with no expansion, replacing the legacy CQT contract at `0xD417144312DbF50465b1C641d016962017Ef6240` at a 1 to 1 ratio, and CXT is also present as a bridged deployment on Base. The live site's code references only the correct official CXT contract and nothing else, with no trace anywhere of the deprecated CQT contract or any lookalike, so there is nothing steering visitors toward the old token, and the audit reference on the site is a genuine link to Covalent's own documentation rather than an unbacked badge. The one minor identity note is that the site's canonical social handle in metadata is listed as a non standard account rather than the expected official Covalent account. This does not put user funds at risk, but it is worth a second look.

---

## 7. Positive Findings (Credited)

- GoldRush is a genuine, paid, revenue generating data business: on the order of 17 billion cumulative API calls, roughly 471 million in a recent quarter, 100 plus chains, over 95 percent paid, and more than 3,000 organizations served.
- CXT has real consumptive utility: operator staking with slashing, stablecoin to CXT query settlement that pays operators, and over 5 million CXT bought back, fueled by actual usage.
- The token contract is clean: a fixed supply today, non upgradeable, non pausable ERC20 with no transfer fee, verified by a Sourcify exact match, with Permit and Permit2 support.
- The CQT to CXT migration is honest (1 to 1, no expansion at deployment), and the frontend references only the correct contract with no lookalike or old token steering.
- Privileged roles sit behind a 3 of 5 Gnosis Safe rather than a single externally owned key, and the retained control is disclosed.

---

## 8. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| CXT 001 | **HIGH** | Inflation capable by design against a "no inflation" claim: the verified contract has a rate limited mint path (EMISSION_ROLE) with no hard cap, dormant only because the role is at the zero address; the 3 of 5 admin multisig can enable it at any time. |
| CXT 002 | **MEDIUM** | Contract control not renounced: a 3 of 5 Gnosis Safe holds the admin, cap manager and permit revoker roles, and can grant the emission role and set the mint rate cap. |
| CXT 003 | **LOW** | The site's canonical social handle in metadata is a non standard account rather than the expected official Covalent account (identity note; no fund risk). |
| CXT 004 | **INFO** | GoldRush data business is real and used, and CXT has genuine consumptive utility via staking and query settlement (positive). |
| CXT 005 | **INFO** | Clean, fixed supply, non upgradeable, non pausable, no fee ERC20, Sourcify verified, with an honest 1 to 1 migration and a clean read only frontend (positive). |

---

## 9. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, fixed supply today, no transfer fee, honest 1 to 1 migration |
| Supply / minting | Medium risk | Dormant but real multisig controllable emission with no hard cap |
| Product reality | Low risk | GoldRush API is genuinely live, paid and widely used |
| Token utility / traction | Low to medium risk | Real consumptive utility and buybacks, but settlement on Moonbeam and the Ethereum emission path dormant |
| Transparency | Medium risk | "No inflationary mechanics" understates the emission capability; non standard social handle in metadata |

---

## 10. Technical Specifications

| Item | Value |
|------|-------|
| Contract | `0x7ABc8A5768E6bE61A6c693a6e4EAcb5B60602C4D` |
| Decimals | 18 |
| Total supply | 1,000,000,000 CXT (fixed today; no hard cap in code) |
| Circulating | ~967.1 million CXT (~96.7 percent) |
| Upgradeable | No (not a proxy; Solidity 0.8.21) |
| Pausable | No |
| Privileged roles | Admin, cap manager, permit2 revoker behind a 3 of 5 Gnosis Safe; EMISSION_ROLE at the zero address (dormant) |
| Transfer fee | None |
| Standard | OpenZeppelin ERC20 with Permit and Permit2; Sourcify exact match |
| Migration | 1 to 1 from CQT (`0xD417144312DbF50465b1C641d016962017Ef6240`); also bridged to Base (`0xb1e1f3cc...628ff`) |

---

## 11. Conclusion

Covalent is a genuine data infrastructure business with a clean, verified token, which keeps its contract level risk low and earns a Passed verdict at 65 out of 100, MEDIUM overall risk. The GoldRush API is real, paid and widely used across 100 plus chains, and CXT has authentic consumptive utility through operator staking and stablecoin to CXT query settlement, with millions of tokens already bought back by real usage rather than promised by a roadmap. What holds the score in the middle band is not a scam or a broken product but two disclosed cautions: the contract can be switched from its current fixed supply state into a rate limited, uncapped inflation, which contradicts the exchange facing "no inflationary mechanics" framing, and privileged control still rests with a 3 of 5 multisig that has not renounced. The caution here is transparency and retained control around an otherwise legitimate and working data project.

---

## 12. Recommendations

**For the Covalent team:**
- Reconcile the "no inflationary mechanics" marketing with the contract by either encoding a hard cap or clearly disclosing that a rate limited emission exists and is dormant only because the emission role is unset.
- Publish the multisig signer set and any timelock or governance process that gates granting the emission role and changing the mint rate cap.
- Correct the non standard canonical social handle in the site metadata to the official Covalent account.
- Continue publishing verifiable usage and buyback figures so the consumptive token loop remains independently checkable.

**For users:**
- Treat the fixed one billion supply as a present state, not a code enforced guarantee; a 3 of 5 multisig can enable a rate limited, uncapped emission.
- Value the token on the real, paid GoldRush usage and the demonstrated buyback loop, while noting that query settlement occurs on Moonbeam rather than on the Ethereum contract.
- Verify the official social and documentation channels independently, given the non standard handle in the site metadata.

---

## 13. Verification

- MEFAI onchain analysis: a direct Ethereum read of the CXT contract (identity, 18 decimals, one billion total supply matching the marketed cap, ~96.7 percent circulating, dormant EMISSION_ROLE at the zero address with a rate limited mint path and no hard cap, admin, cap manager and permit revoker roles held by the 3 of 5 Gnosis Safe `0x381225fa2dffa29c01b52214656077f8550f819e`, non upgradeable, non pausable, no transfer fee), confirmed by a Sourcify exact source match and cross checked against the legacy CQT contract `0xD417144312DbF50465b1C641d016962017Ef6240`.
- Product and usage checks: the GoldRush API positioning and traction figures (cumulative and quarterly API calls, 100 plus chains, paid share, organizations served, buyback total) and the CXT utility model (operator staking and slashing, stablecoin to CXT query settlement on Moonbeam) from Covalent's own documentation and reporting, cross referenced with exchange listings.
- Frontend review: a live read of the site confirming a read only data product referencing only the correct CXT contract, no wallet or signing surface, live figures sourced from official Covalent API endpoints, and a non standard canonical social handle in the metadata.
- Project statements: the project's website, documentation and exchange listings (GoldRush positioning, the CQT to CXT migration record, the fixed supply and "no inflationary mechanics" framing, and the staking and settlement descriptions).
