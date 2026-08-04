# Security Audit Report: NATIX Network (NATIX) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | NATIX Network |
| **Token Symbol** | NATIX |
| **Mint (Solana)** | `FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX` |
| **Chain** | Solana (classic SPL Token, not Token 2022) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **70/100** |
| **Overall Risk** | **LOW to MEDIUM** |
| **Verdict** | **Passed** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

NATIX Network markets itself as a decentralized physical infrastructure network for mapping and Physical AI, where drivers turn dashcam and phone camera footage into fresh map data and AI training data and are rewarded in the NATIX token. Unlike many projects that sell a DePIN story, NATIX largely delivers one. The audit finds a real product, a token that is genuinely used, and a clean, fully renounced contract. The standout caution is economic rather than technical.

1. **The mapping DePIN is real and live.** The network collects footage through the Drive& smartphone app and the VX360 plug in device for Tesla vehicles, and it publishes an interactive coverage map at https://coverage.natix.network/ that renders live from a backend. The homepage network statistics, on the order of 2,890,321 registered drivers, 1,932,230 kilometers mapped, 323,321 map data detections and coverage across 33 countries, are pulled from the project's own live coverage API rather than being hardcoded numbers.
2. **The token is actually used.** NATIX powers contributor rewards, and the project runs a real Deep Staking dApp that hardcodes the correct official mint and lets users stake to earn passive rewards. The team has also executed real onchain token burns, including a 110 million NATIX burn in April 2026 and a larger cumulative reduction, which is behavior consistent with a token that has live utility rather than a purely speculative ticker.
3. **The contract is clean and fully renounced.** MEFAI's direct RPC read shows a classic SPL mint with six decimals, roughly 99.29 billion of a stated 100 billion cap minted, a null mint authority and a null freeze authority, no transfer fee and no transfer hooks. No wallet can create new supply and no wallet can freeze balances, so contract level risk is low.
4. **The main caution is dilution, not the contract.** Independent trackers put circulating supply near 40 percent, which means close to 60 percent of the supply still unlocks on a vesting schedule that stretches into 2028. Team and advisors at 20 percent and early backers near 25 percent make up much of that overhang. There is also product transition risk, since the Drive& phone app is being sunset in favor of VX360, and liquidity lock and depth were not verified from chain in this read only review.

NATIX is a genuine, working DePIN with a clean and fully renounced token and real utility. The dominant residual risk is the large unlock overhang to 2028. This lands NATIX at 70 out of 100, Passed.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | NATIX Network / NATIX |
| **Mint (Solana)** | `FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX` |
| **Decimals** | 6 |
| **Token program** | Classic SPL Token (not Token 2022), no extensions |
| **Max supply** | 100,000,000,000 stated; roughly 99,292,928,176 minted, which is the practical hard cap because mint authority is null |
| **Circulating** | Roughly 40.56 billion, about 40.6 percent per independent trackers |
| **Contract controls** | Mint authority null (renounced), freeze authority null (renounced) |

The published mint on the official site matches the address confirmed live against the Solana mainnet RPC. The token trades on centralized venues including Gate, KuCoin and MEXC and on Solana pools including Raydium and Orca.

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the NATIX mint on Solana returned:

| Check | Result |
|-------|--------|
| Token identity | NATIX, six decimals, verified against the official mint |
| Token program | Classic SPL Token, no Token 2022 extensions |
| Supply | Roughly 99.29 billion minted against a stated 100 billion cap |
| Mint authority | Null (renounced), no new supply can ever be created |
| Freeze authority | Null (renounced), no wallet balance can be frozen |
| Transfer fee | None |
| Transfer hooks | None |

**Interpretation.** At the contract level NATIX is strong. It is a classic SPL mint with both authorities renounced, which removes the two most common Solana token risks, hidden minting and account freezing, at the same time. There is no transfer fee extension and no transfer hook, so transfers are ordinary and predictable. Supply is fixed on the published vesting schedule. The residual risks for this project are therefore economic rather than technical, chiefly the large scheduled unlocks and the liquidity depth and lock status that this read only review did not verify from chain.

**Frontend safety.** MEFAI's frontend review found the public marketing site holds no token or wallet address and performs no wallet connection, loading only reputable third party scripts, while the token interaction lives on the separate staking app. That staking app hardcodes the correct official mint and connects wallets through standard Solana wallet adapter and WalletConnect libraries. It requests signatures and transactions, which is expected for staking, and shows none of the classic drainer traits, meaning no unlimited approvals, no transfer of all assets, no lookalike mint and no obfuscated or remotely injected code. The one caveat is that the shipped bundles were reviewed statically rather than by driving a live wallet transaction end to end.

---

## 3. Claim vs Reality: "A live, community mapping DePIN"

> Site: NATIX presents itself as a decentralized camera network where drivers map the world with dashcam and phone footage and the coverage grows in real time.

**Reality: the DePIN is genuinely live and used.** The network has two real data collection products, the Drive& smartphone app and the VX360 plug in device that taps a Tesla's external cameras and uploads footage over Wi Fi. The coverage map at https://coverage.natix.network/ is a working dynamic application that renders from a backend rather than a static graphic, and the project is listed on independent DePIN trackers such as DePINscan and DePIN Hub. This is a delivered product, not a rendering of intent, which is the single biggest differentiator from projects whose flagship experience is broken.

---

## 4. Claim vs Reality: "Live network statistics"

> Site: The homepage advertises large network metrics, on the order of 2,890,321 registered drivers, 1,932,230 kilometers mapped, 323,321 map data detections, 165,000 plus monthly multi camera footage hours and 33 countries of coverage.

**Reality: the numbers come from a live API, not a hardcoded banner.** MEFAI's frontend review confirmed that the homepage headline statistics are pulled live from the project's own coverage API, and the coverage map renders the same underlying data. That does not independently prove every figure is audited, but sourcing the headline metrics from a live product API rather than a static number is a meaningful transparency positive, and it is consistent with a network that is actually operating.

---

## 5. Claim vs Reality: "NATIX is used for rewards and staking"

> Site: NATIX is described as the unit of account for the network, used to reward contributors and to stake for passive rewards and governance.

**Reality: the token utility is real, not decorative.** Contributors are paid in NATIX for shared footage, and the NATIX Deep Staking dApp is a real product that hardcodes the correct official mint and lets holders stake to earn tiered rewards. The team has also carried out real onchain burns, including a 110 million NATIX burn in April 2026 and a larger cumulative reduction over time, which is active token management tied to the product. Unlike many tokens whose own flagship product settles in a different asset, NATIX is the medium of its own reward and staking loop.

---

## 6. Claim vs Reality: "100 billion max supply and fixed supply"

> Site: NATIX advertises a 100 billion maximum supply with a fixed, capped token.

**Reality: accurate on supply, understated on float.** The chain shows roughly 99.29 billion minted against the stated 100 billion cap, so the headline is accurate and slightly conservative rather than inflated, and because the mint authority is null no new tokens can ever be created. The area where a reader should look past the marketing is float. Independent trackers put circulating supply near 40 percent, which means close to 60 percent still unlocks on a vesting schedule reaching into 2028, with team and advisors at 20 percent and early backers near 25 percent forming much of the overhang. This is a genuine dilution consideration that the fixed supply framing does not surface on its own.

---

## 7. Claim vs Reality: "Partnerships and product roadmap"

> Site: NATIX highlights a roster of well known names, including Google, NVIDIA, Amazon, Solana, Valeo, Grab and the Autoware Foundation, and presents Drive& and VX360 as its product line.

**Reality: real relationships mixed with ecosystem branding, plus a live product pivot.** The Autoware Foundation membership and the Solana, Grab and Valeo associations are consistent with the project's mapping and autonomy focus, but marquee names such as Google, NVIDIA and Amazon read more as cloud and ecosystem relationships than as deep, shipped integrations, and a reader should verify the depth of any specific claim independently. Separately, the product line is in transition. The Drive& phone app is being sunset, with earnings disabled from May 1, 2026 and operations discontinued from July 1, 2026, while focus shifts to the VX360 Tesla device and a StreetVision subnet effort. The pivot is disclosed and forward looking rather than hidden, but the homepage still frames Drive& as a headline product, so the transition is worth flagging as execution risk.

---

## 8. Positive Findings (Credited)

- The mapping DePIN is real and live, with two working data collection products (Drive& and VX360) and an interactive coverage map at https://coverage.natix.network/.
- The token contract is a clean classic SPL mint with both mint and freeze authorities fully renounced, no transfer fee and no transfer hooks.
- The token has genuine utility, powering contributor rewards and a real Deep Staking dApp, with real onchain burns as active supply management.
- The homepage network statistics are sourced from a live coverage API rather than hardcoded, and the marketing site itself holds no wallet connection or address.
- Listings across reputable CEX venues (Gate, KuCoin, MEXC) and Solana DEX pools (Raydium, Orca) corroborate real market presence.

---

## 9. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| NATIX 001 | **MEDIUM** | Large unlock overhang: only about 40 percent circulating, with close to 60 percent vesting into 2028 (team and advisors 20 percent, early backers near 25 percent). Economic dilution risk, not a contract flaw. |
| NATIX 002 | **LOW** | Liquidity lock status and pool depth were not verified from chain in this read only review. |
| NATIX 003 | **LOW** | Product transition and framing: the Drive& phone app is being sunset (earnings off May 1, discontinued July 1, 2026) while the homepage still frames it as a headline product, and several marquee partnerships read as ecosystem relationships rather than deep integrations. |
| NATIX 004 | **INFO** | Contract is a clean classic SPL mint with mint and freeze authorities fully renounced, no fee and no hooks (positive). |

---

## 10. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified official mint, classic SPL, no fee, no hooks |
| Supply / minting | Low risk | Mint authority renounced, supply fixed at the minted amount |
| Product reality | Low risk | Live DePIN, working apps, live coverage map and API |
| Token utility | Low risk | Real rewards and staking dApp, active onchain burns |
| Tokenomics / float | Medium risk | Only about 40 percent circulating, near 60 percent unlock overhang to 2028 |
| Liquidity | Medium risk | LP lock and depth not verified from chain |
| Transparency | Low risk | Live stat sourcing and disclosed vesting, with minor partnership and pivot framing caveats |

---

## 11. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX` |
| Chain | Solana |
| Token program | Classic SPL Token (no Token 2022 extensions) |
| Decimals | 6 |
| Supply minted | Roughly 99.29 billion of a 100 billion stated cap |
| Circulating | Roughly 40.6 percent per independent trackers |
| Mint authority | Null (renounced) |
| Freeze authority | Null (renounced) |
| Transfer fee | None |
| Transfer hooks | None |
| Venues | Gate, KuCoin, MEXC (CEX); Raydium, Orca (DEX) |

---

## 12. Conclusion

NATIX Network is a real decentralized physical infrastructure network with a clean, fully renounced token, and it scores 70 out of 100, Passed. The mapping DePIN is genuinely live, with working data collection products, an interactive coverage map, and homepage statistics sourced from a live coverage API rather than a static banner. The token is actually used, powering contributor rewards and a real staking dApp, and supported by real onchain burns. At the contract level the risk is low, because both the mint and freeze authorities are renounced and there is no transfer fee or hook. The caution here is not a contract exploit or a broken product, it is economics: only about 40 percent of supply is circulating and close to 60 percent still unlocks into 2028, and liquidity lock and depth were not verified from chain. NATIX is a project whose product is largely delivered and whose main open risk sits in its unlock schedule.

---

## 13. Recommendations

**For the NATIX team:**
- Surface the vesting and unlock schedule prominently alongside the fixed supply framing, so the near 60 percent overhang to 2028 is transparent to holders.
- Publish verifiable liquidity lock details and pool depth so the market presence can be confirmed from chain.
- Update the homepage product framing to reflect the Drive& sunset and the VX360 focus, and label big name relationships as ecosystem versus shipped integration.
- Continue publishing onchain burn and reward data to keep the token utility loop verifiable.

**For users:**
- Treat the DePIN and token utility as real and delivered, which is a genuine positive relative to peers.
- Size the dilution risk deliberately: only about 40 percent of supply circulates today and a large amount unlocks through 2028.
- Understand that liquidity lock and depth were not verified from chain, and interact with staking only through the official staking app and the confirmed mint.

---

## 14. Verification

- MEFAI onchain analysis: a direct Solana RPC read of the NATIX mint (identity, classic SPL Token program, six decimals, roughly 99.29 billion minted, null mint authority, null freeze authority, no transfer fee, no transfer hooks), plus circulating supply and vesting dates from independent trackers.
- Frontend review: static review of the marketing site (no wallet connection or address, reputable third party scripts, headline stats sourced from the live coverage API) and the staking app (correct hardcoded mint, standard Solana wallet adapter and WalletConnect, no drainer traits), reviewed statically rather than by driving a live wallet transaction.
- Product checks: live fetch of the coverage map at https://coverage.natix.network/ (dynamic, backend rendered), confirmation of the Drive& and VX360 products, and the disclosed Drive& sunset and VX360 pivot.
- Project statements: the project's website and blog (network statistics, staking and rewards, the 100 billion supply claim, token burns, and the partnership roster).
