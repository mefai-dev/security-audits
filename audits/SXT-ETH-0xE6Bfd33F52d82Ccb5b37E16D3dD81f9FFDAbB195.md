# Security Audit Report: Space and Time (SXT) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Space and Time |
| **Token Symbol** | SXT |
| **Contract (Ethereum)** | `0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195` |
| **Chain** | Ethereum ERC 20 (canonical), with a mirror on Base at `0xa2c22252cdc8b7cddee1b0b2e242818509fcf7b8`; SXT also secures and pays gas on the SXT Chain |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **74/100** |
| **Overall Risk** | **LOW** |
| **Verdict** | **Passed** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Space and Time markets itself as a verifiable data blockchain for apps and AI, and unlike many projects with an AI narrative, most of what it sells is actually shipped and inspectable. The token contract is a clean, fixed supply ERC 20, the flagship technology is real and open source, and the token has genuine designed utility on a live chain. The rating is held below the top band by real control and maturity considerations, not by any legitimacy gap.

1. **The flagship product is real and usable.** Proof of SQL, the zero knowledge prover that lets a smart contract or an application verify a SQL query ran correctly over committed data, is open source in Rust under the DOSL 1.0 license, actively developed with thousands of commits and published benchmarks, and SXT Chain is live on mainnet. This is a working product, not a promise.
2. **The token has genuine, designed utility.** SXT is staked by validators and delegators to secure the chain, and queries and data inserts are paid in SXT denominated gas, with fees routed to validators and provers. Utility is built into the protocol rather than bolted onto marketing.
3. **Supply is genuinely fixed on chain.** A direct read confirms exactly five billion tokens, eighteen decimals, no mint function, a non proxy deployment, and no transfer fee, and the blog address, the RPC identity and the aggregator listing agree across three independent confirmations.
4. **The cautions are control and maturity, not legitimacy.** An active pauser and admin role can halt transfers and are not renounced, a large team and investor unlock schedule overhangs supply across four years, the token and chain are young (mainnet and token launched in May 2025) so traction is real but early, and the site names four audit firms without linking their reports.

The contract is clean and the product is real. This lands Space and Time at 74 out of 100, LOW risk, Passed.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 1 |
| Low | 3 |
| Informational | 2 |
| **Total** | **6** |

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Space and Time / SXT |
| **Contract (Ethereum)** | `0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195` |
| **Decimals** | 18 |
| **Total and max supply** | 5,000,000,000 SXT, minted in full at genesis; no mint function, so supply is fixed |
| **Circulating** | Roughly 2.6 billion SXT, about 52 percent of max (approximately 28 percent entered circulation at mainnet launch) |
| **Allocation** | Community Rewards 28.0 percent, Investors 25.9 percent, Ecosystem 23.7 percent, Team 22.4 percent; team and investor tranches on a four year linear vest with a 15 percent cliff at month twelve |
| **Contract controls** | No Ownable; role based AccessControl with DEFAULT_ADMIN_ROLE and PAUSER_ROLE present and not renounced; non upgradeable; standard OpenZeppelin ERC 20 with Permit and Votes extensions |

---

## 2. Onchain Security Assessment (MEFAI analysis)

MEFAI's direct read of the SXT contract on Ethereum returned:

| Check | Result |
|-------|--------|
| Token identity | Space and Time, SXT, 18 decimals, verified |
| Total supply | 5,000,000,000, minted at genesis, matches the published tokenomics |
| Mint authority | **None**, there is no mint selector, so supply cannot be inflated |
| Pause authority | Present, held via PAUSER_ROLE; paused currently returns false |
| Admin authority | DEFAULT_ADMIN_ROLE present and not renounced |
| Upgradeable | No, the EIP 1967 proxy slots are empty and the implementation is direct |
| Transfer fee | None |
| Extensions | ERC 20 Permit (EIP 2612) for gasless approvals and ERC20Votes for governance delegation |

**Interpretation.** At the contract level SXT is strong. It is a fixed supply, non upgradeable, fee free ERC 20 with no mint function, so circulating supply cannot be inflated and transfers are untaxed. The residual contract risk is control centralization rather than dilution, because the token is Pausable and a privileged role holder could halt transfers, and the admin and pauser roles are active rather than renounced. The AccessControl mapping is non enumerable, so the exact role holder addresses are not listable over RPC. This is a caution to disclose, not a red flag on its own, and the more meaningful part of the story is at the product level below, which for this project is genuinely positive.

---

## 3. Claim vs Reality: "Verifiable database secured by Proof of SQL and zero knowledge proofs"

> Site: Space and Time is positioned as a decentralized replacement for a blockchain indexing service, database and API layer, with a verifiable onchain database that secures offchain data using zero knowledge proofs and lets smart contracts execute ZK proven SQL.

**Reality: the technology is real, open source and usable.** The core innovation, Proof of SQL, is a high performance zero knowledge prover that cryptographically attests a SQL query was computed correctly against a tamper evident, committed data state. It is published openly on GitHub at `spaceandtimefdn/sxt-proof-of-sql`, written in Rust under the Decentralized Open Software License 1.0, with thousands of commits, several thousand stars and hundreds of forks, indicating sustained active development rather than a one time drop. The team has published reproducible benchmarks (on the order of one million rows proven in roughly one second on GPU accelerated hardware), and the prover is downloadable and runnable by anyone. SXT Chain is live on mainnet, and the Chain App at chain.spaceandtime.io and the Data Studio are the user facing interfaces. This is one of the stronger claim to reality matches in the sector: the flagship is a working, inspectable product, not aspirational branding.

---

## 4. Claim vs Reality: "Live on mainnet, and SXT powers queries and staking"

> Site: Space and Time is described as live on mainnet and ready for production, with SXT presented as the token you stake to secure the network and earn rewards.

**Reality: genuine designed utility, on a young network still building traction.** The token utility is real and built into the protocol. Validators must stake SXT to join consensus, witness index commitments and verify Proof of SQL results, with slashing for signing invalid commitments or going offline, and holders can delegate SXT to validators to share rewards. Queries and data inserts are paid in SXT denominated gas, and those fees are distributed to validators and provers, so SXT is the settlement asset of its own product rather than a decoupled ticker. The honest qualifier is maturity. Mainnet and the token generation event took place in May 2025, so at review the network is young: the validator set and staking participation were still bootstrapping, base staking rewards were described as ramping toward roughly eight percent while an early Genesis Validator Rewards program offered elevated incentives to attract operators. Adoption is real but early, and the token price is correspondingly speculative. The utility exists today; the scale of paid usage is still growing into the design.

---

## 5. Claim vs Reality: "Backed by Chainlink, Microsoft and NVIDIA and integrated across chains"

> Site: Space and Time displays a roster of partners and integrations, including Chainlink, Microsoft, NVIDIA, Circle, Avalanche, Stellar, zkSync and US Bank, as evidence of traction.

**Reality: the headline relationships are substantiated, a few are ecosystem breadth.** The most load bearing claims hold up. The Space and Time verifier runs natively on Chainlink nodes so that proof validity can be brought to consensus, which is a concrete, technical integration rather than a logo. Microsoft is a genuine backer and the project raised a 20 million Series A, NVIDIA is a real hardware and performance partner behind the GPU accelerated prover, and there is a working Google BigQuery integration. Some of the wider names read more as ecosystem membership or exploration than deep shipped integrations, which is normal for a young network. On balance the partnership story is better supported than the sector average and is not the kind of intent framed as delivery that would warrant a flag.

---

## 6. Claim vs Reality: "Audited by Hashlock, Spearbit, Pashov and Cantina" and fixed supply controls

> Site: an Audited by section names Hashlock, Spearbit, Pashov and Cantina, and the tokenomics promise a fixed five billion supply.

**Reality: the auditors are real firms but the reports are not one click verifiable, and the fixed supply checks out with one nuance.** Hashlock, Spearbit, Pashov and Cantina are all established security firms, so the names are credible, but on the homepage the logos are display images only and are not linked to the underlying reports, so a visitor cannot verify the audits directly from the page. Separately, MEFAI's frontend review found the homepage clean: it is a static marketing page with no wallet connection, no signature prompts and no token address embedded in the HTML or JavaScript, so there is no lookalike or address swap risk and nothing resembling a drainer, and every loaded script is free of eval, obfuscation or web3 approval and transfer calls. The only mild hygiene note is that two production helper scripts are served from third party sandbox and app hosts rather than first party hosting. On supply, the fixed five billion claim is confirmed precisely on chain with no mint function, and the blog address matches both the RPC identity and the aggregator listing across three independent confirmations. The one thing the marketing underplays is that transfers can be paused and the admin roles remain in place, so the token is not immutable or fully decentralized in its controls, and a large team and investor tranche is still vesting.

---

## 7. Positive Findings (Credited)

- The flagship, Proof of SQL, is a real, open source, actively developed zero knowledge prover with published benchmarks, and SXT Chain is live on mainnet.
- The token contract is a clean, fixed supply, non upgradeable ERC 20 with no mint function and no transfer fee, verified across three independent address confirmations.
- SXT has genuine protocol level utility as the staking asset for validators and delegators and as the gas paid for queries and data inserts.
- The named headline relationships, especially the native Chainlink integration and Microsoft backing, are substantiated rather than decorative.
- The public homepage is clean, with no wallet hooks, no embedded address and no drainer behavior.

---

## 8. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| SXT 001 | **MEDIUM** | Active control roles: DEFAULT_ADMIN_ROLE and PAUSER_ROLE are present and not renounced, so a role holder can pause transfers and manage roles; the token is currently unpaused and the role holders are not enumerable on chain. |
| SXT 002 | **LOW** | Supply overhang: team (22.4 percent) and investor (25.9 percent) allocations vest over four years with recurring 2026 unlocks, against roughly 52 percent circulating. |
| SXT 003 | **LOW** | Early maturity: mainnet and the token launched in May 2025, the validator set and staking participation were still bootstrapping at review, so traction is real but early and the token is speculative. |
| SXT 004 | **LOW** | Transparency and hygiene: the Audited by logos (Hashlock, Spearbit, Pashov, Cantina) are not linked to reports, and two production helper scripts are served from third party sandbox and app hosts. |
| SXT 005 | **INFO** | Fixed five billion supply with no mint function on a non proxy, fee free contract (a strong positive). |
| SXT 006 | **INFO** | Real, open source flagship (Proof of SQL) with genuine SXT staking and query fee utility, a native Chainlink integration and Microsoft backing (positive). |

---

## 9. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, fixed five billion supply, no mint, no transfer fee |
| Supply / minting | Low risk | No mint function; supply fixed at genesis (vesting overhang is a market factor, not inflation) |
| Control / centralization | Medium risk | Active, unrenounced admin and pauser roles can halt transfers and manage roles |
| Product reality | Low risk | Open source, benchmarked prover; SXT Chain live on mainnet |
| Traction / maturity | Medium risk | Young token and chain, validator set and staking still bootstrapping, speculative |
| Transparency | Low to medium risk | Strong onchain verifiability; audit logos not linked to reports |

---

## 10. Technical Specifications

| Item | Value |
|------|-------|
| Contract (Ethereum) | `0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195` |
| Mirror (Base) | `0xa2c22252cdc8b7cddee1b0b2e242818509fcf7b8` |
| Decimals | 18 |
| Total and max supply | 5,000,000,000 SXT (fixed, no mint function) |
| Circulating | Roughly 2.6 billion (about 52 percent) |
| Upgradeable | No (non proxy, EIP 1967 slots empty) |
| Privileged roles | DEFAULT_ADMIN_ROLE and PAUSER_ROLE, active and not renounced |
| Transfer fee | None |
| Extensions | ERC 20 Permit (EIP 2612) and ERC20Votes |
| Native chain | SXT Chain (SXT as staking and gas) |

---

## 11. Conclusion

Space and Time is one of the more genuine verifiable data and AI projects, which places it at 74 out of 100, LOW risk, Passed. Its flagship Proof of SQL prover is real, open source, benchmarked and actively developed, SXT Chain is live on mainnet, and the token has authentic protocol level utility as the staking asset and the gas paid for queries, with the headline Chainlink and Microsoft relationships substantiated. The token contract reinforces this: a fixed five billion supply with no mint function, non upgradeable and fee free, confirmed across three independent sources. The cautions are control and maturity rather than legitimacy, namely active and unrenounced admin and pauser roles that can halt transfers, a sizable team and investor unlock schedule that overhangs supply over four years, and a young network whose traction is real but early. This is a project whose product is shipped and inspectable, with the residual risk sitting in centralized controls and early stage adoption.

---

## 12. Recommendations

**For the Space and Time team:**
- Move the admin and pauser roles toward a timelock or a documented multisig, publish the role holder addresses, and set out a path to reduce or renounce pause control as the chain matures.
- Link the Audited by logos to the actual Hashlock, Spearbit, Pashov and Cantina reports so visitors can verify audits in one click.
- Continue publishing live validator, staking and query volume metrics so the growing traction is transparent, and serve production helper scripts from first party hosting.

**For users:**
- Treat the technology and token utility as real and live, while recognizing the network is young and adoption is still ramping, so the token is speculative.
- Understand that supply is genuinely fixed with no mint, but a privileged role can pause transfers, and large team and investor tranches unlock over four years.
- Verify the token address independently (the official blog, the RPC identity and the aggregator listing all agree) before transacting.

---

## 13. Verification

- MEFAI onchain analysis: a direct Ethereum read of the SXT contract (identity, 18 decimals, fixed five billion total supply with no mint selector, non proxy deployment, no transfer fee, and active DEFAULT_ADMIN_ROLE and PAUSER_ROLE with paused false), cross checked against the published tokenomics.
- Product checks: the open source Proof of SQL repository at `spaceandtimefdn/sxt-proof-of-sql` (Rust, DOSL 1.0, active development and published benchmarks), confirmation that SXT Chain is live on mainnet with SXT staking and SXT denominated query gas, and the native Chainlink integration.
- Frontend review: a live review of the spaceandtime.io homepage (static marketing page, no wallet connection, no embedded token address, no drainer behavior, audit logos not linked to reports).
- Project statements: the project's website and blog (verifiable database and Proof of SQL positioning, live on mainnet and ready for production claims, the SXT staking and query utility, the partner roster, and the fixed five billion supply and vesting outline).
- Sources: `spaceandtime.io/blog introducing the SXT token`; `docs.spaceandtime.io stake sxt and delegated staking`; `github.com/spaceandtimefdn/sxt-proof-of-sql`; `etherscan 0xe6bfd3...`; `coingecko space-and-time`; `ethereum-rpc.publicnode.com`; `l2beat sxt`.

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
