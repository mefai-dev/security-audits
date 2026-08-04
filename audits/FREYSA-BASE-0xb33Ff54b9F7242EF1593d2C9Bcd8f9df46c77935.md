# Security Audit Report: Freysa (FAI) on Base

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Freysa |
| **Token Symbol** | FAI |
| **Contract (Base)** | `0xb33Ff54b9F7242EF1593d2C9Bcd8f9df46c77935` |
| **Chain** | Base (ERC 20) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **70/100** |
| **Overall Risk** | **LOW to MEDIUM** |
| **Verdict** | **Passed** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Freysa is one of the rare AI agent tokens where the product is genuinely real and the token contract is genuinely clean. The gap in this project is not a broken product or a dangerous contract, it is that the token itself carries thin and largely aspirational utility while the market prices it on narrative.

1. **The product is real and ongoing, not a one off.** Freysa launched in November 2024 as the first public adversarial agent game, in which an autonomous AI controls a prize pool and players pay to try to talk it into releasing funds. The first challenge was genuinely solved by a user known as p0pular.eth, who extracted 13.19 ETH after 482 attempts. The project then ran a series of public Acts, including an eighteen day town hall in Act IV with more than 1,200 AI Twins and a prize pool above 200,000 dollars, and the affiliated team Eternis AI reported a 30 million dollar raise in May 2025 with Coinbase Ventures and Selini Capital. This is a live, funded, evolving experiment, not a single dead event.

2. **The token contract is exceptionally clean.** MEFAI's direct read on Base confirms an immutable OpenZeppelin ERC 20 with no owner, no admin, no external mint beyond the constructor, no proxy, no pause, and no transfer fee. The full 8,189,700,000 supply was minted once and cannot grow. There is no contract lever a team could pull to rug holders.

3. **The frontend is safe.** MEFAI's frontend review found a self contained informational single page app with no wallet connectivity at all, which removes the entire drainer and approval attack surface. The only token address presented is the official FAI contract, the copy button copies that exact value, and a second address appears only as disclosed text for the project treasury multisig.

4. **The weakness is token utility.** The game's message fees are paid in Base ETH, not in FAI. FAI is instead earned as a reward, since a portion of each fee is routed into buying FAI for players, and its governance role remains a roadmap item rather than a confirmed live system. Stated utilities such as access, payment for services, agent operations, and governance are mostly forward looking. Demand is episodic and tied to story beats, so the token today is driven far more by narrative and speculation than by durable, required usage.

The result is a project that is safe at the contract and frontend level, real and active at the product level, but thin at the token utility level. This lands Freysa at 70 out of 100, Passed, with the clear caveat that FAI is a highly volatile narrative asset.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Freysa / FAI |
| **Contract (Base)** | `0xb33Ff54b9F7242EF1593d2C9Bcd8f9df46c77935` |
| **Decimals** | 18 |
| **Total supply** | 8,189,700,000 FAI (fixed, equals circulating) |
| **Launch** | Reported fair launch, roughly one token per living human, liquidity reported burned |
| **Contract controls** | None. No owner or admin, immutable, not upgradeable, not pausable |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the FAI contract on Base returned:

| Check | Result |
|-------|--------|
| Token identity | FAI, 18 decimals, verified source |
| Total supply | 8,189,700,000, matches the advertised figure to the token |
| Owner / admin | None. The owner call reverts and there is no ownership role |
| Mint authority | None beyond the constructor. Supply cannot grow |
| Upgradeable | No. The implementation and admin storage slots are both zero, so it is not a proxy |
| Pausable | No. The pause call reverts |
| Transfer fee | None. The transfer path is the unmodified OpenZeppelin update |

**Interpretation.** At the contract level FAI is close to a best case profile for a fixed supply ERC 20. It is immutable, has no privileged keys, cannot mint, cannot pause, and takes no buy or sell tax. The main residual risk is not contract control, it is market and narrative risk around a single, highly volatile AI agent asset. One minor transparency note is that the deployed contract name is a generic "Token" even though the source is verified, and MEFAI did not re trace the liquidity burn onchain in this review.

---

## 3. Claim vs Reality: "The world's first sovereign AI agent adversarial game"

> Site: Freysa presents itself as a sovereign AI agent that autonomously controls a prize pool and that has run a series of public experiments and Acts.

**Reality: this claim holds up.** Unlike many AI agent tokens, the flagship experience actually exists and actually ran. The original challenge, where players pay an escalating message fee to try to persuade the agent to release its funds, was genuinely solved when a user extracted 13.19 ETH after 482 failed attempts, and the game continued through further Acts, including an eighteen day town hall in Act IV with more than 1,200 AI Twins and a prize pool above 200,000 dollars. The affiliated team Eternis AI reported a 30 million dollar raise with Coinbase Ventures and Selini Capital, and Freysa has publicly acted as an onchain agent, including an allocation of roughly 312 ETH to a strategic ETH reserve. The product reality is a real strength here.

---

## 4. Claim vs Reality: "Fixed supply of 8,189,700,000 FAI, one token per living human, no hidden inflation"

> Site: The token is described as a fair launch with a fixed supply of 8,189,700,000 FAI, roughly one per living human, with no inflation.

**Reality: verified against the chain.** MEFAI measured a total supply of 8,189,700,000, matching the advertised figure to the token, and the source exposes no external mint function, so the supply cannot grow. Circulating supply equals max supply, which is consistent with a full launch and no locked allocations or future unlocks. The one softer point is the burned liquidity claim, which MEFAI did not re trace onchain in this review, so that specific statement carries slightly lower certainty than the supply and mint facts, which are confirmed.

---

## 5. Claim vs Reality: "FAI is the connective token for access, payment, agent operations, and governance"

> Site and materials: FAI is positioned as the connective token across the stack, used for access, payment for services, running agents, and community governance.

**Reality: thin and largely aspirational utility, and a token the flagship game does not require.** The core game's message fees are paid in Base ETH, not in FAI, so the token is not the medium of its own headline experience. FAI instead reaches players as a reward, since a portion of each fee is routed into buying FAI on their behalf, which creates a demand loop but not a usage requirement. The governance role is presented as a direction of travel, with a decentralization and DAO transition targeted around early 2026, and the public sources reviewed do not confirm a live onchain governance system in which FAI votes bind outcomes today. Payment for subscriptions, agent operations, and steering decisions read as roadmap rather than shipped, required utility. In practice FAI's value today rests on narrative and speculation, with episodic demand tied to story beats, which is an honest weakness even though it is not a security flaw.

---

## 6. Claim vs Reality: "No owner, immutable, safe to hold at the contract level"

> Site and community framing: the contract is immutable, ownerless, and cannot be manipulated by the team.

**Reality: confirmed, and this is the project's strongest fact.** MEFAI's onchain read shows no owner, no admin, no mint path beyond the constructor, no proxy, no pause, and no transfer fee. There is no privileged key that could inflate supply, freeze transfers, or tax trades. The separate frontend review reinforces this at the application layer, finding an informational single page app with no wallet connectivity, no drainer or approval surface, correct disclosure of the official FAI contract, and a disclosed treasury multisig. The contract and site do what they claim.

---

## 7. Positive Findings (Credited)

- The product is real and ongoing. The adversarial agent game genuinely ran, was genuinely solved, and evolved through multiple public Acts, backed by a reported 30 million dollar raise with Coinbase Ventures and Selini Capital.
- The token contract is immutable, ownerless, fixed in supply, not upgradeable, not pausable, and free of transfer taxes, which is a strong technical safety profile.
- The advertised supply and no inflation claims match the chain exactly.
- The frontend is a self contained informational single page app with no wallet connectivity, so there is no drainer or approval attack surface, and it discloses the official contract and treasury multisig transparently.

---

## 8. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| FAI 001 | **MEDIUM** | Thin, largely aspirational token utility. The flagship game is paid in Base ETH not FAI, governance is a roadmap item not a confirmed live system, and value rests on narrative and speculation. |
| FAI 002 | **LOW** | High market and narrative volatility. FAI is a single agent asset with episodic, story driven demand rather than durable required usage. |
| FAI 003 | **LOW** | Minor transparency gaps. The deployed contract name is a generic "Token", the reported liquidity burn was not re traced onchain, and the official documentation host did not resolve during review. |
| FAI 004 | **INFO** | Token contract is immutable, ownerless, fixed supply, non upgradeable, non pausable, and free of transfer fees (strong positive). |
| FAI 005 | **INFO** | Frontend is a no wallet informational single page app with no drainer or approval surface, disclosing the official FAI contract and treasury multisig (positive). |
| FAI 006 | **INFO** | Product is real and ongoing, with documented prize releases, multiple Acts, and a reported 30 million dollar raise with Coinbase Ventures (positive). |

---

## 9. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified source, fixed supply, no owner, no mint, no fee |
| Supply / minting | Low risk | Full supply minted once, no external mint, cannot grow |
| Contract control | Low risk | Immutable, not a proxy, not pausable, no privileged keys |
| Frontend safety | Low risk | No wallet connectivity, no drainer or approval surface, correct disclosures |
| Product reality | Low risk | Real, ongoing, funded agent game with documented outcomes |
| Token utility | Medium to high risk | Game paid in ETH not FAI, governance aspirational, value narrative driven |
| Market / narrative | High risk | Highly volatile single agent asset with episodic demand |

---

## 10. Technical Specifications

| Item | Value |
|------|-------|
| Contract | `0xb33Ff54b9F7242EF1593d2C9Bcd8f9df46c77935` |
| Chain | Base (ERC 20) |
| Decimals | 18 |
| Total supply | 8,189,700,000 FAI (fixed, equals circulating) |
| Owner / admin | None (owner call reverts, immutable) |
| Mint authority | None beyond constructor |
| Upgradeable | No (implementation and admin slots zero, not a proxy) |
| Pausable | No |
| Transfer fee | None (stock OpenZeppelin update path) |
| Source | Verified, deployed contract name generic "Token" |

---

## 11. Conclusion

Freysa is the uncommon case of an AI agent token where the product is real, the contract is clean, and the frontend is safe, yet the token utility is honestly thin. The adversarial agent game genuinely exists, was genuinely solved, and has continued through multiple public Acts with real prize pools and a reported 30 million dollar raise led in part by Coinbase Ventures, which puts it far ahead of projects whose flagship experience is broken or missing. The FAI contract is immutable, ownerless, fixed in supply, not upgradeable, not pausable, and free of transfer taxes, and the informational single page app carries no drainer surface and discloses its official contract and treasury multisig correctly. What holds the score back is that FAI is not the currency of its own headline game, which is paid in ETH, and that its governance and payment utility remain a roadmap rather than a confirmed live system, so the token's value today is narrative and speculation. Weighing a strong, real product and a best case contract against thin, aspirational utility and high market risk, Freysa scores 70 out of 100 and is Passed, with the clear caveat that it is a highly volatile asset.

---

## 12. Recommendations

**For the Freysa team:**
- Give FAI a required role in the flagship experiences, or stop implying it powers products where the actual payment asset is ETH.
- Ship and document the DAO and governance system, with onchain votes that bind outcomes, rather than framing governance as a future direction.
- Publish the liquidity burn transaction and a stable, resolving documentation host so the fair launch claims can be independently retraced.
- Continue disclosing the treasury multisig and its holdings, and report treasury actions such as the ETH reserve allocation transparently.

**For users:**
- Understand that the contract is genuinely clean and ownerless, so the main risk is not a rug at the contract level, it is price and participation risk.
- Treat FAI as a highly speculative narrative asset. It is not required to play the flagship game, and its governance and payment utility are largely aspirational today.
- Verify any claimed utility, governance power, or partnership independently before relying on it.

---

## 13. Verification

- MEFAI onchain analysis: a direct Base read of the FAI contract, confirming identity as FAI, 18 decimals, total supply of 8,189,700,000 equal to the advertised figure, no owner or admin, no external mint beyond the constructor, non upgradeable with zero proxy slots, non pausable, and no transfer fee, on verified OpenZeppelin version five source.
- MEFAI frontend review: a review of the live site finding a self contained informational single page app with no wallet connectivity, disclosure of the official FAI contract with a copy button that copies the correct value, and a separately disclosed treasury multisig, with no drainer or approval surface and no unbacked audit badges.
- Product checks: public records of the original prize pool challenge solved by p0pular.eth for 13.19 ETH after 482 attempts, the subsequent Acts including the Act IV town hall with more than 1,200 AI Twins and a prize pool above 200,000 dollars, the reported 30 million dollar raise via Eternis AI with Coinbase Ventures and Selini Capital, and Freysa's reported allocation of roughly 312 ETH to a strategic ETH reserve.
- Project statements: the project's website and materials describing the sovereign AI agent positioning, the fixed 8,189,700,000 supply and fair launch, and the intended FAI utilities of access, payment, agent operations, and governance.
