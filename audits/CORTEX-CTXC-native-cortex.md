# Security Audit Report: Cortex (CTXC), Native Coin of the Cortex Chain

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Cortex |
| **Token Symbol** | CTXC |
| **Canonical asset** | Native coin of the Cortex layer one (PoW) |
| **Legacy contract (Ethereum)** | `0xea11755ae41d889ceec39a63e6ff75a02bc1c00d` (deprecated ERC 20 mirror) |
| **Chain** | Cortex native layer one; legacy Ethereum ERC 20 mirror exists |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **49/100** |
| **Overall Risk** | **HIGH** |
| **Verdict** | **Flagged** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Cortex is one of the earliest attempts to run artificial intelligence inference directly on a blockchain, a genuinely pioneering idea from the 2018 era. The audit confirms the technology is real and the chain still exists, but it also finds a project whose market presence and product momentum have quietly collapsed. The honest reading is that Cortex is now closer to dormant than to thriving.

1. **The token has effectively stopped trading.** CoinGecko reports that CTXC has ceased trading on every exchange it lists, with a market capitalization around 186,000 dollars, a price near 0.0008 dollars, and roughly 50,000 dollars of daily volume. CTXC was delisted by OKX in June 2025, by ONUS in April 2025, and by Bithumb, which cited a failure to address investment warnings and a lack of disclosure transparency. A native coin that no longer trades on any listed venue has thin real utility.

2. **The flagship AI narrative is largely undelivered or unused.** The site presents the Cortex Virtual Machine, the Synapse deterministic inference engine, and the ZkMatrix layer two as live pillars, yet ZkMatrix appears in the project's own roadmap as reaching its main version only in 2026, and MEFAI found no verifiable evidence of meaningful current on chain AI inference usage. The chain is secured by miners pursuing block rewards, not by demonstrable AI inference demand.

3. **The public surface is stale and partly unreachable.** The marketing site at www.cortexlabs.ai carries a 2018 to 2023 copyright, and the official explorer at cerebro.cortexlabs.ai was unreachable across repeated attempts at review time. This is not what a currently thriving AI chain looks like.

4. **The chain and the token contract are nonetheless real and clean.** The Cortex layer one still produces blocks through proof of work, with a network hashrate around 862 Gps and roughly 4,800 blocks per day in mid 2026, and the core client is still maintained on GitHub into 2026. The deprecated Ethereum mirror is a fixed cap, non mintable, non proxy, no fee token, and the homepage carries no wallet integration and no scam vectors.

The contract is not a scam and the project is not deceptive, but as a going concern Cortex reads as effectively dormant: the token no longer trades on listed exchanges, the AI inference story shows no current traction, and the public surface is aging. This lands Cortex at 49 out of 100, Flagged.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Cortex Coin / CTXC |
| **Canonical form** | Native coin of the Cortex layer one; holders of the old Ethereum token were told to swap to the native coin |
| **Legacy contract (Ethereum)** | `0xea11755ae41d889ceec39a63e6ff75a02bc1c00d` (deprecated mirror) |
| **Decimals** | 18 |
| **Max supply** | 299,792,458 CTXC (a deliberate reference to the speed of light in metres per second), hard capped and fully defined |
| **Circulating supply** | Approximately 238,000,000 CTXC, near 79 percent of the cap |
| **Emission** | Native proof of work mining, originally 2.5 CTXC per block on a four year halving schedule, plus a large preallocation |
| **Mirror controls** | Owner is an externally owned account `0xb84041d064397bd8a1037220d996c16410c20f11`; the mirror is pausable but not a mint source |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the deprecated CTXC mirror on Ethereum returned:

| Check | Result |
|-------|--------|
| Token identity | Cortex Coin, CTXC, 18 decimals, verified |
| Total supply | 299,792,458, matching the hard cap and fully defined |
| Mint authority | None, the mirror does not inherit any mintable module |
| Pause authority | Present, held by an externally owned account, currently not paused |
| Upgradeable | No, the implementation slot is empty, so it is not a proxy |
| Transfer fee | None |

**Interpretation.** At the contract level the mirror is clean: a fixed cap, non mintable, non upgradeable ERC 20 with no fee on transfer. The one residual contract concern is that the pause function sits behind a single externally owned account rather than a multisig, so that account could freeze transfers on the mirror. This is a minor caution rather than a red flag, and it matters less than usual because the mirror is deprecated. The material concerns for this project sit at the market and product level below, not in the token contract itself.

---

## 3. Claim vs Reality: "Native coin of a thriving AI chain"

> Site: Cortex presents CTXC as the native fuel of an active layer one that runs AI and AI powered applications, with the coin paying for transactions and inference and rewarding miners and model contributors.

**Reality: the coin has stopped trading on listed exchanges.** CoinGecko reports that CTXC tokens have stopped trading on all exchanges it lists, with a market capitalization around 186,000 dollars, a price near 0.0008 dollars, and about 50,000 dollars of daily volume. The token was delisted by OKX in June 2025 and by ONUS in April 2025, and Bithumb announced a delisting citing a failure to address investment warnings and a lack of disclosure transparency. A native coin whose primary claim is being the fuel of a thriving ecosystem, yet which can no longer be bought or sold on any listed venue and carries a market capitalization in the low six figures, has thin real utility today. The chain still mines, but the economic life around the token has largely drained away.

---

## 4. Claim vs Reality: "On chain AI inference is live" (CVM and Synapse)

> Site: Cortex is described as the first decentralized world computer capable of running AI on the blockchain, with the Cortex Virtual Machine executing AI models on chain using GPUs and Synapse guaranteeing deterministic inference results.

**Reality: real technology, no visible current usage.** The Cortex Virtual Machine is a genuine and historically pioneering piece of engineering, and Cortex was an early mover in on chain inference, so this is not vaporware. However, MEFAI found no public, verifiable evidence that on chain AI inference is being used at any meaningful scale today. The network hashrate reflects miners chasing block rewards rather than measurable inference demand, and the official explorer that would substantiate contract and inference activity, cerebro.cortexlabs.ai, was unreachable across repeated attempts at review time. A capability that exists but is not demonstrably used, and whose activity dashboard is not reachable, is an aspiration kept alive rather than a delivered, in demand product.

---

## 5. Claim vs Reality: "ZkMatrix layer two"

> Site: ZkMatrix is presented as a Cortex layer two using zkRollup technology to increase throughput and cut fees for AI computation, with third party writeups crediting it for a large surge in AI model deployments.

**Reality: not yet delivered by the project's own timeline.** ZkMatrix appears in the project's own published roadmap as reaching its main version only in 2026, which places the flagship scaling layer in the future rather than in production. The widely repeated claim of a 400 percent surge in AI model deployments traces to third party marketing pages, not to independently verifiable on chain data, and MEFAI could not corroborate it. Presenting a roadmap item as an existing pillar, and repeating an unverified traction figure, is a transparency gap between the marketing framing and the delivered state.

---

## 6. Claim vs Reality: "Active development" and the public surface

> Site: The project positions itself as an actively developed AI chain with open source code and live infrastructure.

**Reality: quiet maintenance on an aging surface.** There is genuine credit due here. The core client, CortexTheseus, and several supporting repositories under the CortexFoundation GitHub organization still received commits into 2026, so the codebase is not abandoned. The nature of that activity, though, reads as maintenance and dependency upkeep rather than major new delivery, and the outward surface is aging: the marketing site carries a 2018 to 2023 copyright, and the explorer was unreachable at review time. The picture is a mature project on quiet, custodial maintenance, not one in active growth.

---

## 7. Positive Findings (Credited)

- The Cortex native layer one genuinely exists and still produces blocks through proof of work, with a network hashrate around 862 Gps and roughly 4,800 blocks per day in mid 2026.
- The Cortex Virtual Machine and on chain inference concept are real and historically pioneering, and Cortex was an early mover in the space.
- The core client and supporting repositories are still maintained on GitHub into 2026, so the code is not abandoned.
- The deprecated Ethereum mirror is a clean, fixed cap (299,792,458), non mintable, non upgradeable, no fee token.
- The supply story is honest and precise, capped at 299,792,458 as a deliberate reference to the speed of light, with circulating supply near 238,000,000.
- The homepage carries no wallet integration, no addresses, no obfuscated scripts, no fabricated audit badges, and no scam vectors, so it is low risk for end users.

---

## 8. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| CTXC 001 | **HIGH** | CTXC has stopped trading on all exchanges listed by CoinGecko; market capitalization around 186,000 dollars, price near 0.0008 dollars, daily volume around 50,000 dollars; delisted by OKX, ONUS, and Bithumb. |
| CTXC 002 | **HIGH** | The flagship AI inference story shows no verifiable current usage; CVM and Synapse exist but demonstrable on chain inference demand is not evident, and the explorer was unreachable at review time. |
| CTXC 003 | **MEDIUM** | ZkMatrix is framed as a live pillar but reaches its main version only in 2026 per the project's own roadmap; the 400 percent deployment surge claim is unverified third party marketing. |
| CTXC 004 | **MEDIUM** | Aging public surface: the marketing site carries a 2018 to 2023 copyright and the official explorer cerebro.cortexlabs.ai was unreachable across repeated attempts. |
| CTXC 005 | **LOW** | The deprecated ERC 20 mirror is pausable by a single externally owned account `0xb84041d064397bd8a1037220d996c16410c20f11`, which could freeze transfers on the mirror (currently not paused). |
| CTXC 006 | **LOW** | Wrong destination risk: the ERC 20 mirror is deprecated and holders must swap to the native coin; sending mirror tokens to a native mainnet address risks loss, and the homepage carries no explicit do not send warning. |
| CTXC 007 | **INFO** | Native chain is genuinely live and still mining, and the core client is maintained into 2026 (positive). |
| CTXC 008 | **INFO** | The token contract is clean, fixed cap, non mintable, non proxy, and no fee, and the frontend is clean with no scam vectors (positive). |

---

## 9. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, fixed cap, non mintable, no transfer fee |
| Supply / minting | Low risk | Fully capped at 299,792,458, no mint function on the mirror |
| Product reality | High risk | Flagship ZkMatrix undelivered, no verifiable on chain inference usage, explorer unreachable |
| Traction | High risk | Token stopped trading on all listed exchanges, market cap near 186,000 dollars, multiple delistings |
| Transparency | Medium risk | Honest supply and no scam vectors, but live framing of aspirational features and an aging public surface |

---

## 10. Technical Specifications

| Item | Value |
|------|-------|
| Canonical asset | Cortex native coin (proof of work layer one) |
| Legacy mirror contract | `0xea11755ae41d889ceec39a63e6ff75a02bc1c00d` (deprecated) |
| Decimals | 18 |
| Max supply | 299,792,458 CTXC (hard cap) |
| Circulating supply | Approximately 238,000,000 CTXC |
| Emission | Proof of work mining, 2.5 CTXC per block originally, four year halving, plus preallocation |
| Upgradeable | No, the mirror is not a proxy |
| Mint authority | None on the mirror |
| Pause authority | Present on the mirror, held by an externally owned account, not paused |
| Transfer fee | None |
| Explorer | cerebro.cortexlabs.ai (unreachable at review time) |
| Code | CortexFoundation on GitHub, maintained into 2026 |

---

## 11. Conclusion

Cortex deserves genuine credit for being an early and honest attempt to put AI inference on chain, and the audit confirms that the chain still mines, the code is still maintained, and the token contract is clean and fixed at its speed of light cap. As a whole project, however, it scores 49 out of 100 and is Flagged, because the reality has drifted well behind the marketing. CTXC has stopped trading on every listed exchange and now carries a market capitalization in the low six figures after delistings by OKX, ONUS, and Bithumb, the flagship ZkMatrix layer two is a future roadmap item rather than a live pillar, there is no verifiable current usage of the on chain inference the project is built around, and the public surface is aging with a stale site and an unreachable explorer. The caution here is not a contract exploit or a fraud, it is a mature project that has quietly gone dormant while its site still speaks in the present tense.

---

## 12. Recommendations

**For the Cortex team:**
- Restore the official explorer at cerebro.cortexlabs.ai and publish live activity metrics so on chain inference usage can be verified.
- Clearly separate delivered features from roadmap items, and stop presenting ZkMatrix as a live pillar until its main version ships.
- Retract or independently substantiate the 400 percent deployment surge figure.
- Refresh the marketing site and add an explicit warning that the Ethereum mirror is deprecated and must not be sent to a native mainnet address.

**For users:**
- Treat CTXC as effectively dormant: it no longer trades on listed exchanges, liquidity is minimal, and the market capitalization is in the low six figures.
- Understand that the on chain AI inference narrative is real technology with no demonstrable current usage, not a thriving product.
- If holding the deprecated ERC 20 mirror, follow the official swap guidance and never send mirror tokens to a native mainnet address.

---

## 13. Verification

- MEFAI onchain analysis: a direct Ethereum read of the deprecated CTXC mirror `0xea11755ae41d889ceec39a63e6ff75a02bc1c00d` (identity Cortex Coin, 18 decimals, total supply 299,792,458 equal to the hard cap, no mint module, empty implementation slot so non proxy, pausable by an externally owned account and currently not paused, no transfer fee).
- Market checks: CoinGecko for CTXC price near 0.0008 dollars, market capitalization around 186,000 dollars, daily volume near 50,000 dollars, circulating supply about 238,000,000, and the note that trading has stopped on all listed exchanges; public reporting of delistings by OKX (June 2025), ONUS (April 2025), and Bithumb.
- Product and code checks: live fetch of www.cortexlabs.ai (CVM, Synapse, and ZkMatrix positioning, 2018 to 2023 copyright), repeated unreachable fetches of the explorer cerebro.cortexlabs.ai, the CortexFoundation GitHub organization showing core client maintenance into 2026, and public mining data showing the chain still producing roughly 4,800 blocks per day at a hashrate around 862 Gps in mid 2026.
- Frontend integrity: the homepage is a static marketing site with no wallet integration, no addresses, no obfuscated or external crypto scripts, no fabricated audit badges, and no scam vectors, reading as clean and low risk for end users.
