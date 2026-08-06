# Roam (ROAM): Whitepaper Claims vs Code Reality

**Score: 48/100, MEDIUM RISK (Flagged)**

Date: 2026-08-06
Prepared by: MEFAI Security, Source Code Audit (ICE/ION deep read)

Token (live, verified on Solana mainnet RPC 2026-08-06):
- Mint: `RoamA1USA8xjvpTJZ6RvvxyDRzNh6GCA1zVGKSiMVkn`
- Program owner: `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (classic SPL Token program, account space 82, no Token-2022 extensions)
- Decimals: 6
- Supply: 995,632,489,227,210 raw units, that is 995,632,489.22721 ROAM
- Mint authority: `DqeBtBQ5Ue4Ms7kjJ9kienccL8piCbiLJRfY3Dp7dRzJ` (ACTIVE, not revoked)
- Freeze authority: `6oSNcJSzSr7UcAcJDKdD3N2tiu2TL6KGVC2etFS3HaM1` (ACTIVE, not revoked)
- Both authority accounts are owned by the System Program (`11111111111111111111111111111111`), space 0, so they are ordinary keypair or vault accounts, not revoked and not held by any published on chain governance program.

Websites: weroam.xyz, user.weroam.xyz (app), whitepaper at weroam.xyz/whitepaper

GitHub: github.com/weroamxyz (org display name "Roam", created 2021-10-14, 20 public repos). Prior identity MetaBlox Labs. Secondary org github.com/RoamLabs (1 repo).

## Severity Summary

| Finding | Area | Severity | Status |
|---|---|---|---|
| Mint authority live on a near fully minted supply, 1 billion cap not enforced on chain | Token | HIGH | CONFIRMED (on chain) |
| Freeze authority live, any holder account can be frozen by a single key | Token | HIGH | CONFIRMED (on chain) |
| WiFi coverage and location are self reported off chain and never verified on chain | Rewards | HIGH | FALSE claim (code) |
| Reward check in requires an admin cosigner and records only an opaque hash on chain | Rewards | MEDIUM | CONFIRMED (code) |
| Core reward engine, points ledger, node controller, and the on chain check in program are closed or unpublished | Transparency | MEDIUM | CONFIRMED (absence) |
| No public Solana staking or rewards program, only old EVM era MetaBlox code is open | Programs | MEDIUM | CONFIRMED (code) |
| Network size claims conflate third party OpenRoaming federation hotspots with Roam nodes | Traction | MEDIUM | OVERSTATED |
| Decentralized identity DID and VC SDKs genuinely open sourced | Identity | POSITIVE | CONFIRMED (code) |

## Why This Report Exists

Roam markets itself as the largest decentralized wireless network, a DePIN that pays a global mesh of node operators for real WiFi coverage, secured and made verifiable by the Solana blockchain, OpenRoaming, decentralized identifiers, and verifiable credentials. A prior review flagged that the SPL mint carries both an active mint authority and an active freeze authority. This report independently confirms the token state on chain and, more importantly, reads whatever actual public source exists to test the flagship claim that the coverage and reward mechanism is on chain and verifiable rather than a centralized application that hands out points. The focus is the whole project. The token contract is secondary but included. There is no team analysis here.

## Method

1. Queried Solana mainnet RPC directly (`getAccountInfo` jsonParsed, `getTokenSupply`) for the mint and for both authority accounts to establish live token facts and to test whether the authorities are revoked, held by a program, or held by plain accounts.
2. Enumerated the project GitHub org `weroamxyz` (20 public repos) and inspected the repositories relevant to the core claims: `solana_checkin`, `did-sdk-go_solana`, `metablox-staking`, `roam-smart-contracts`, and the DID registry and resolver.
3. Downloaded and read the actual Go source of the check in backend and the staking service, citing file and line.
4. Cross read the public whitepaper table of contents, Solana Compass, DePINscan, and launch coverage for the marketed claims, then compared each marketed claim against the code and the chain.
5. Labeled every material claim CONFIRMED IN CODE, OVERSTATED, or FALSE, backed by a file and line reference or an on chain fact.

Verdict counts: CONFIRMED IN CODE 3, OVERSTATED 4, FALSE 1.

## The Foundation: ROAM Is a Plain SPL Token, and the Reward Engine Is a Server

On chain, ROAM is an ordinary SPL token. The mint account is owned by the standard SPL Token program, occupies the classic 82 byte layout, and carries no Token-2022 extensions such as transfer hooks or a permanent delegate. There is no custom on chain program embedded in the mint itself. Everything Roam calls proof of service, mining, and points lives above this token in application code.

The single most important structural fact for a DePIN audit is where reward truth is computed. In Roam's case the public code answers plainly: reward attestation is produced by a centralized backend server, and the chain is used only to record an admin signed marker. The rest of this report walks the specific claims against that reality.

## Claim 1: The Coverage and Reward Mechanism Is On Chain and Verifiable

CLAIM: Marketing states that "The Solana blockchain anchors the proof-of-service consensus and records contribution data," that operators earn "rewards tied to node uptime via proof-of-service," and that connecting to "verified Roam or OpenRoaming access points earns check-in rewards" (Solana Compass project page, weroam whitepaper).

REALITY: FALSE as stated. The public check in service builds a Solana transaction whose only on chain payload is an opaque string hash. The user's claimed WiFi location is submitted to the server as a plain self reported latitude and longitude string and is never placed on chain, never cryptographically bound to a physical access point, and never validated by any published program. The chain records that an admin key attested a hash, nothing about actual coverage.

EVIDENCE:
- `weroamxyz/solana_checkin`, `api/check/v1/check.go:10-14`: the reward endpoint is `POST /solana/3w/tx`, and it accepts `UserAddress`, `Did`, `Location` as a free text string with the example `"40.748817,-73.985428"`, and `Timestamp`. Location is a self reported string.
- `internal/logic/check/check.go:48-68` (buildCheckInInst): the instruction data serializes only an eight byte discriminator plus a single `BizHash` string field. The location, the DID, and the timestamp are not included in the on chain instruction data. Coverage is not proven on chain, only a hash is.
- `internal/logic/check/check.go:62`: the account meta list marks `Admin` with `IsSigner: true`. `check.go:94` sets `FeePayer: adminAccount.PublicKey` and `check.go:104` sets `Signers: []types.Account{adminAccount}`. Every check in must be cosigned by the Roam admin key, so the server is a mandatory gatekeeper on what the chain will accept.

IMPACT: The flagship decentralization claim does not hold at the mechanism level. Whether a node "provided coverage" is decided off chain by Roam's server and rubber stamped on chain by an admin signature over a hash. A third party cannot independently verify from the chain that any given reward corresponds to real WiFi service. This is the definition of a centralized application that mints points, dressed as on chain proof of service.

## Claim 2: An On Chain Program Governs ROAM Rewards and Staking

CLAIM: ROAM "can be staked and used for community governance," and points are "burned to receive ROAM tokens," implying an on chain rewards and staking program on Solana.

REALITY: OVERSTATED, and the relevant program is unpublished. The check in client does reference an on chain program, so an Anchor style program with the eight byte discriminator `[209, 253, 4, 217, 250, 241, 207, 50]` and a PDA seeded by `"config"` is being called. But that program's source is not in any public Roam repository, and its program ID is read from a private config value, not committed to the repo. Separately, the only staking code Roam has ever open sourced is the old MetaBlox EVM service, not a Solana staking program, and its interest logic is a stub.

EVIDENCE:
- `solana_checkin/internal/logic/check/check.go:27-30`: `programId` is loaded from configuration (`solana.programId`) and the config PDA is derived as `FindProgramAddress([][]byte{[]byte("config")}, programId)`. The program itself is external to the repo and its address is not disclosed in code.
- Org language inventory for `weroamxyz`: repositories are Go, TypeScript, JavaScript, Solidity, Swift, C, and MDX. There is no Rust and no Anchor program repository, so no on chain Solana program source is published.
- `weroamxyz/metablox-staking` is the only staking repo. `go.mod` module is `github.com/metabloxStaking` and it depends on `github.com/ethereum/go-ethereum`, so it is EVM, not Solana. `stakingContract/stakingContract.go:11-40` is an auto generated go-ethereum binding around an Ethereum contract. `interest/interest.go:3` is a placeholder: `func CalculateInterest() { //placeholder, should query Colin's code to update interest values in db }`. Interest was computed off chain into a database.
- `weroamxyz/roam-smart-contracts` is Solidity and is archived, another EVM artifact rather than the live Solana logic.

IMPACT: The claim that Solana programs govern ROAM economics is only half true. A closed check in program exists and is invoked, but staking, rewards accounting, and points are not demonstrable from any public on chain program. The public "smart contracts" are legacy EVM code from the MetaBlox era, and one of them literally leaves interest calculation as a to do. Investors cannot audit the rules that actually mint and distribute value.

## Claim 3: A Fixed One Billion ROAM Supply

CLAIM: "The total supply is fixed at 1 billion ROAM," with 400 million at token generation and 600 million mined over time (Solana Compass, whitepaper tokenomics).

REALITY: OVERSTATED and a live risk. The "fixed" cap is a policy statement, not an on chain constraint. On chain the supply already stands at 995,632,489.22721 ROAM, roughly 99.6 percent of the stated one billion, while the mint authority remains active. A single key can mint beyond the advertised cap at any time because nothing on chain enforces the ceiling.

EVIDENCE:
- RPC `getTokenSupply` on `RoamA1USA8xjvpTJZ6RvvxyDRzNh6GCA1zVGKSiMVkn`: amount 995,632,489,227,210 at decimals 6.
- RPC `getAccountInfo` jsonParsed on the mint: `mintAuthority` is `DqeBtBQ5Ue4Ms7kjJ9kienccL8piCbiLJRfY3Dp7dRzJ`, present and not null.

IMPACT: HIGH. A near fully minted supply plus a live mint authority means the supply cap depends entirely on the honesty and key security of the authority holder. If that key mints more, or is compromised, holders are diluted with no on chain protection.

## Claim 4: Live SPL Token Authorities, the Freeze Risk

CLAIM (from prior review, to be confirmed): the mint and freeze authorities are both active.

REALITY: CONFIRMED on chain. Both authorities are present and not revoked.

EVIDENCE:
- Mint authority `DqeBtBQ5Ue4Ms7kjJ9kienccL8piCbiLJRfY3Dp7dRzJ` and freeze authority `6oSNcJSzSr7UcAcJDKdD3N2tiu2TL6KGVC2etFS3HaM1`, both returned by the jsonParsed mint account.
- Both authority addresses resolve to accounts owned by the System Program with space 0, so they are plain wallet or vault accounts, not held by any published multisig or governance program that a reader could inspect.

IMPACT: HIGH. An active freeze authority means a single key can freeze any ROAM token account, blocking transfers for arbitrary holders, an unusual and centralizing power for a token traded on major exchanges. Combined with the live mint authority, the token carries two of the classic centralized control levers at once. This is a genuine risk regardless of the operator's intentions.

## Claim 5: Global OpenRoaming Network of Millions of Nodes and Users

CLAIM: "Over 2.3 million users and 2 million+ WiFi nodes across 190+ countries," access to "7.5+ million hotspots," "first on DePINscan for hardware nodes."

REALITY: OVERSTATED. OpenRoaming is a real Wireless Broadband Alliance industry standard, and Roam's integration with it and its DID and VC identity layer are genuine. But the headline hotspot counts fold in the global OpenRoaming federation, which is millions of access points owned by airports, carriers, cities, and vendors that are not Roam's network and not rewarded by ROAM. Roam's own contribution is the self built node count, cited around 600,000, and those nodes are attested by the self reported check in flow examined in Claim 1, with no on chain coverage proof. User and node totals are operator reported and not verifiable on chain.

EVIDENCE:
- The whitepaper landing itself separates the numbers: roughly 4.5 million OpenRoaming hotspots versus over 3 million self built WiFi nodes, confirming that most of the advertised coverage is third party federation, not Roam infrastructure.
- Per Claim 1 code, node and coverage validity is decided off chain, so none of the totals can be independently reconstructed from Solana state.

IMPACT: The project is real and operating, but the marketed scale is inflated by counting an external federation as if it were Roam's decentralized network, and the Roam specific figures rest on centralized self reporting rather than verifiable on chain data.

## Positive Findings

- The token is live and the on chain facts match a straightforward, standard SPL token with no exotic Token-2022 traps such as transfer hooks or a permanent delegate. What you see is what you get at the token layer.
- The decentralized identity work is genuinely open sourced. `weroamxyz` publishes `metablox-did-sdk`, `metablox-did-registry`, `metablox-did-resover`, and `did-sdk-go_solana`, so the DID and verifiable credential claims are backed by real, readable code rather than marketing alone.
- The organization is long lived and not anonymous vaporware. The GitHub org dates to 2021, the MetaBlox to Roam lineage is public, and there is real product surface: a live app, eSIM service, and hardware routers.
- A real on chain footprint exists beyond a bare token. The check in client demonstrably calls an Anchor style program with a config PDA, so contribution markers are being written to Solana, even if the program source is unpublished and the marker is only a hash.
- ROAM has genuine market traction, listed on multiple large exchanges (Bybit, Bitget, Gate, KuCoin, MEXC, Backpack, LBank, and others per launch coverage), which distinguishes it from a purely speculative deployment.

## Conclusion

Roam is a real, operating DePIN business with a live SPL token, an open identity stack, and meaningful market presence, so it clears the bar for a passing score. But its central technical promise, that WiFi coverage and rewards are on chain and verifiable, does not survive a source read. The public check in backend proves that coverage is self reported off chain, that the on chain record is only an admin signed opaque hash, and that a Roam admin key must cosign every reward event. The programs that would actually govern rewards and staking on Solana are not published, and the only open staking code is legacy EVM work with a placeholder interest function. On chain, ROAM is a plain SPL token whose advertised fixed one billion cap is not enforced, with 99.6 percent already minted while the mint authority stays live, and with a live freeze authority that lets a single key freeze holders. None of this proves bad intent, and the identity SDKs and exchange traction are real positives, but the flagship decentralization and verifiability claims are overstated to false, and the token carries two genuine centralized control risks. Treat the coverage and reward numbers as operator reported marketing, not as chain verifiable facts, and treat the live mint and freeze authorities as material risks until they are revoked or moved to a transparent, inspectable governance program.

Score: 48/100, MEDIUM RISK. Flagged, because a single unaccountable key can mint past the advertised cap and freeze any holder's account today, and the flagship on chain verifiable coverage claim is false, with the genuine app and hardware product and the open source DID and VC SDKs keeping it at the top of the flagged band rather than lower.
