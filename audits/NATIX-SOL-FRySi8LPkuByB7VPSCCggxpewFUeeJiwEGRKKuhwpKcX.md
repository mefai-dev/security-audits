# NATIX Network (NATIX): Whitepaper Claims vs Code Reality

**Score: 64/100, MEDIUM RISK (Passed)**

Date: 2026-08-06
Analyst: MEFAI Security, senior source code auditor (ICE/ION deep audit, read only, public sources)

**Token (live, verified on Solana mainnet 2026-08-06):**
Mint `FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX`. Program owner `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (classic SPL Token, NOT Token-2022). Decimals 6. Mint authority `null` (revoked, supply is fixed). Freeze authority `null` (accounts cannot be frozen). Supply 99,292,928,176.772374 NATIX (about 99.29 billion, consistent with a 100 billion cap net of burns). isInitialized true.

**Websites:** natix.network (main), natix.network/tokens, natix.network/blog

**GitHub:** github.com/natixnetwork (5 public repos) and the legacy github.com/natix-io org (video and computer vision heritage, plus the active nxdl repo)

---

## Severity Summary

| Area | Finding | Severity | Label |
|------|---------|----------|-------|
| SPL token authorities | Mint and freeze authority both revoked, fixed supply, classic SPL | Positive | CONFIRMED IN CODE (on chain) |
| Drive to earn rewards | Earned as off chain in app points on a centralized leaderboard, converted later | High | FALSE (claim of on chain, verifiable rewards) |
| On chain program scope | A real staking program exists (saphira) but is admin centralized and the live mainnet program is not source matched; no data marketplace program is public | Medium | OVERSTATED |
| AI camera and map data (VX360) | Device and vision code are real, but the ingestion and processing pipeline is closed and not decentralized in a verifiable way | Medium | OVERSTATED |
| Traction (drivers, km) | Partnerships are real and press confirmed; driver and kilometer counts are self reported and inconsistent across sources | Medium | OVERSTATED (numbers) / CONFIRMED (partners) |
| Core code transparency | Drive& app, mapping backend, reward ledger and conversion service are closed source | Medium | Core code closed or unverifiable |
| Staking reward math | On chain reward uses f64 floating point compounding in the public source | Low | Code quality note |
| Deploy hygiene | Public deploy scripts target devnet and commit test keypairs | Low | Hygiene note |

Label tally: CONFIRMED IN CODE 2, OVERSTATED 3, FALSE 1.

---

## Why This Report Exists

NATIX Network markets itself as a decentralized, drive to earn mapping DePIN on Solana: a network where drivers with a phone, a dashcam or the VX360 camera capture street imagery, contribute it to a decentralized map, and earn the NATIX token, with an AI camera product and partners such as Grab. The marketing repeatedly frames the reward and the network as on chain and decentralized. This report tests those claims against the ACTUAL public source code and the on chain state, and separates what is genuinely verifiable from what is a centralized product wrapped in decentralization language. No team analysis is included. Everything here is read only and drawn from public code, public pages and public Solana RPC.

## Method

1. Confirmed the token independently on Solana mainnet via public RPC getAccountInfo and getTokenSupply against mint `FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX`: program owner, decimals, mint authority, freeze authority, supply.
2. Enumerated the public GitHub orgs natixnetwork and natix-io, cloned the Solana staking repo (saphira) and read the Rust source line by line.
3. Checked whether the staking program identifier in the repo deploy scripts exists on mainnet, and looked up the hardcoded owner authority on chain.
4. Read the official token page, the token explainer blog and third party coverage to extract the reward mechanism, tokenomics, burns and traction claims.
5. Labeled each flagship claim CONFIRMED IN CODE, OVERSTATED or FALSE with a file and line reference or an on chain fact.

Transparency limit: the Drive& mobile app, the mapping and data ingestion backend, the in app reward ledger and the in app to on chain conversion service are not published anywhere public. Per MEFAI policy, unseen claims about those components are not credited. In addition, a note on completeness: public Solana RPC endpoints rate limited or blocked getTokenLargestAccounts during this audit, so top holder concentration was not independently pulled. That single distribution metric is an explicit gap; every other on chain fact above was retrieved directly.

## The Foundation: the token is real and clean

Verified on chain, the NATIX mint is a plain, well behaved classic SPL token, not Token-2022. Both dangerous authorities are gone:

- Mint authority is `null`. No party can print new NATIX. Supply is fixed at the current 99,292,928,176.772374 units.
- Freeze authority is `null`. No party can freeze or seize holder token accounts.
- Decimals 6, initialized, owned by the standard SPL Token program `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA`.

This is the strongest positive in the report. The classic rug vectors of a DePIN token, an open mint authority and an open freeze authority, are both closed. The token itself carries low direct risk. The concerns in this report are about how the NATIX network works, not about the token contract.

Reported tokenomics (from natix.network and CoinCarp) allocate 37 percent to an Incentivization Pool released based on product usage, 24.9 percent to Early Backers, 20 percent to Team and Advisors, 8 percent to liquidity, 5.1 percent Reserve and 5 percent Public Sale, on a 100 billion cap. The ICO on CoinList reportedly raised about 5M USD. These allocations are disclosed, which is a positive, but the vesting and the pool releases are administered off chain.

---

## Claim 1: Drivers earn NATIX on chain for mapping data, verifiably

**CLAIM (marketing):** the network is a decentralized mapping DePIN where contributors earn the NATIX token for the map data they provide. The framing throughout is on chain and decentralized.

**REALITY: FALSE as stated.** Mapping rewards are off chain application points computed by a centralized leaderboard, not on chain, and not verifiable on chain. Only an optional later withdrawal touches the blockchain.

**EVIDENCE:**
- The official token explainer states the earning path plainly: "The top 60% of the regional leaderboards earn in app NATIX every monthly cycle." Ranking and issuance happen in the closed Drive& backend.
- The same page describes a two currency design: in app NATIX (points) versus on chain NATIX (the SPL token), and gates conversion: "in app NATIX to on chain $NATIX withdrawal has a 30-day cooldown period or a 30% fee in case of instant withdrawal." That is a centralized ledger paying out, not an on chain reward.
- There is no public program, and no public repository, for the reward ledger, the leaderboard, the map data verification or the point issuance. The mapping backend is closed source.

**IMPACT:** the core drive to earn value loop is a centralized application that mints points and later disburses tokens at the operator's discretion. Contributors cannot independently verify on chain that mapping work produced the correct reward. This is the central overreach: the decentralization is at the token and staking layer, not at the earning layer.

## Claim 2: An on chain program governs NATIX staking and rewards

**CLAIM (marketing):** "The NATIX deep staking platform is live", tokens can be staked for rewards, and the token powers validator nodes and network governance.

**REALITY: PARTIALLY CONFIRMED, but OVERSTATED.** A real Solana staking program is public and hardcodes the NATIX mint. However it is administered by a single owner key, its rewards come from a team funded pool rather than protocol revenue, the public deploy configuration targets devnet, and the actual mainnet staking program is not source matched from the repo. There is no public on chain data marketplace program.

**EVIDENCE:**
- The saphira repo contains a native Solana staking program that binds to the exact NATIX mint: `programs/stake_v2/src/get_natix_token_mint.rs:22` returns `Pubkey::try_from("FRySi8LPkuByB7VPSCCggxpewFUeeJiwEGRKKuhwpKcX")`. This confirms the code is written for this token.
- The reward rate is a centrally set knob, not a market outcome. `programs/stake_v2/src/change_interest_rate.rs:26` documents "Only owner of program account can call this", and enforces it via `control_owner`. The owner is a single hardcoded key: `programs/stake_v2/src/get_owner_id.rs:7` returns `Ewbt8FTk39iAXuymJNdfvL6wEfU91ochNSbAdc5KWHkc`. On mainnet that address is a plain system owned wallet holding about 0.101 SOL, a normal single signer keypair, not a multisig or a governance program.
- The same owner controls pause, resume, set_config and max stakers (`pause.rs`, `resume.rs`, `set_config.rs`, `control_max_stakers.rs`), so staking can be halted or reconfigured unilaterally.
- Reward computation is continuous compounding at the owner set rate: `programs/stake_v2/src/get_reward.rs:39` and `:48` apply `p.mul(E.powf(r.mul((clamped as f64).div(365.0))))`. This uses f64 floating point in on chain financial math, a determinism and precision concern.
- The repo deploy scripts point at devnet, not mainnet: `programs/vote/constants.js:8` sets `connectionUrl = "https://api.devnet.solana.com"`, and the staking program id it references, `FqTzWJoJSAqG6fPwo4cucDtaK8MowBUH82TmxSXhLxbJ`, does not exist as an account on mainnet (getAccountInfo returns null). The live mainnet staking program therefore runs code that cannot be matched to this public source.
- The same constants.js commits raw secret keys for the payer and authority keypairs. These are devnet test keys, so the direct risk is low, but it is poor hygiene.
- No public program exists for a data marketplace, validator node rewards, or governance voting on chain. The vote tooling in saphira is a devnet program, not a live governance system.

**IMPACT:** staking is genuinely on chain and honestly bound to the right mint, which is better than pure marketing DePINs. But it is admin centralized (one key sets the yield and can pause), its rewards are subsidised from the Incentivization Pool rather than earned by the protocol, and the running mainnet program is not source verifiable. The broad claim that an on chain program governs the NATIX network is overstated.

## Claim 3: The VX360 AI camera and a decentralized, verifiable map dataset

**CLAIM (marketing):** the VX360 device unlocks Tesla 360 cameras, and NATIX builds a decentralized network for real time, high resolution map data and physical AI.

**REALITY: OVERSTATED.** A real device and real computer vision code exist, and the hardware is confirmed by independent press, but the end to end data pipeline is closed source and centralized, so the decentralized and verifiable framing is not demonstrable.

**EVIDENCE:**
- Real vision code is public. The streetvision-subnet repo (Python, MIT, over 1000 commits, active in 2026) is a Bittensor subnet that classifies road imagery, for example detecting construction sites. This is a genuine machine learning codebase. Note that it runs on Bittensor, not Solana, and it is not the NATIX reward mechanism.
- The legacy natix-io org shows years of computer vision engineering (the vsdkx video SDK family, face detection, object detection), which supports the claim that NATIX has real vision capability heritage.
- However, the VX360 firmware, the camera to network ingestion, the map building pipeline and the AI processing that turns imagery into map data are not published anywhere public. There is no way from code to verify that the dataset is decentralized, that contributions are attributed on chain, or that the pipeline is anything other than a conventional centralized data collection service that pays app points.
- The device itself is corroborated by independent coverage (Invezz, AInvest, SolanaFloor) describing VX360 built on Grab hardware to capture Tesla 360 footage.

**IMPACT:** the AI and camera story is plausible and partly backed by real, inspectable vision code, which is a positive. The decentralized and verifiable adjectives applied to the map dataset are not supported by any public code and should be treated as marketing.

## Claim 4: Real traction, drivers and partners

**CLAIM (marketing):** over 100,000 users and nearly 40 million kilometers mapped in the first year, later described as millions of registered drivers, plus partners including Grab.

**REALITY: partners CONFIRMED, traction numbers OVERSTATED (self reported and inconsistent).**

**EVIDENCE:**
- The Grab partnership (May 2025) is confirmed by multiple independent outlets, not only by NATIX. This is real and material.
- Driver and kilometer figures are self reported by NATIX and disagree across sources. NATIX marketing cites "over 100,000 users" and "nearly 40 million kilometers" in year one; a third party research writeup instead hedges to "millions of registered drivers, hundreds of thousands of mapped kilometers", which is internally inconsistent with the 40 million kilometer claim. None of these numbers are verifiable on chain or from any public dataset.
- Burns are referenced (a claim of over 8 million NATIX burned, plus periodic burn reports). The on chain supply of about 99.29 billion sitting below the 100 billion cap is consistent with some burning having occurred, but the exact burn totals are self reported.

**IMPACT:** the partnership substance is real and is the strongest external validation of the project. The usage metrics are marketing figures that an auditor cannot confirm, so they should not be relied upon as facts.

---

## Positive Findings

- The token contract is clean and safe: classic SPL, mint authority revoked, freeze authority revoked, fixed supply of about 99.29 billion. The primary rug vectors are closed.
- A real Solana staking program is public (saphira) and correctly hardcodes the NATIX mint, which is more transparency than most DePIN tokens offer.
- Genuine engineering heritage: an active, MIT licensed Bittensor vision subnet with real ML code, and years of prior computer vision repositories.
- Real, press confirmed partnerships (Grab), which are hard to fabricate.
- Disclosed tokenomics with explicit vesting schedules and an incentivization pool tied to product usage.
- Deflationary burns are documented and are consistent with supply sitting below the stated cap.

## Conclusion

NATIX Network is a legitimate project with a clean, fixed supply SPL token, real partnerships, real vision engineering and some genuinely public on chain code. It is not a scam. The problem is a consistent gap between the decentralized, on chain framing and the actual architecture. The flagship drive to earn reward is an off chain, centralized leaderboard that issues application points and later lets users convert them to tokens, gated by a cooldown and a 30 percent instant withdrawal fee; that specific claim of on chain, verifiable mapping rewards is FALSE. The on chain program story is real but narrow and admin centralized: a single owner key sets the staking yield and can pause the program, the yield is subsidised from a team pool rather than protocol revenue, and the live mainnet staking program cannot be matched to the public source, whose deploy scripts target devnet. The core value creating components, the Drive& app, the mapping backend, the reward ledger and the conversion service, are closed source and therefore unverifiable, so their claims are not credited.

Net: the investor facing token risk is low, but the decentralization claims that define the product are overstated, and the earning mechanism that the whole DePIN thesis rests on is a centralized app. Score 64 out of 100, MEDIUM risk, Passed, driven up by a clean token and real partnerships and held down by a false on chain reward claim, a centralized and unverified staking deployment, and closed core code.

Labels: CONFIRMED IN CODE 2, OVERSTATED 3, FALSE 1.
