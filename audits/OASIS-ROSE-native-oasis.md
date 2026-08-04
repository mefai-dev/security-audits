# Security Audit Report: Oasis Network (ROSE), native token

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Oasis Network |
| **Token Symbol** | ROSE (native consensus token) |
| **Canonical asset** | native ROSE on the Oasis Layer 1 (not an ERC 20) |
| **Wrapped forms** | Sapphire `0x8Bc2B030b299964eEfb5e1e0b36991352E56D2D3` (Wrapped ROSE), Ethereum Wormhole `0x26B80FBfC01b71495f477d5237071242e0d959d7` |
| **Chain** | Oasis Network Layer 1, with the Sapphire confidential EVM and Emerald EVM ParaTimes |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **67/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Passed** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Oasis Network markets itself as infrastructure for confidential compute and responsible AI, and this audit finds that the substance largely matches the story. Unlike projects whose flagship products are unreachable, Oasis is a real, shipping privacy Layer 1 with clean token mechanics. The cautions here are about hardware trust, historical concentration, and moderate traction, not about a broken product or an unsafe contract.

1. **The privacy Layer 1 is real and shipping.** Sapphire is a production confidential EVM that keeps smart contract state and calldata encrypted inside secure enclaves while staying Ethereum compatible, and the ROFL mainnet launched in July 2025 to extend verifiable offchain compute to workloads such as AI agents and inference on sensitive data. These are live products, not roadmap promises.
2. **The token mechanics are sound.** ROSE is a native consensus token with a fixed maximum supply of ten billion, no owner key, no arbitrary mint, no pause, and no fee on transfer. New supply enters only through capped staking rewards paid to validators and delegators.
3. **The privacy guarantee rests on Intel TEE hardware trust.** Confidentiality on Sapphire and ROFL is enforced by Intel SGX and TDX trusted execution environments rather than by cryptography alone, which introduces a hardware trust and side channel assumption. Oasis addresses this openly with a public TEE Break Challenge bounty, but the reader should understand the assumption.
4. **Traction is moderate and history carries concentration.** Market capitalization sits near 46 million dollars, Sapphire DeFi value locked is modest, and usage is real but not large. The original distribution allocated roughly 43 percent to backers and core contributors on a backloaded ten year schedule, most of which is now behind the market with circulating supply near 78 percent.

The contract level risk is low and the product is genuinely delivered, so Oasis lands at 67 out of 100, Passed, with the honest reservation that its confidentiality depends on enclave security and its economic activity is moderate rather than large.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Oasis Network / ROSE |
| **Canonical asset** | native ROSE, a consensus layer token, not an ERC 20 |
| **Decimals** | 9 (native consensus), 18 on the EVM wrapped forms |
| **Max supply** | 10,000,000,000 ROSE (fixed hard cap, no increase) |
| **Circulating** | roughly 78 percent of the cap |
| **Wrapped Sapphire** | `0x8Bc2B030b299964eEfb5e1e0b36991352E56D2D3`, Wrapped ROSE, 18 decimals, supply near 14.42 million |
| **Wrapped Ethereum (Wormhole)** | `0x26B80FBfC01b71495f477d5237071242e0d959d7`, supply near 828 thousand, thinly used |
| **Contract controls** | none on native ROSE (no owner, no mint, no pause); wrappers are minimal WETH style, non upgradeable |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the ROSE asset and its wrapped forms returned:

| Check | Result |
|-------|--------|
| Token identity | native ROSE consensus token, plus Wrapped ROSE on Sapphire and a Wormhole representation on Ethereum, all verified |
| Max supply | 10,000,000,000, fixed, no increase path |
| Mint authority | none arbitrary; new supply is only bounded staking rewards capped near 2.3 billion (about 23 percent), governance set, 2 to 20 percent APR |
| Owner / admin | native ROSE has no owner key; the network runs on onchain governance with about 120 validators, stewarded by the Oasis Protocol Foundation |
| Upgradeable | consensus and ParaTimes evolve through governance and validator voted hard forks; the Sapphire wROSE wrapper is non upgradeable |
| Pause / freeze | none on the token; slashing exists only at the validator level |
| Transfer fee | none |

**Interpretation.** At the asset level ROSE is sound: a fixed supply native token with no owner, no arbitrary mint, no pause, and no transfer fee, whose only emission is a capped staking reward mechanism controlled by consensus parameters and governance. The Sapphire Wrapped ROSE at `0x8Bc2B030b299964eEfb5e1e0b36991352E56D2D3` verified over RPC as name Wrapped ROSE, symbol wROSE, eighteen decimals, and total supply near 14.42 million, with a minimal deposit and withdraw wrapper built on ERC20 and ERC20Burnable, no admin, no mint authority, no upgrade path, and no transfer fee. The residual risks are not contract exploits. They are the hardware trust that underpins privacy, the historical insider and unlock concentration, modest liquidity, and third party bridge exposure on the Ethereum wrapped form, each covered below. No single named third party formal audit was found, though the code is open source and Oasis runs a public TEE bounty.

---

## 3. Claim vs Reality: "Sapphire is a production confidential EVM"

> Site: Oasis presents onchain privacy through confidential smart contracts that keep state encrypted while remaining EVM compatible and composable, positioning Sapphire as the industry first confidential EVM.

**Reality: the claim holds.** Sapphire is a live confidential EVM ParaTime that executes Solidity contracts inside Intel SGX and TDX enclaves, keeping calldata and contract state encrypted even from the node operator while paying gas in ROSE. It launched to mainnet in 2023 and remains in production, with real dApps deployed and the Oasis Privacy Layer letting existing EVM apps add confidentiality. This is a delivered product rather than a promise, and the confidential EVM positioning is credible. The only qualifier, addressed in section 6, is that the confidentiality is enforced by hardware enclaves rather than by cryptography alone.

---

## 4. Claim vs Reality: "Trustless execution and AI agents through ROFL"

> Site: Oasis promotes a confidential compute framework for offchain apps with onchain verification, and highlights trustless AI agents such as trading bots, portfolio managers, and automated strategies.

**Reality: shipping, with the same hardware caveat.** The ROFL mainnet launched in July 2025 and extends the enclave model to verifiable offchain compute, so an application can run heavier logic, including AI training and inference on sensitive data, and produce an onchain attestation that the code ran as claimed. Named builders such as WT3, DeFAI, and others are working on ROFL, and the framework is real infrastructure rather than a slogan. The honest framing is that trustless here means trust rooted in Intel TDX attestation, not trust removed entirely, and the AI agent narrative is early stage traction rather than mass adoption.

---

## 5. Claim vs Reality: "ROSE is the fuel of the network"

> Site: ROSE is presented as the token that powers the Oasis Network for gas and staking across the consensus layer and the ParaTimes.

**Reality: genuine utility, but moderate scale.** ROSE is meaningfully used. It is the gas token on Sapphire and Emerald and the staked asset that secures the proof of stake consensus, delegated across roughly 120 validators with rewards in the 2 to 20 percent APR range. This is real token utility tied to the product, unlike projects whose flagship settles in some other asset. The reservation is scale rather than authenticity. Market capitalization is near 46 million dollars, Sapphire DeFi value locked is modest, and onchain activity is present but not large. ROSE genuinely powers its own network, yet the level of economic activity is moderate and liquidity is thin, so the token does its job on a small stage.

---

## 6. Claim vs Reality: "Privacy you can trust, enforced by hardware"

> Site: Oasis frames its privacy as data that stays encrypted even from server operators, enforced by hardware, with every execution producing a cryptographic proof users can verify.

**Reality: true, conditional on enclave security.** The confidentiality of Sapphire and ROFL rests on Intel SGX and TDX trusted execution environments. That is a real and widely used privacy mechanism, and Oasis layers governance, economic, and cryptographic protections around it so that compromising a single node enclave should not by itself break the network. Even so, the guarantee ultimately depends on the integrity of Intel hardware and its resistance to side channel and microarchitectural attacks, which is a different and stronger trust assumption than privacy proven by cryptography alone. To Oasis's credit, the project discloses this openly and runs a public TEE Break Challenge bounty that invites researchers to attack the enclaves. The claim is fair, provided the reader understands that hardware trust is part of the model.

---

## 7. Positive Findings (Credited)

- The privacy Layer 1 is real and shipping: Sapphire is a production confidential EVM and the ROFL mainnet launched in July 2025 for verifiable offchain compute.
- Native ROSE has clean mechanics: a fixed ten billion hard cap, no owner key, no arbitrary mint, no pause, and no fee on transfer, with new supply only from capped staking rewards.
- The Sapphire Wrapped ROSE wrapper is a minimal, non upgradeable WETH style contract with no admin and no mint authority, verified over RPC.
- The team is transparent about the hardware trust assumption and backs it with a public TEE Break Challenge bounty.

---

## 8. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| ROSE 001 | **MEDIUM** | Privacy depends on Intel SGX and TDX hardware trust; a hardware compromise or side channel attack is the main residual confidentiality risk, mitigated but not eliminated by protocol design and the TEE bounty. |
| ROSE 002 | **MEDIUM** | Historical concentration: roughly 43 percent of supply allocated to backers and core contributors on a backloaded ten year schedule created years of unlock overhang, most of which is now behind the market at about 78 percent circulating. |
| ROSE 003 | **LOW** | Moderate traction and thin liquidity: market capitalization near 46 million dollars, modest Sapphire DeFi value locked, and usage that is real but not large. |
| ROSE 004 | **LOW** | Governance and validator concentration: onchain governance and hard fork upgrades are steered by roughly 120 validators and stewarded by the Oasis Protocol Foundation. |
| ROSE 005 | **LOW** | Wrapped ROSE bridge exposure: the Ethereum Wormhole representation depends on third party bridge custody and mint authority rather than on Oasis itself. |
| ROSE 006 | **INFO** | Sapphire confidential EVM and ROFL mainnet are live and shipping (positive). |
| ROSE 007 | **INFO** | Native ROSE and the Sapphire wROSE wrapper have clean, fixed supply, non upgradeable, fee free mechanics (positive). |

---

## 9. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Fixed ten billion cap, no owner, no arbitrary mint, no transfer fee |
| Supply / minting | Low risk | Emission only through capped staking rewards, governance set |
| Product reality | Low risk | Sapphire confidential EVM and ROFL mainnet are live and in production |
| Privacy model | Medium risk | Confidentiality rests on Intel SGX and TDX hardware trust and side channel resistance |
| Traction | Medium risk | Moderate usage, modest DeFi value locked, thin liquidity near 46 million dollars market cap |
| Transparency | Low to medium risk | Open source and disclosed TEE trust with a public bounty, but no single named third party formal audit found |

---

## 10. Technical Specifications

| Item | Value |
|------|-------|
| Canonical asset | native ROSE consensus token (not an ERC 20) |
| Native decimals | 9 (18 on EVM wrapped forms) |
| Max supply | 10,000,000,000 ROSE (fixed hard cap) |
| Emission | capped staking rewards, near 2.3 billion, 2 to 20 percent APR, governance set |
| Owner / admin | none on native ROSE; onchain governance with about 120 validators and the Oasis Protocol Foundation as steward |
| Upgradeable | consensus and ParaTimes via governance and validator voted hard forks; Sapphire wROSE non upgradeable |
| Pause / freeze | none on the token; slashing only at validator level |
| Transfer fee | none |
| Sapphire wROSE | `0x8Bc2B030b299964eEfb5e1e0b36991352E56D2D3`, 18 decimals, supply near 14.42 million |
| Ethereum Wormhole wROSE | `0x26B80FBfC01b71495f477d5237071242e0d959d7`, supply near 828 thousand |
| Products | Sapphire confidential EVM, Emerald EVM, ROFL offchain compute framework |

---

## 11. Conclusion

Oasis Network is a real, shipping privacy Layer 1, which keeps its contract and product level risk low. Sapphire is a production confidential EVM, the ROFL mainnet is live for verifiable offchain compute, and native ROSE is a clean, fixed supply token with no owner, no arbitrary mint, and no fee, used genuinely for gas and staking. This is a project whose marketing and delivered product line up. The reservations that hold the score to 67 out of 100, Passed, are honest and specific: the confidentiality guarantee depends on Intel TEE hardware trust rather than cryptography alone, the original distribution concentrated roughly 43 percent with insiders on a backloaded schedule, and traction and liquidity are moderate rather than large. None of these is a red flag on its own, and the team discloses the hardware assumption openly, so Oasis reads as a legitimate privacy infrastructure project with a defensible token, carrying medium rather than high risk.

---

## 12. Recommendations

**For the Oasis team:**
- Continue to state the Intel TEE hardware trust assumption plainly alongside the privacy marketing, and keep the TEE Break Challenge bounty visible and funded.
- Commission and publish a named third party formal audit of the ParaTime and wrapper contracts to complement the open source code and bounty.
- Publish clear, current onchain usage metrics so that traction claims can be verified independently rather than inferred.
- Keep documenting remaining token unlocks so the residual overhang stays transparent.

**For users:**
- Understand that privacy on Sapphire and ROFL is enforced by Intel SGX and TDX enclaves, so the guarantee rests on hardware integrity and side channel resistance, not on cryptography alone.
- Recognize that ROSE genuinely powers gas and staking, but that liquidity is thin and the market capitalization is small, so price and exit risk are real.
- Use the canonical native ROSE and the verified Sapphire wROSE rather than thinly used bridge representations, and treat the Ethereum Wormhole wROSE as a bridge asset with third party custody risk.

---

## 13. Verification

- MEFAI onchain analysis: verification of native ROSE mechanics (fixed ten billion cap, no owner, no arbitrary mint, no pause, no transfer fee, emission only through capped staking rewards) and direct RPC reads of the Sapphire Wrapped ROSE at `0x8Bc2B030b299964eEfb5e1e0b36991352E56D2D3` (name Wrapped ROSE, symbol wROSE, eighteen decimals, supply near 14.42 million, minimal deposit and withdraw wrapper, non upgradeable, no admin) and the Ethereum Wormhole representation at `0x26B80FBfC01b71495f477d5237071242e0d959d7` (supply near 828 thousand, bridge held mint authority).
- Product checks: live fetch of the official site, which redirects cleanly from oasisprotocol.org to oasis.net and presents onchain privacy, trustless execution, programmable accounts, and AI agents, plus confirmation that Sapphire is a production confidential EVM and that the ROFL mainnet launched in July 2025.
- Governance and economics: onchain governance stewarded by the Oasis Protocol Foundation with roughly 120 validators, staking rewards in the 2 to 20 percent APR band, an original allocation of about 43 percent to backers and core contributors on a backloaded ten year schedule, and circulating supply near 78 percent.
- Market and traction: market capitalization near 46 million dollars, modest Sapphire DeFi value locked, and a public TEE Break Challenge bounty acknowledging the hardware trust model. No single named third party formal audit was found at review time.
- Frontend integrity: the website contains no wallet or contract addresses, no drainer or approval signatures, and only an optional read only prompt to add the Sapphire network to a wallet, so it reads as safe for end users.
