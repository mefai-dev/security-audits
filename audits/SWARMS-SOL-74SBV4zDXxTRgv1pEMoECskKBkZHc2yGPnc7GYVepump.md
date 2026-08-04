# Security Audit Report: Swarms (SWARMS) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Swarms |
| **Token Symbol** | SWARMS |
| **Contract (Solana mint)** | `74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump` |
| **Chain** | Solana (classic SPL Token) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **50/100** |
| **Overall Risk** | **HIGH** |
| **Verdict** | **Flagged** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Swarms markets itself as the token that powers a decentralized multi agent AI economy, combining an open source orchestration framework with an onchain agent marketplace. The audit finds that, unusually for a token of this origin, the underlying product is genuinely real, but the token that sits on top of it is a speculative pump.fun asset with thin utility, a severe price collapse, and a DAO that moves user funds by asking people to send tokens to a treasury address.

1. **The framework and marketplace are real.** The Swarms multi agent framework, built by The Swarm Corporation, is a genuine, actively maintained open source project with roughly 7,000 GitHub stars and thousands of commits, and swarms.world operates a working marketplace for agents, prompts, and tools. This is a real product, not a mockup.
2. **The token contract is clean.** A direct read of the mint shows the mint authority and the freeze authority are both null, the supply is permanently fixed at about 1 billion, and the mint is a classic SPL Token program with no transfer fee. At the contract level there is nothing to exploit.
3. **The token is a speculative meme origin asset with a severe drawdown.** SWARMS launched on pump.fun in December 2024 and reached an all time high of about 0.6055 dollars on January 7, 2025, near a 600 million dollar valuation, before collapsing about 98.8 percent to roughly 0.007 dollars and a 7 million dollar market cap at review. The narrative is AI infrastructure, but the price history is a memecoin.
4. **DAO participation is a send to address scheme.** The DAO invites holders to participate by manually sending SWARMS or SOL to a treasury address, and the DAO page advertises figures such as up to 20 percent APY, a 1,000 SWARMS minimum, and a 30 day minimum stake, yet MEFAI's frontend review found no visible onchain escrow or staking contract behind this. Yield style language attached to unescrowed deposits is a material user caution even when the receiving address is legitimate.

The contract is not a scam and the product genuinely exists, which lifts Swarms above a project whose flagship is broken. But the pump.fun origin, the near total drawdown, the thin real token utility, and the unescrowed send to address DAO keep it in caution territory. This lands Swarms at 50 out of 100, Flagged.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Swarms / SWARMS |
| **Contract (Solana mint)** | `74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump` |
| **Decimals** | 6 |
| **Supply** | About 999.97 million (raw 999968929278244), roughly a 1 billion nominal cap, about 100 percent circulating |
| **Origin** | Created on pump.fun on December 17, 2024, via a bonding curve rather than a formal token sale |
| **Contract controls** | Mint authority null and freeze authority null; classic SPL Token program with no extensions |
| **Market status** | Market cap equals fully diluted value at about 7 million dollars at review, down roughly 98.8 percent from an all time high near 600 million dollars |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the SWARMS mint on Solana returned:

| Check | Result |
|-------|--------|
| Token identity | SWARMS, 6 decimals, verified via RPC |
| Supply | About 999.97 million (raw 999968929278244), roughly 1 billion nominal, about 100 percent circulating |
| Mint authority | Null, so the supply is permanently fixed and no new tokens can be minted |
| Freeze authority | Null, so individual token accounts cannot be frozen |
| Token program | Classic SPL Token, not Token 2022, with no extensions |
| Transfer fee | None, so transfers move the full amount with no protocol level tax |

**Interpretation.** At the contract level SWARMS is clean. Both authorities are revoked, the supply is fixed, and there is no fee on transfer, which removes the most common Solana token risks of silent minting, account freezing, and hidden transfer taxes. The near fully circulating supply is also consistent with market cap equaling fully diluted value, so there is no large undisclosed unlock overhang visible on the mint. The real concerns for this project sit at the token utility, market, and DAO level below, not in the mint account. Project stated figures such as the community allocation percentage, holder concentration, and any liquidity or team lock could not be independently proven from the mint account and are treated as unverified.

---

## 3. Claim vs Reality: "A real multi agent AI framework and marketplace"

> Site: Swarms presents an enterprise grade multi agent orchestration framework and a marketplace at swarms.world for discovering, deploying, and monetizing agents, prompts, and tools.

**Reality: this claim holds.** Unlike many tokens with an AI story, the product here is genuinely real. The Swarms framework, published by The Swarm Corporation at github.com/kyegomez/swarms, is an actively maintained open source Python project with roughly 7,000 stars, close to a thousand forks, and thousands of commits, and it ships as an installable package. The swarms.world marketplace is a live platform, and the project reports figures such as more than 77 million dollars in cumulative trading volume and several million in liquidity, which are project stated but consistent with a functioning venue. MEFAI credits this: the framework and marketplace are real and usable, which is the strongest point in the project's favor.

---

## 4. Claim vs Reality: "SWARMS is the fuel of the agent economy"

> Site: SWARMS is described as the base currency and utility token of the ecosystem, tied to governance, staking rewards, protocol revenue share, exclusive feature access, and early product access.

**Reality: thin and largely aspirational utility.** The framework is open source and can be installed and run without holding or spending SWARMS, and the marketplace does not require the token as its settlement rail for the core experience. The listed utilities read as governance and access perks plus forward looking promises such as revenue share and staking rewards rather than mechanics that force real token demand today. This is the central gap for Swarms: a genuinely real AI infrastructure product exists, but the token riding on it captures little of that product's value and its utility is mostly narrative. The distance between the AI infrastructure story and a token whose price history is a pump.fun memecoin is wide.

---

## 5. Claim vs Reality: "Join the Swarms DAO"

> Site: The DAO invites community members to invest directly by sending SWARMS or SOL to the official treasury address, and the DAO page presents figures such as up to 20 percent APY, a 1,000 SWARMS minimum, active participation, a governance commitment, and a 30 day minimum stake.

**Reality: a send to address scheme with no visible onchain escrow.** MEFAI's frontend review of the reachable official Swarms frontends found that DAO participation is described as manually sending tokens to a treasury address, with no visible onchain escrow, vault, or staking contract mediating the deposit. The treasury address shown was the documented official one and the frontends were otherwise clean, with correct token references and no drainer or impostor patterns, so this is a design caution rather than a code exploit. Even so, attaching yield style language such as up to 20 percent APY and a minimum stake period to funds that a user simply sends to an address, with no smart contract holding or accounting for them, is a material risk. The user is trusting a treasury operator, not a verifiable escrow, and there is no onchain guarantee of return, withdrawal, or the advertised rate.

---

## 6. Claim vs Reality: "A serious AI infrastructure asset"

> Site: The overall positioning frames SWARMS as durable AI infrastructure and a foundational token for the agent economy.

**Reality: a pump.fun origin and a near total drawdown temper the narrative.** SWARMS was created on pump.fun on December 17, 2024, through a bonding curve rather than a structured sale, which is a memecoin style launch. It then rose to an all time high near 0.6055 dollars on January 7, 2025, at a valuation approaching 600 million dollars, before retracing roughly 98.8 percent to about 0.007 dollars and a market cap near 7 million dollars at review. The token is listed on several centralized venues and the mint is clean, but the combination of a pump.fun origin, an almost complete price collapse, and thin liquidity relative to the infrastructure narrative means the market treats this as a high volatility speculative asset. Impostor tokens also circulate, so users must confirm the official mint `74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump` before transacting.

---

## 7. Positive Findings (Credited)

- The Swarms multi agent framework is a genuine, actively maintained open source project (about 7,000 GitHub stars, thousands of commits) built by The Swarm Corporation, and swarms.world runs a real marketplace.
- The token contract is clean: mint authority null and freeze authority null, a permanently fixed supply of about 1 billion, and a classic SPL Token program with no transfer fee.
- The reachable official frontends behave as advertised, point every token reference to the correct official mint, load no untrusted third party scripts, and show no drainer, obfuscation, or impostor address patterns.
- The supply is about 100 percent circulating, so market cap equals fully diluted value and there is no large hidden unlock overhang visible on the mint.

---

## 8. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| SWARMS 001 | **HIGH** | DAO participation is a send to address scheme: users are asked to send SWARMS or SOL to a treasury address with no visible onchain escrow, while the DAO page advertises up to 20 percent APY, a 1,000 SWARMS minimum, and a 30 day minimum stake. |
| SWARMS 002 | **HIGH** | Severe speculative drawdown and meme origin: launched on pump.fun in December 2024, reached an all time high near 0.6055 dollars (about 600 million dollars) on January 7, 2025, then fell roughly 98.8 percent to about 0.007 dollars and a 7 million dollar market cap. |
| SWARMS 003 | **MEDIUM** | Thin token utility: the framework and marketplace function without requiring SWARMS, and the listed utilities are largely governance perks and forward looking promises, leaving a wide gap between the AI infrastructure narrative and real token demand. |
| SWARMS 004 | **MEDIUM** | Project stated figures not independently verifiable: the community allocation percentage, holder concentration, and any liquidity or team lock cannot be proven from the mint account. |
| SWARMS 005 | **LOW** | Impostor tokens circulate; only the mint `74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump` is official per swarms.world. |
| SWARMS 006 | **LOW** | The main marketplace homepage sat behind a bot challenge that could not be rendered from the review network, so that specific page should be re verified from a clean network. |
| SWARMS 007 | **INFO** | Positive: clean mint (authorities null, fixed supply, no transfer fee) and a real, actively maintained open source framework and marketplace. |

---

## 9. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified mint, fixed supply, both authorities revoked, no transfer fee |
| Supply and minting | Low risk | Mint authority null, permanently fixed supply of about 1 billion, about 100 percent circulating |
| Product reality | Low risk | Genuine, actively maintained open source framework and a working marketplace |
| DAO and user funds | High risk | Send to address treasury with no visible onchain escrow, paired with yield style APY language |
| Market and traction | High risk | Pump.fun origin, roughly 98.8 percent drawdown from near 600 million dollars, thin liquidity relative to the narrative |
| Transparency | Medium risk | Community percentage, holder distribution, and liquidity or team locks are project stated and unverified; AI infrastructure framing sits over a memecoin origin |

---

## 10. Technical Specifications

| Item | Value |
|------|-------|
| Contract (mint) | `74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump` |
| Chain | Solana |
| Token program | Classic SPL Token (not Token 2022), no extensions |
| Decimals | 6 |
| Supply | About 999.97 million (raw 999968929278244), roughly 1 billion nominal, about 100 percent circulating |
| Mint authority | Null (supply permanently fixed) |
| Freeze authority | Null (accounts cannot be frozen) |
| Transfer fee | None |
| Origin | pump.fun bonding curve, December 17, 2024 |
| Market status | Market cap equals fully diluted value at about 7 million dollars at review; all time high near 0.6055 dollars on January 7, 2025 |

---

## 11. Conclusion

Swarms is an unusual case. The product is genuinely real, since the multi agent framework is a live, actively maintained open source project and swarms.world runs a working marketplace, and the token contract is clean, with both authorities revoked, a fixed supply, and no transfer fee. That combination keeps it well clear of the broken flagship category. As an asset, however, it scores 50 out of 100 and is Flagged, because the token that sits on this real product is a pump.fun origin memecoin that has fallen roughly 98.8 percent from a valuation near 600 million dollars, its onchain utility is thin, and its DAO moves user funds by asking people to send tokens to a treasury address with no visible onchain escrow while advertising yield. The caution here is not a contract exploit, it is the gap between a serious AI infrastructure narrative and a speculative token whose participation mechanics and price history do not match that story.

---

## 12. Recommendations

**For the Swarms team:**
- Replace the send to address DAO with a verifiable onchain escrow, vault, or staking contract, and stop presenting APY figures for deposits that no contract actually holds or accounts for.
- Give SWARMS concrete, enforced utility in the framework and marketplace, or stop implying the token is the fuel of the ecosystem.
- Publish verifiable liquidity, lock, and holder distribution data rather than stated figures.
- Clearly label the pump.fun origin and the price history so the AI infrastructure framing does not overstate durability.

**For users:**
- Treat SWARMS as a high volatility speculative asset: it launched on pump.fun and is down roughly 98.8 percent from its all time high.
- Understand that DAO participation means sending tokens to a treasury address with no onchain escrow, so any advertised APY is a trust based promise, not a smart contract guarantee.
- Confirm the official mint `74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump` before transacting, since impostor tokens exist.

---

## 13. Verification

- MEFAI onchain analysis: a direct Solana RPC read of the SWARMS mint (identity, 6 decimals, supply of about 999.97 million, mint authority null, freeze authority null, classic SPL Token program, no transfer fee).
- Frontend review: inspection of the reachable official Swarms frontends confirming correct token references to the official mint, the documented DAO treasury address, no untrusted third party scripts or drainer patterns, and the send to address DAO design with no visible onchain escrow; the main marketplace homepage sat behind a bot challenge and should be re verified from a clean network.
- Product checks: the Swarms framework repository at github.com/kyegomez/swarms (about 7,000 stars, thousands of commits, actively maintained) and the swarms.world marketplace.
- Market data: all time high near 0.6055 dollars on January 7, 2025, a drawdown of roughly 98.8 percent, and a market cap equal to fully diluted value near 7 million dollars at review, with a pump.fun launch dated December 17, 2024.
- Project statements: the project's website and documentation (framework and marketplace positioning, token utility claims, and the DAO participation and APY figures).
