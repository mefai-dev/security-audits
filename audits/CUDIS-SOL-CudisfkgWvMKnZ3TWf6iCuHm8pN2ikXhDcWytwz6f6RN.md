# CUDIS (CUDIS): Whitepaper Claims vs Code Reality

**Score: 35/100, RISK: MEDIUM**

Date: 2026-08-05
Auditor: MEFAI Security (source code and on chain review, read only, public sources)

**Token (live):** CUDIS. Solana SPL mint `CudisfkgWvMKnZ3TWf6iCuHm8pN2ikXhDcWytwz6f6RN` (decimals 9, on chain supply 499,999,102.27, mint authority null, freeze authority null, owner is the standard SPL Token program, not `Token-2022`, not a pump.fun launch). Second deployment on BNB Chain `0xc1353d3ee02fdbd4f65f92eee543cfd709049cb1` (`BEP-20`, 18 decimals, `Ownable`, owner `0x29b4350d0f40e572ad03bdfb0825bb377175dcd3`). Price approximately $0.001218, market cap approximately $301k, FDV approximately $1.22M, 24h volume approximately $444k across 24 venues (per CoinGecko), circulating approximately 247.5M. All time high $0.2699 on November 4, 2025, now down approximately 99.5%. All time low $0.001122 on August 3, 2026.

**Websites:** cudis.xyz, app at cudis.xyz, Linktree linktr.ee/cudis, X @CudisWellness
**GitHub:** github.com/CudisWellness (organization, 5 repositories)

---

## Severity Summary

| Area / Claim | Verdict | Severity |
|---|---|---|
| "Open source" ecosystem on GitHub | FALSE. Every repository is a single README stub, no code | HIGH |
| Health reward pipeline is open and on chain | FALSE. Off chain app accounting, no reward program on chain | HIGH |
| Health data stored on chain and user owned on chain | OVERSTATED. Data lives off chain (app plus IPFS), optional NFT record | MEDIUM |
| On chain governance, DAO and staking (10 to 15% APY) | OVERSTATED. No discoverable on chain program, closed source | MEDIUM |
| AI health agent | Core closed or unverifiable. App feature, no public code | MEDIUM |
| Token integrity and 1B hard cap | OVERSTATED. Two mints combined approximately 1.088B exceed the 1B cap; BSC side is centrally `Ownable` | MEDIUM |
| Solana token authorities (mint and freeze) | CONFIRMED. Both null. No rug mint, no freeze on the Solana mint | POSITIVE |
| Product traction (rings, users, funding, listings) | CONFIRMED via external evidence, not code | POSITIVE |
| Core system transparency | Core code closed or unverifiable. Do not credit unseen claims | HIGH |

Counts: CONFIRMED 2, OVERSTATED 4, FALSE 2, plus one explicit closed source transparency finding.

---

## Why This Report Exists

CUDIS markets itself as a decentralized physical infrastructure (DePIN) health and wellness network on Solana: a smart ring that measures biometrics, an ecosystem that pays users in CUDIS tokens for their health data and healthy behavior, a health data marketplace, an on chain reputation layer, an AI health coach, and community governance. That framing implies verifiable, open, on chain machinery. This audit tests that framing against the only two things that cannot be edited in a press release: the public source code and the on chain state. Where a claim cannot be traced to code or to a chain fact, we say so and refuse to credit it.

## Method

1. Independently confirmed the CUDIS SPL mint against a Solana RPC (`getAccountInfo`, `getTokenSupply`, `getSignaturesForAddress`, and pool data from GeckoTerminal). Confirmed identity by cross referencing the mint on CoinGecko and by live DEX pools named CUDIS quoting that exact mint.
2. Cloned and read every repository in the `CudisWellness` GitHub organization at full depth, inspected commit history and the actual file tree of each repository, not just the rendered landing pages.
3. Verified the second deployment on BNB Chain by direct `eth_call` (`totalSupply`, `decimals`, `symbol`, `name`, `owner`, `eth_getCode`).
4. Cross checked marketing and tokenomics claims against The Block, Chainwire, Solana Compass, Morningstar Ventures, Phemex and CoinGecko.

Scope note: this is a code and chain transparency audit. No team, identity, or personnel analysis. Read only.

## The Foundation: A Real Company Behind a Closed System and an Empty Repository

CUDIS is not vaporware. It is a funded hardware company: a $5M seed round led by Draper Associates (September 2024), a shipping smart ring (models 001, 002 and a Pioneer package sold through multiple retailers), roughly 20,000 rings sold and roughly 200,000 users reported, a token listed on many centralized exchanges, and a World App integration for proof of personhood. That part is real and is the strongest thing here.

The problem is the gap between that real consumer product and the decentralized, open, on chain narrative wrapped around it. Under audit, the machinery that would make CUDIS a DePIN protocol rather than a normal health app with a token attached is not present in any public code, and is not present as any discoverable on chain program. The most important single finding of this report is what the `CudisWellness` GitHub organization actually contains.

Every one of the five repositories consists of exactly one file, a `README.md`, with only an initial commit and a single later README edit (dated May 2025) and no source code of any kind:

- `cudis-health-index-oracle`: README only, no source code. Describes an oracle that scores biometric data and "pushes it on chain" with "Support for Solana (via Anchor)". There is no Anchor program, no scoring engine, no publisher, no test suite. Only prose.
- `cudis-api-mock-server`: README only, no source code. Describes a FastAPI mock server. No Python, no FastAPI app, no endpoints. A mock of an API whose real implementation is nowhere public.
- `Wellness-Data-Visualizer`: README only, no source code. Describes a Next.js dashboard and references `/pages/dashboard.js`, `/components/Chart.tsx`, a `LICENSE` and a `CODE_OF_CONDUCT.md`. None of those files exist in the repository.
- `cudis-missions.vercel.app`: README only, no source code. Describes a gamified missions app with MongoDB and NextAuth. No application code.
- `.github`: organization profile README, which states "For detailed documentation and system architecture, refer to the `/docs` folder." There is no `/docs` folder anywhere in the organization.

Two tells confirm these are aspirational placeholders, not real projects. First, the setup instructions in each README still read `git clone https://github.com/your-org/...`, the untouched template placeholder, never replaced with `CudisWellness`. Second, all substance is future tense capability description, never implementation. The organization profile even lists what the repositories "may serve" as purposes, then delivers none of them.

Conclusion of the foundation review: the CUDIS core (the ring firmware, the data ingestion API, the reward and points engine, the AI coach, any staking, governance, marketplace, or oracle logic) is closed source and unverifiable from public code. Per audit policy, unseen claims are not credited, and this closed core caps the score.

---

## Claim 1: "Open Source Initiative" and an open developer ecosystem

**CLAIM.** The organization profile is titled "CUDIS Wellness . Open Source Initiative" and states the space "enables developers, researchers, and innovators to build complementary tools, integrate with blockchain protocols, and contribute." Repositories advertise an oracle, an SDK, dashboards, smart contracts and a missions engine.

**REALITY: FALSE.**

**EVIDENCE.** All five repositories in `github.com/CudisWellness` contain only a `README.md` and no functional code. Each repository has only an initial commit plus a single README edit and no source code. The oracle repository README promises an Anchor program and a scoring engine that do not exist; the mock server README promises a FastAPI service that does not exist; the visualizer README references source files (`/pages/dashboard.js`, `/components/Chart.tsx`, `LICENSE`) that are absent; the profile README points to a `/docs` folder that does not exist. Clone instructions retain the placeholder `github.com/your-org/...`. This is documentation of an intent to be open source, not an open source codebase.

**IMPACT.** The single most load bearing transparency claim is not true. A reader who trusts the "Open Source Initiative" label would reasonably believe the reward logic, oracle and contracts are auditable. They are not published at all.

## Claim 2: The health data and reward pipeline is open and on chain (Move to Earn, Sleep to Earn)

**CLAIM.** Users earn CUDIS for healthy behavior and for their data; the oracle "transforms raw biometric data ... into a verifiable Health Index Score and pushes it on chain," enabling "Sleep-to-Earn" and "Move-to-Earn" smart contracts and "on chain reputation."

**REALITY: FALSE (as stated). It is centralized off chain accounting.**

**EVIDENCE.** No reward, oracle, or reputation program is published in code, and none is discoverable as a deployed program tied to the token. The Solana mint (`CudisfkgWvMKnZ3TWf6iCuHm8pN2ikXhDcWytwz6f6RN`) is a plain SPL token: `getAccountInfo` returns `type: mint`, owner the standard SPL Token program, with no associated custom program. Airdrops and "seasons" are distributed by the company (Chainwire: "50,000,000 $CUDIS to be distributed" in Season 1, gated by ring ownership and partner onboarding). This is an app that computes points and rewards on private servers and periodically distributes tokens, not an on chain reward protocol. The oracle that would put scores on chain exists only as a README.

**IMPACT.** The earn mechanism is a trust me ledger. Points, scores and eligibility are mutable off chain with no public contract enforcing them, which is the opposite of the "verifiable" and "on chain" language used.

## Claim 3: Health data is stored on chain and owned by the user on chain

**CLAIM.** Marketing states "All user health data is securely stored on the Solana blockchain, with the user having complete control," and that users "self-custody their onchain health data."

**REALITY: OVERSTATED.**

**EVIDENCE.** The more technical launch coverage contradicts the blanket storage claim: Chainwire describes Longevity Decentralized IDs that "enable them to mint health records as NFTs," with the health data itself stored off chain, and other write ups describe encryption and IPFS storage. Biometric streams (described elsewhere as "billions of biometric signals") are not written to Solana; that volume of raw data on chain is neither present nor economically plausible. What can exist on chain is an optional NFT pointer or identity record. So user owned identity primitives may exist, but the sweeping "all health data stored on the blockchain" statement is not accurate.

**IMPACT.** Users may believe their raw biometrics are decentralized and self custodied. In practice the sensitive data sits in company infrastructure and off chain storage, governed by the same closed backend that has no public code.

## Claim 4: On chain governance, a DAO, a treasury, and staking rewards

**CLAIM.** The token "serves as a governance layer," holders "vote proportionally on protocol decisions," tokenomics allocate "10% treasury," and Solana Compass repeats "Estimated 10 to 15% APY" staking for "network security participation."

**REALITY: OVERSTATED. Core closed or unverifiable.**

**EVIDENCE.** No governance program, staking program, or treasury program is published or discoverable on chain. There is no CUDIS validator set to secure, so "staking for network security" does not map to any real Solana mechanism; any yield would be an emissions program run by the company, and no such program code is public. The only wallet surfaced by the project itself is a donation address in the profile README (`2AQLTnNDknGtCuN5N4yxfR8CdPDRk6QCXdfkNJQqV6Hz`), an ordinary account, not a governance or treasury program. Free tier RPC endpoints rate limited or paywalled the holder enumeration call (`getTokenLargestAccounts`), so on chain holder concentration could not be independently measured for this report and is stated as unknown rather than guessed.

**IMPACT.** Governance and staking are marketing constructs at audit time. There is no on chain DAO to inspect and no staking contract to verify; any promised APY is an unverifiable off chain promise.

## Claim 5: An AI health agent or coach delivering personalized insights

**CLAIM.** An "AI-powered health coach" that has "delivered over 1 million personalized AI insights," with an implied stack of "OpenAI API, LangChain, vector embeddings."

**REALITY: Core closed or unverifiable.**

**EVIDENCE.** No AI module, prompt, model, or agent code is published in any repository. The capability is described only in prose in the `.github` profile README and in marketing. Nothing about it is on chain. It is, by every available signal, a conventional server side application feature (most likely a third party large language model wrapped around the user's metrics), which is a normal product feature but is neither decentralized nor verifiable, and is not evidence of any protocol.

**IMPACT.** The AI agent is plausible as a shipped app feature but contributes nothing to the decentralization or on chain claims and cannot be audited. It is credited as neither confirmed nor denied, only as closed.

## Claim 6: A 1 billion capped, utility bearing token

**CLAIM.** "Total supply capped at 1 billion tokens," "Max Supply 1B" (CoinGecko), with utility for rewards, payments in the data marketplace, and governance.

**REALITY: OVERSTATED on supply integrity; token is real and tradeable; Solana authorities are safe.**

**EVIDENCE.** On chain, the picture is split and does not reconcile to a single enforced cap:
- Solana mint `Cudisfkg...6RN`: supply 499,999,102.27 (raw `499999102273241887`, 9 decimals). Mint authority null and freeze authority null, so on the Solana side the supply is frozen and no more can ever be minted and no account can be frozen. This is genuinely safe token hygiene and is the strongest on chain positive.
- BNB Chain contract `0xc135...9cb1`: `symbol` CUDIS, `name` CUDIS, 18 decimals, `totalSupply` approximately 587,769,823.60, and it is `Ownable` with a live owner `0x29b4350d0f40e572ad03bdfb0825bb377175dcd3` (a centralized controller, in contrast to the renounced Solana authorities).
- The two independent supplies sum to approximately 1,087,768,926 tokens, which exceeds the advertised 1 billion "max supply." Without a public, verifiable lock and mint bridge reconciling the two, the "hard cap" is a marketing figure, not a cryptographic guarantee, and CoinGecko's reported 247.5M circulating and 1B total match neither on chain mint.

Utility in practice: the token is tradeable, but on chain Solana DEX liquidity is thin. The deepest Solana pool is CUDIS / SOL on Meteora DAMM v2 with roughly $36.8k of liquidity and roughly $2k of 24h volume; other Solana pools are negligible. The reported approximately $444k daily volume is therefore almost entirely centralized exchange and BNB Chain activity, not Solana on chain trading. Utility beyond speculation (marketplace payments, governance) depends on the closed backend described in Claims 2 to 4.

**IMPACT.** Buyers relying on a fixed 1 billion supply should know the enforced Solana supply is approximately 500M with authorities renounced, while a second, centrally controlled BSC supply pushes the combined total above the stated cap. The Solana mint is safe against rug minting and freezing; the cross chain supply story is not transparently enforced.

## Claim 7: Product and market traction

**CLAIM.** Roughly 20,000 rings sold, roughly 200,000 users across many countries, "billions of biometric signals," a $5M seed led by Draper Associates, listings on major exchanges.

**REALITY: CONFIRMED via external evidence (not code).**

**EVIDENCE.** Consistent across The Block, Chainwire, Morningstar Ventures and retail listings: rings are for sale at multiple retailers, the seed round and investor are on record, and the token is live on 24 venues per CoinGecko (Bitget, MEXC, HTX, Bybit, Bithumb and others). This is a real, shipping consumer product with a real user base.

**IMPACT.** This is why the project scores above the floor: it is a genuine company and product, not a honeypot or an abandoned shell. The failure is transparency and overstated decentralization, not existence.

---

## Positive Findings

- **Solana mint authority is null and freeze authority is null.** The Solana SPL supply cannot be inflated and holder accounts cannot be frozen on that mint. This is the correct, safe configuration and removes the two most common Solana token rug vectors on that chain.
- **Standard SPL token, not `Token-2022`, no transfer hook or fee extension surface,** so there is no hidden transfer tax or freeze extension on the Solana side. Not a pump.fun mint; the address is a vanity keypair prefixed `Cudis`, consistent with a deliberate corporate deployment rather than a memecoin factory launch.
- **Real hardware and real funding.** Shipping rings, a reported 200,000 user base, and a $5M seed led by a well known venture firm. The underlying business exists.
- **Token is genuinely tradeable** on many centralized venues with nontrivial aggregate volume, so exit liquidity is not zero (though on chain Solana liquidity specifically is thin).
- **Identity confirmed independently.** The audited mint is the same address indexed by CoinGecko and quoted by live GeckoTerminal pools, so this report is not analyzing an impostor token.

## Conclusion

CUDIS is a real, VC backed wellness hardware company whose Solana token is configured safely at the mint level, wrapped in a decentralization and open source narrative that the code and chain do not support. The flagship transparency claim, an "Open Source Initiative," is false: the entire `CudisWellness` GitHub organization is five single file README stubs with placeholder clone URLs and references to source files and a `/docs` folder that do not exist. The reward and health data pipeline that would make this a DePIN protocol is centralized off chain accounting with no public contract and no discoverable on chain program; on chain governance, DAO, treasury and staking are marketing constructs at audit time; the AI health agent is an unverifiable closed app feature. On supply, the Solana mint is safely capped at approximately 500M with renounced authorities, but a second, centrally `Ownable` BSC deployment of approximately 588M pushes the combined multi chain supply above the advertised 1 billion cap with no public bridge reconciling them.

The core system is closed or unverifiable, and per policy unseen claims are not credited. The token itself is not an obvious scam, the mint is hygienically configured, and the product ships, which keeps the score off the floor. But almost every decentralization, transparency and on chain claim is overstated or false, and the machinery that would justify the DePIN framing is simply not public.

**Verdict: FLAGGED. Score 35/100. Risk MEDIUM.** Token holder acid risk on the Solana mint is low (authorities renounced), but transparency risk, centralization risk (closed backend plus an `Ownable` BSC supply), cross chain supply integrity risk, and thin on chain liquidity are material, and the project is a microcap trading roughly 99.5% below its all time high.
