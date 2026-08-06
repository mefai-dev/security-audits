# Alchemist AI (ALCH): Whitepaper Claims vs Code Reality

**Score: 56/100, MEDIUM RISK (Passed, with heavy transparency and centralization caveats)**

Date: 2026-08-06
Auditor: MEFAI Security, source code and on chain review (read only, public sources)

**Token (live):** Solana SPL mint `HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump` (pump.fun origin). Live and confirmed on chain across two independent RPC providers. Standard SPL Token program mint (owner `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA`, account size 82 bytes). Decimals 6. Mint authority null. Freeze authority null. On chain supply 999,953,124.835659 ALCH (raw 999953124835659). Both authorities being null is a genuine positive: no future dilution and no account freeze vector.

**Websites:** Official app at alchemistai.app; documentation at docs.alchemistai.app; legacy build at v1.alchemistai.app; X account @alchemistAIapp. A cluster of lookalike or impersonating domains exists in the wild: alchemistai.org (previously flagged as a parked lookalike), alchemistai.lat, alchemistai.cc, and alchemistaiofficial.com, all presenting as the official platform. This is a live phishing and brand impersonation surface for holders.

**GitHub:** No official public source repository was found. Searches surface only unrelated projects (a Git console manager at alchemist-org/alchemist, the Alchemy Web3 infrastructure company, academic and simulator projects). The product is a closed SaaS. Core code is closed or unverifiable; this is treated below as an explicit transparency finding, and unseen claims are not credited.

## Severity Summary

| Finding | Verdict | Severity |
|---|---|---|
| SPL mint live, both authorities null, fixed supply | Confirmed on chain | Positive |
| 200 ALCH per app generation | Confirmed (site and docs) | Info |
| Hard capped supply near 1B | Confirmed on chain | Positive |
| Real documented product plus marketplace UI | Confirmed (docs render) | Info |
| On chain program governing ALCH credits | False (none exists) | High |
| Decentralized store / marketplace framing | False / Overstated (centralized SaaS) | Medium |
| Blockchain native positioning of spend and market | Overstated (off chain accounting) | Medium |
| Traction: over 100,000 application builds | Overstated / Unverifiable (self reported) | Medium |
| Tokenomics vesting distribution 85/5/7/3 | Overstated / Unverified off chain | Medium |
| Azarus security scanner assigns 0 to 100 ratings | Overstated / Unverifiable | Low |
| Core product code closed / unverifiable | Transparency finding | High |
| Lookalike domain cluster (.org, .lat, .cc, official.com) | Phishing surface | Medium |

## Why This Report Exists

Alchemist AI markets itself as an AI powered no code builder on Solana that turns natural language prompts into working applications, with a native token (ALCH) spent as credits (about 200 ALCH per app), a creation environment (variously branded the AI Sacred Laboratory or Sacred Compute), and a marketplace (Arcane Forge) where apps are bought, sold, and tipped. Exchange listings (Coinbase, Kraken, Bybit, Bitget), CoinGecko coverage, and roughly 27.9M USD market capitalization mean real money is exposed to these claims. The purpose here is to separate what is verifiable in code and on chain from what is only asserted on marketing surfaces, and to state plainly where the crypto substance is thinner than the branding implies.

## Method

On chain: direct Solana JSON RPC calls (getAccountInfo, getTokenSupply) against the mainnet cluster, cross checked on a second independent public RPC (publicnode) for the mint state. The mint account was parsed for owner program, decimals, mint authority, freeze authority, and supply. An attempt to enumerate largest holders (getTokenLargestAccounts) was rate limited across every free public endpoint tried (mainnet-beta, Ankr, publicnode, dRPC, rpcpool) and Solscan is Cloudflare gated to automated fetches, so holder concentration is reported as not independently retrieved rather than guessed.

Off chain: the official documentation was read page by page in raw markdown form (introduction.md, get-started, ai-laboratory.md and its features.md, marketplace.md, azarus-ai-agent.md, alchemistry-101.md, token-usdalch.md), plus the official app copy, CoinGecko market data, and independent write ups (Solana Compass, Gate Learn, Millionero, CoinMarketCap AI, Bybit Learn). Every claim below is labeled CONFIRMED, OVERSTATED, or FALSE against either a documentation quote, an app quote, or an on chain fact. Because there is no public source repository, product side claims cannot be checked against implementation code; they are graded on documentation self consistency and on chain footprint only.

## The Foundation: A Plain pump.fun SPL Token Wrapped in a Closed SaaS

The entire on chain footprint of Alchemist AI is a single classic SPL token. The mint `HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump` carries the `pump` suffix of a pump.fun launch, is owned by the standard SPL Token program (not Token 2022, no transfer hooks, no extensions), occupies the standard 82 byte mint layout, has 6 decimals, and reports both mint authority and freeze authority as null on two independent RPCs. Supply is fixed at 999,953,124.84 tokens, consistent with the stated one billion hard cap (the small shortfall is normal for pump.fun accounting).

There is no custom Solana program in the picture. The documentation never cites a program ID, an anchor IDL, a PDA, or any instruction that the app builder or marketplace would invoke. Everything that the marketing calls an on chain economy resolves, in the documentation itself, to a centralized web application that reads a wallet ALCH balance and settles credits in its own database. That is the foundation the claims below sit on.

## Claim 1: An on chain program governs ALCH credits (about 200 ALCH per app)

CLAIM: Marketing and third party explainers present ALCH as a blockchain native credit consumed by the platform, spending it to generate applications, access premium AI models, and tip creators, on Solana chosen because users may interact with the blockchain dozens of times per session.

REALITY: FALSE as stated. No on chain program governs ALCH credits. ALCH is a plain SPL token; credit accounting is off chain inside a centralized SaaS. What is confirmed is the price point, not the mechanism: the official app states, quote, "Generating your app will cost 200 $ALCH tokens." The docs describe a wallet ALCH balance gating a paid mode, quote, "switch between normal mode (unpaid, quick) and paid mode (paid, slower but far more intelligent)". Nowhere do the docs describe a per action Solana transaction, a spend program, or a burn instruction.

EVIDENCE: On chain, the mint owner is `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (the stock SPL Token program) with an 82 byte account and no extensions; there is no associated custom program anywhere in the token metadata or docs. In docs/get-started/ai-laboratory/features.md the paid tier is a "Premium Model" offering "faster processing, exclusive model enhancements, and additional resources," which is SaaS metering language, not on chain settlement. docs/token-usdalch.md lists tokenomics but specifies no program, no burn, and no on chain credit logic.

IMPACT: High for the crypto thesis. The token functions as a prepaid voucher whose spend, balance, and consumption are all trust me numbers held by the operator. Holders get none of the auditability that an on chain credit program would provide, and the Solana dozens of interactions per session framing is not borne out by any observable on chain activity.

## Claim 2: Arcane Forge is a decentralized marketplace

CLAIM: Third party coverage describes Arcane Forge as a decentralized store where the community shares and monetizes creations, with all transactions powered by ALCH.

REALITY: FALSE / OVERSTATED. The official documentation describes a centralized in app listing and checkout flow, not a decentralized protocol. Quote from docs/get-started/marketplace.md: a creator clicks the shop icon after generating an app, then must "Fill in the Name, Description & Price of your application and click on Add," and buyers "Use `$ALCH` to purchase apps instantly or tip your fellow creators." Purchasing instantly inside the app, with listings stored as platform records, is centralized SaaS commerce. No marketplace smart contract, escrow program, or on chain order book is referenced anywhere.

EVIDENCE: docs/get-started/marketplace.md (listing via shop icon, name/description/price form, instant purchase and tipping); absence of any program ID or on chain settlement in the entire docs tree; on chain footprint limited to the SPL mint.

IMPACT: Medium. Users may assume trustless, on chain ownership and settlement of apps and tips. In reality the operator mediates every listing, sale, and tip off chain and could alter, delist, or reverse them.

## Claim 3: Sacred Compute / the AI Sacred Laboratory is an open, verifiable builder

CLAIM: The platform turns natural language into functional apps through a creation surface branded as the AI Sacred Laboratory (marketed elsewhere as Sacred Compute), including a multi AI system, image generation, multiplayer, and third party API support.

REALITY: OVERSTATED and unverifiable; core code closed. The builder is a closed SaaS with no published source, so none of its behavior can be independently confirmed. The docs describe a UI and a feature list but disclose no architecture, no models, and no export path. The exact phrase Sacred Compute does not appear in the official docs at all; the official name is the AI Laboratory (with a Magic Orb and a Wizard Bot editing flow). Feature claims read as a product brochure, quote from features.md: a "Multi-AI System" with "Dedicated models handling specific tasks," "Third-Party API Support" for "16+ external APIs," "AI-Within-AI," "Multiplayer Games," and "Forge Rewind" version control. Whether these work as described cannot be tested from outside.

EVIDENCE: docs/get-started/ai-laboratory.md and docs/get-started/ai-laboratory/features.md (UI and feature narrative, no architecture, no code export, no compute engine detail); no GitHub or open source disclosure in any doc; introduction.md is pure lore ("turn spoken desires into tangible reality") rather than technical specification.

IMPACT: High for transparency. The flagship value proposition rests entirely on unseen code. Per audit policy, these claims are not credited. Notably, docs/get-started/azarus-ai-agent.md says the platform even scans user app codebases and assigns a 0 to 100 security rating; an unverifiable security scanner grading unverifiable generated code is a compounding transparency gap, not an assurance.

## Claim 4: Traction (over 100,000 application builds) and published tokenomics

CLAIM: Independent coverage repeats over 100,000 application builds since launch, and docs/token-usdalch.md publishes a distribution of Liquidity Pool 85 percent (no vesting), Marketing 5 percent (3 month linear unlock), Treasury and Ecosystem 7 percent (12 month linear unlock), and Team 3 percent (1 month cliff, 6 month linear unlock).

REALITY: OVERSTATED / unverifiable. Because credits and app creation are off chain, there is no on chain footprint to corroborate the 100,000 builds figure; it is a self reported operator metric. The vesting distribution is an off chain assertion; the only piece confirmable on chain is that mint authority is revoked, so no new supply can be minted. Vesting schedules and wallet allocations were not independently traced (holder enumeration was rate limited on every free RPC). What is solid is market presence, not usage: CoinGecko shows price about 0.02788 USD, market capitalization about 27.9M USD, fully diluted valuation about 27.9M USD (supply is effectively fully circulating), 24 hour volume about 2.6M USD, all time high 0.2433 USD on 2025-12-17 (down roughly 88 percent), all time low 0.01436 USD on 2025-02-24.

EVIDENCE: docs/token-usdalch.md (distribution and one billion supply); on chain mint authority null (dilution disabled); CoinGecko market data; Solana Compass and Gate Learn for the 100,000 builds and product narrative. Down about 88 percent from ATH indicates a token well past its speculative peak.

IMPACT: Medium. Investors should treat usage and vesting numbers as operator claims, not verified facts, and weight the position on the confirmed items (safe token authorities, real liquidity and listings) rather than the unconfirmed ones.

## Positive Findings

1. Token authorities both null. On chain, mint authority and freeze authority are both null, verified on two independent RPCs. No dilution risk and no freeze/blacklist vector. This is a real, material positive.
2. Fixed supply, standard SPL. Classic SPL Token program, 82 byte mint, 6 decimals, roughly one billion fixed supply. No Token 2022 transfer hooks or hidden extensions that could tax or trap transfers.
3. Genuine market and liquidity. Roughly 27.9M USD market cap and 2.6M USD daily volume with tier one exchange listings (Coinbase, Kraken, Bybit, Bitget) and CoinGecko coverage; this is not a zero liquidity ghost token.
4. A real, documented product. The app, a coherent documentation set, and an active X presence exist; this is an operating SaaS, not a pure vaporware shell.
5. Transparent pricing surface. The 200 ALCH per app cost is stated plainly on the product and a tokenomics table is published, which is more disclosure than many pump.fun era tokens offer.

## Conclusion

Alchemist AI is a centralized, closed source SaaS with a clean but plain SPL token bolted on. The token layer is the strongest part of the story: the mint is live, standard, hard capped, and has both authorities revoked, verified on chain twice, so the classic memecoin rug vectors (mint dilution, freeze, transfer hooks) are absent, and there is genuine exchange liquidity. That is why this review lands on the passing side.

The crypto substance, however, is thinner than the branding. There is no on chain program governing ALCH; credits, the marketplace, and tips are off chain operator ledgers, so the decentralized and blockchain native framing is false to overstated. The flagship builder and the Azarus security scanner are unverifiable closed code and are not credited. Traction and vesting figures are self reported. A cluster of lookalike domains adds a live phishing risk. Net, ALCH is best understood as a tradeable prepaid credit for a private AI app builder, safe at the token level but centralized, unaudited, and unverifiable at the product level, and priced about 88 percent below its December 2025 peak.

Score: 56/100. Passed on token safety and real market presence; flagged findings on transparency, centralization, and off chain accounting keep the risk rating at MEDIUM. Verdict scale: 51 or above Passed, 50 or below Flagged.
