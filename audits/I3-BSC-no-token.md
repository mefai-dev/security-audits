# Intelligence Cubed: Whitepaper Claims vs Code Reality

**Score: 32/100, HIGH RISK**

**Date:** 2026-08-06

**Token:** No token found. No Intelligence Cubed / I3 token exists on CoinGecko, DexScreener, or GeckoTerminal, and no contract address appears anywhere on the project's site, app, or litepaper. A separate BSC token "i3D" (symbol I3D, `0x40d451de379b92ce43c65afb827f8a9debca0062`) is an unrelated dead 2024 token and is not this project (ruled out on chain below). The "I3 token" and "I3 credits" that the product references are off chain points stored in a Firebase Firestore database.

**Websites:** intelligencecubed.io (marketing site, HTTP 200, live), intelligence3.io (the "AI Terminal" demo app, HTTP 200, live), intelligencecubed.com (redirects to the GitBook litepaper), litepaper at intelligence-cubed.gitbook.io/intelligence-cubed/i-cubed-litepaper (live). Socials referenced: X handle @I3_Cubed, Telegram t.me/I3_Cubed. Academic litepaper reference: preprints.org/manuscript/202506.1717/v1 (June 2025).

**GitHub:** None found. A GitHub repository search for "Intelligence Cubed", "i3", "intelligencecubed", and "intelligence3" returned zero repositories. The core is closed source. The demo app ships as a single self contained bundle hosted on a Firebase web.app project.

## Severity Summary

| ID | Finding | Severity | Basis |
|----|---------|----------|-------|
| ICE 001 | No on chain token exists despite token centric marketing and "connect wallet to earn I3 tokens" prompts | Critical | CoinGecko, DexScreener, GeckoTerminal all negative; zero 0x addresses in any asset |
| ICE 002 | Core is closed source; no public code repository | High | GitHub search returns 0 repos; app is a compiled bundle |
| ICE 003 | "On chain" utility is not on chain; credits and check ins live in a Firestore database, WalletConnect projectId is the placeholder `PROJECT_ID` | High | Firestore references and placeholder config in app bundle |
| ICE 004 | Flagship "on chain AI model marketplace / IMO" is conceptual only; the live app is an AI chat "Terminal" | High | Litepaper names no chain, no contracts; app tabs are Chats / Modelverse / Benchmark |
| ICE 005 | Traction claims (1.5M+ MAU, 100K+ DAU) are unverifiable and implausible for a scaffolded Firebase demo | Medium | No public analytics; app is a prototype |
| ICE 006 | Awards and partnership claims (DappBay #1, ETHDenver, Solana x402, CMU) unverifiable from public source or chain | Medium | Marketing assertions with no on chain or code evidence |

## Why This Report Exists

A prior thin listing scored Intelligence Cubed very low (20) on little more than a glance. The project markets itself aggressively as "the world's 1st AI Model Nasdaq" on BNB Chain, claims millions of users, and invites visitors to "connect wallet to earn I3 tokens." That framing implies a live BNB Chain token, on chain utility, and a working decentralized marketplace. This report independently tests each of those implications against the actual public source, the live sites, and BSC on chain state, so the score rests on evidence rather than a headline.

## Method

Everything here is public and read only. I fetched the marketing site (intelligencecubed.io), the live demo app (intelligence3.io, the "AI Terminal"), and the GitBook litepaper, then stripped them to text and searched for token, contract, chain, and wallet wiring. I searched for the token independently on CoinGecko (`/search`), DexScreener (`/dex/search`), and GeckoTerminal (`/search/pools`) by both "Intelligence Cubed" and "I3". I searched GitHub for any public repository. I confirmed the one near name BSC candidate ("I3D") directly on chain via a public BSC RPC (`bsc-dataseed.binance.org`) using `eth_getCode` and `eth_call` for `name`, `symbol`, `decimals`, and `totalSupply`. No private endpoints, no authentication, no team analysis.

## The Foundation: A Concept Paper and a Chat Demo, No Chain

The litepaper (GitBook, roughly 30,000 characters of prose) describes an ambitious "d/acc driven open source intelligence economy": an AI model marketplace where models become tradeable assets through an "IMO" (Intelligence Model Offering), a staking phase to reserve ownership shares, an "Open-Source at 51%" milestone, recursive royalty distribution, and a four layer stack topped by a "Control Layer (On Chain): Smart contracts for ownership, royalties, and governance," with a DePIN execution layer.

Two facts about that document matter. First, it never names a blockchain. It mentions "on chain" ten times, "smart contract" three times, and "layer 2" once, but there is no BNB, no Binance, no Ethereum, no Solana, and no chain id anywhere in the text. The fifteen apparent "BSC" hits in the raw HTML are a false positive: they are the letters b s c inside the word "su**bsc**ription." Second, it defines no token: no ticker, no supply, no tokenomics table, and no contract address. The token is a conceptual "value share" you "earn back to the ecosystem," not a deployed asset.

The live product at intelligence3.io is titled "Intelligence Cubed - AI Terminal." It is a JavaScript single page app (359KB of HTML, about 1.2KB of visible text before scripts run) hosted on a Firebase web.app project and importing libraries from esm.sh, jsdelivr, and unpkg. Its tabs are Chats, Modelverse, Benchmark, Workflows, and Canvas; the main pane says "Select a model to start chatting." Its backend is Firebase Firestore (fourteen Firestore references in the bundle). This is a real, live, interactive demo, but it is an AI chat interface with a gamified points system, not the on chain model exchange the litepaper describes.

## Claim 1: "Connect wallet to earn I3 tokens" implies a live BNB Chain token

**CLAIM**
> "Log in and connect wallet to earn I3 tokens" with buttons for "Binance Wallet, MetaMask, WalletConnect, Coinbase Wallet" (intelligence3.io app). The marketing site frames the project as a BNB Chain (BSC) project.

**REALITY: FALSE.** No Intelligence Cubed / I3 token exists on chain. CoinGecko search returns nothing for "Intelligence Cubed" and nothing relevant for "I3" (nearest results are Api3, Autonomys AI3, Charli3, all unrelated). DexScreener search for "Intelligence Cubed" and "I3 Cubed" returns no matching token. GeckoTerminal search for "intelligence cubed" is empty. There is no 0x contract address anywhere in the marketing site, the app bundle, or the litepaper. The "I3 tokens" and "I3 credits" the app hands out are off chain points recorded in Firestore. The wallet buttons exist, but the WalletConnect `projectId` in the shipped bundle is the literal placeholder string `PROJECT_ID`, so even the wallet connect path is unconfigured scaffolding.

**EVIDENCE:** CoinGecko `/search?query=Intelligence%20Cubed` returns empty coin array; `/search?query=I3` returns only unrelated tokens. DexScreener and GeckoTerminal searches return no match. App bundle contains `projectId: 'PROJECT_ID'` and fourteen Firestore references; zero `0x` forty character addresses across all three assets.

**IMPACT:** The central financial premise, a BNB Chain I3 token, does not exist. Anyone connecting a wallet to "earn I3 tokens" is farming database points against an unbuilt token, on placeholder infrastructure. There is no traded asset to value, and any future token would inherit the inflated framing described below.

## Claim 2: The near name BSC token "I3D" might be this project

**CLAIM**
> The only BNB Chain token whose ticker resembles "I3" is I3D, surfaced by a GeckoTerminal search for "I3" on bsc.

**REALITY: FALSE (ruled out on chain).** The I3D / WBNB pool holds about $0.62 of reserves and was created on 2024-07-06. I queried the base token directly on BSC: name "i3D", symbol "I3D", decimals 8, total supply 220388524499999999 raw (about 2.2 billion at 8 decimals), 3,005 bytes of bytecode. It is an abandoned 2024 token with dust liquidity and no connection to intelligencecubed.io, whose product and litepaper are from 2025 and reference a Firestore points system rather than this contract. It is not the project.

**EVIDENCE:** BSC RPC `bsc-dataseed.binance.org`, `eth_call` on `0x40d451de379b92ce43c65afb827f8a9debca0062`: name = "i3D", symbol = "I3D", decimals = 8, totalSupply = 220388524499999999. GeckoTerminal pool `0xbbfeef201a6cc8276f0c282127aa168014084453` reserve_in_usd = 0.625, created 2024-07-06.

**IMPACT:** Confirms the negative result: there is genuinely no live token for this project on BSC, not even a near miss.

## Claim 3: On chain utility, "daily on chain check in", staking, and royalty smart contracts

**CLAIM**
> "Check-In on Chain: Complete your daily on chain check in to earn I3 credits." The litepaper adds a "Staking Phase" and "Smart contracts for ownership (IMO), royalties, and governance."

**REALITY: FALSE / OVERSTATED.** There is no deployed contract to check in to, stake in, or pay royalties from. The credits counter, streak counter, and "Total Check ins" all read from and write to Firestore, an off chain Google database. The wallet layer that would make anything "on chain" is unconfigured (placeholder `projectId`). The staking, IMO, and royalty mechanisms exist only as prose and diagrams in the litepaper, with no chain named and no contract address, verified or otherwise, to point to.

**EVIDENCE:** App bundle: "Complete your daily on chain check in to earn I3 credits", "Current Streak 0 days", "Total Check ins", backed by Firestore; no contract address; placeholder WalletConnect config. Litepaper: staking and royalty language present, but zero chain identifiers and zero deployed contracts.

**IMPACT:** The word "on chain" is doing marketing work the code does not support. On chain utility is effectively zero. Users are led to believe daily activity accrues an on chain asset when it accrues a database number.

## Claim 4: "The world's 1st AI Model Nasdaq" with an on chain model marketplace

**CLAIM**
> "AI × EI × Decentralized intelligence"; models can be "uploaded, traded, and invested in as on chain assets," with continuous royalties, forking, and revenue splits, the "world's 1st AI Model Nasdaq."

**REALITY: OVERSTATED.** A live app exists, but it is an AI chat "Terminal" with a "Modelverse" and a "Benchmark" tab, not a functioning exchange where models are tokenized and traded on chain. There is no on chain listing, no IMO transaction, no order book, and no settlement, because there are no contracts and no chain. The marketplace, the tokenized model assets, and the recursive royalty engine are all forward looking design, not shipped functionality. What is shipped is a model chat and a gamified points loop.

**EVIDENCE:** App tabs (Chats, Modelverse, Benchmark, Workflows, Canvas) and "Select a model to start chatting"; Firestore backend; no marketplace contract; litepaper describes IMO and royalties as design.

**IMPACT:** The flagship value proposition is unbuilt. Crediting "an AI model Nasdaq" would be crediting a slide, not a system.

## Claim 5: 1.5M+ monthly active users and 100K+ daily active users

**CLAIM**
> "1.5M+ MAU" and "100K+ DAU" (marketing site, attributed to a BNB Chain showcase).

**REALITY: OVERSTATED / unverifiable.** These figures cannot be confirmed from any public source, and they are implausible for a Firebase hosted prototype whose wallet layer is unconfigured and whose core loop is a chat plus daily check in. Even taken from DappBay style dapp analytics, such counts are trivially inflatable by bots and wallet farming and carry no independent audit. Nothing in the code or on chain corroborates them.

**EVIDENCE:** No public analytics endpoint; app is a scaffolded single page bundle; no on chain transaction volume exists to cross check (no contracts).

**IMPACT:** Headline traction should not be credited. It is a marketing number with no verifiable substrate.

## Claim 6: Awards and partnership (DappBay #1 in AI, ETHDenver, Solana x402, CMU)

**CLAIM**
> "#1 in the AI category on Binance DappBay", "ETHDenver Pitch Contest winner", "1st place, Solana x402 Hackathon", "Partnership with CMU Modelverse Dev Initiative."

**REALITY: OVERSTATED / unverifiable.** A direct DappBay dapp page for the project returned 404 at the obvious slug, and none of these claims is verifiable from the public source or from chain. The BNB Chain ecosystem may well have showcased the project (the site cites @BNBCHAIN), and some of these may be true, but none can be confirmed here, and none reflects on chain substance or shipped code. They are reputational assertions, credited to zero.

**EVIDENCE:** DappBay dapp URL 404; no on chain or code artifact tied to any award; claims appear only in marketing copy.

**IMPACT:** These do not raise the technical or on chain score. At most they signal marketing effort.

## Positive Findings

The project is not a dead link or an empty shell, and that is worth stating plainly.

- Both public sites are live (HTTP 200). intelligencecubed.io is a coherent marketing site; intelligence3.io is a genuinely interactive demo.
- The "AI Terminal" is a real, working single page application with a model chat, a Modelverse browser, a benchmark view, and a daily check in loop, backed by a real Firebase Firestore database. It is more product than a typical vaporware site.
- There is a substantive academic style litepaper (GitBook, with a preprints.org reference dated June 2025) laying out a thought through design. The concept is articulate.
- The project is recently and visibly active (2025 paper, x402 hackathon framing, ongoing socials), not abandoned.
- Because no token is deployed, there is currently no honeypot, no mint function, no transfer tax, and no upgradeable proxy to exploit. There is no token rug surface today, precisely because there is no token.

## Conclusion

Intelligence Cubed is a live, actively promoted project with a real academic concept and a working AI chat demo, wrapped in marketing that implies far more than the code and the chain deliver. Tested against evidence: the "I3 token" does not exist on chain anywhere (CoinGecko, DexScreener, GeckoTerminal, and a direct BSC RPC check are all negative, and the near name I3D is an unrelated dead 2024 token); the "on chain" check in and credits are Firestore database points behind a WalletConnect layer whose project id is still the placeholder `PROJECT_ID`; the flagship "AI Model Nasdaq" marketplace with tokenized, tradeable, royalty bearing models is entirely conceptual; and the millions of users, DappBay ranking, and awards are unverifiable and uncorroborated by any code or chain fact. The core is closed source with no public repository.

The result is a project whose live footprint (a chat demo and a paper) is real but whose on chain and token claims are not. There is no on chain utility to credit, no token to value, and no marketplace to inspect. Because the marketing actively invites wallet connection to "earn I3 tokens" against unbuilt, placeholder infrastructure and inflated traction, the forward risk to users is high, even though there is no tradeable asset to lose today. Weighing a live but scaffolded product and an articulate paper against zero on chain substance, a nonexistent token, closed source, and overstated traction, the whole project scores **32/100, HIGH RISK, Flagged**. That is above a bare "nonexistent project" floor because a live app and paper do exist, and well below a passing grade because the on chain, token, and marketplace core, the entire reason to care, is unbuilt or unverifiable.

Users should treat any "connect wallet to earn I3 tokens" prompt and any future I3 airdrop or token sale with strong caution, verify a real, audited contract address on chain before interacting, and discount the traction and award claims until independently proven.
