# Cere Network (CERE): Whitepaper Claims vs Code Reality

**Score: 61/100, MEDIUM RISK (PASSED)**

Date: 2026-08-06

Token (live, independently verified on chain):
- Ethereum ERC20 CERE `0x2dA719DB753dFA10a62E140f436E1d67F2ddB0d6`. On chain `name()` returns "CERE Network", `symbol()` returns "CERE", `decimals()` returns 10 (0x0a), `totalSupply()` returns 100000000000000000000 base units, that is exactly 10,000,000,000 CERE (ten billion). Verified source on Blockscout (Solidity 0.8.3, ERC-20). Roughly 7,011 holders (Ethplorer). A second bridged deployment exists on Base `0x9886447ff4c350f4600e4bf95db756bdc629b1ca` (CoinGecko). Aggregated market value near 2 million USD with 24 hour volume near 556 USD (CoinGecko), that is a thin market.
- Native chain token: the live Cere Substrate chain reports `tokenSymbol` "CERE" and `tokenDecimals` 10 via `system_properties`, matching the ERC20. The ERC20 is a fixed supply bridged representation, while the native chain issues new CERE through staking inflation (see Claim 5).

Websites: cere.network, docs.cere.network, developer.cere.network, ddc.cere.network, explorer.cere.network, live cluster domain ddc-dragon.com.

GitHub: github.com/Cerebellum-Network. Key repos read for this audit: blockchain-node (the Substrate chain, default branch `dev`, last public release tag v7.3.16 mirrored on `master`), cere-ddc-sdk-js (client SDK), ddc-node-provider-auth-contracts (node authorization ink! contracts). Note: on the current `dev` branch the `ddc-payouts`, `ddc-verification`, and `ddc-dac-host` crates are private external repositories (404 to the public); the payout and verification code cited here is the last public in tree version (v7.3.16 / `master`).

---

## Severity Summary

| # | Finding | Area | Status | Severity |
|---|---------|------|--------|----------|
| T | ERC20 is a clean fixed supply token: no mint, no owner, no pause, verified source | Token | CONFIRMED IN CODE | Info (positive) |
| 1 | "Truly decentralized" storage and CDN is in reality a single operator fleet on one Cere domain (ddc-dragon.com) | DDC | OVERSTATED | High |
| 2 | Storage and CDN node operation is permissioned (admin whitelist) and the node daemon is closed source, so it is not permissionless | DDC | FALSE (as marketed) | High |
| 3 | Usage metering is off chain (DAC plus a single authorised caller); only settlement is on chain | DDC | OVERSTATED | Medium |
| 4 | A real Substrate NPoS chain exists, but the validator set is capped at 80 with no surplus and a sudo superuser key is present | Chain | CONFIRMED IN CODE, decentralization OVERSTATED | Medium |
| 5 | Native CERE has genuine on chain utility: customer deposits, cluster and node staking, provider payouts | Token utility | CONFIRMED IN CODE | Info (positive) |
| 6 | Real but modest usage; enterprise and "mainnet" framing overstates a single beta cluster with thin token liquidity | Traction | OVERSTATED | Medium |

Confirmed in code: 4. Overstated: 4. False: 1.

---

## Why This Report Exists

Cere Network markets itself as "The Decentralized Data and Finance Cloud for Enterprises" and claims to be "ENABLING TRUE DATA DECENTRALIZATION" through a Decentralized Data Cloud (DDC) of "Fully Autonomous and Sovereign Data Clusters." Those are strong architectural claims: they assert that storage and content delivery are decentralized, that accounting for usage happens trustlessly on a blockchain, and that the CERE token is the economic engine of a real network. This report tests each claim against the actual public source code and the actual live on chain state, and it separates two questions that are frequently conflated: is the token itself clean (yes), and are the flagship DDC decentralization claims backed by code (largely no, they are overstated). No team, founder, or legal matters are analyzed here; this is a code and chain audit only.

## Method

1. Token verification by direct Ethereum JSON-RPC (`eth_call`) against a public node for `name`, `symbol`, `decimals`, `totalSupply`, `owner`, and a bytecode function selector scan for mint, ownership, and pause entry points. Cross checked against Blockscout (verification status, compiler) and Ethplorer (holders, issuances).
2. Live native chain verification by direct Substrate JSON-RPC against `rpc.mainnet.cere.network`: chain identity, node version, peer count, token properties, and raw state reads (`state_getStorage`, `state_getKeysPaged`) for the validator set and for the DDC pallet storage maps. Storage keys were computed locally with an xxHash64 implementation validated against the canonical `System` prefix.
3. Source reading of the real files in `blockchain-node` (runtime and DDC pallets) and `cere-ddc-sdk-js` (client presets and routing), plus the `ddc-node-provider-auth-contracts` authorization contract, with file and line citations.

## The Foundation: What Is Real

Two things are genuinely real and verifiable, and the report gives Cere full credit for them.

The ERC20 token is clean. Function selector analysis of the deployed bytecode shows no `mint`, no `mint(uint256)`, no `addMinter`, no `owner`, no `transferOwnership`, no `renounceOwnership`, no `pause`, and no `unpause`. The only supply changing entry point is `burn(uint256)`. Supply is fixed at ten billion, the contract is verified (Solidity 0.8.3), and Ethplorer reports no issuance events. This is a standard fixed supply OpenZeppelin style ERC20. There is no rug vector in the token contract itself.

The Substrate chain is real and running. `system_chain` returns "Cere Mainnet Beta", node version 7.4.0, with 24 connected peers and `isSyncing:false`. The runtime is a genuine Polkadot-SDK Nominated Proof of Stake chain (BABE for authoring, GRANDPA for finality, `pallet_staking` for elections), and the DDC pallets are compiled into it and actively used (verified by live state reads below). This is not vaporware; it is a working network. The dispute is narrower and sharper: how decentralized it actually is versus how it is marketed.

---

## Claim 1: A "truly decentralized" storage and CDN network

**CLAIM.** cere.network states the DDC delivers "TRUE DATA DECENTRALIZATION" and "Fully Autonomous and Sovereign Data Clusters," and the launch blog asserts that "data clusters can be truly decentralized, sovereign and autonomous" (blog.cere.network, Dragon 1 launch, 2024-05-16).

**REALITY: OVERSTATED.** The on chain node registry proves the opposite in practice. I read the live `DdcNodes.StorageNodes` map and decoded the registered node hosts. Every storage and CDN node on mainnet is registered under a single Cere controlled domain, ddc-dragon.com, on a handful of contiguous IP ranges. There is exactly one cluster.

Live state read (rpc.mainnet.cere.network):
- `DdcClusters.Clusters` map has 1 entry (one cluster on mainnet).
- `DdcNodes.StorageNodes` map has 68 entries; `DdcClusters.ClustersNodes` also 68 (all 68 nodes belong to the one cluster).
- Decoded node host strings from the map values include `dstorage-42.ddc-dragon.com` (178.18.148.209), `dstorage-43.ddc-dragon.com` (178.18.148.208), `dstorage-57.ddc-dragon.com` (152.53.39.45), `dstorage-60.ddc-dragon.com` (152.53.39.213), and `cdn-3.ddc-dragon.com` (178.251.228.83). Sequential hostnames on one domain across a few IP blocks are the signature of a single operator fleet, not an independent, sovereign, multi party network.

The client SDK confirms the same shape. The shipped CLI presets route every operation to one Cere owned gateway pair per network: mainnet uses `storageUrl: 'https://storage.dragon-1.xyz'` and `cdnUrl: 'https://cdn.dragon-1.xyz'` (cere-ddc-sdk-js, `packages/cli/src/createClient.ts:44-46`), and the endpoint resolver maps writes to the single storage host and reads to the single CDN host with no peer discovery (`packages/ddc/src/routing/EndpointResolver.ts:28-36`). The blockchain RPC endpoints are likewise all Cere domains: `wss://rpc.mainnet.cere.network/ws` (`packages/blockchain/src/papi/descriptors.ts:8-10`). Even the older multi node preset that shipped at tag v2.16.0 listed 64 nodes all named `storage-N.testnet.cere.network`, that is a Cere run fleet rather than third party nodes.

**EVIDENCE.**
- Live `DdcNodes.StorageNodes` = 68, `DdcClusters.Clusters` = 1, decoded hosts all `*.ddc-dragon.com` (state read at storage key prefix `0x05b252b2...4797de`).
- cere-ddc-sdk-js `packages/cli/src/createClient.ts:44-46` (single gateway per network, dragon-1.xyz).
- cere-ddc-sdk-js `packages/ddc/src/routing/EndpointResolver.ts:28-36` (single configured endpoint, no discovery).
- cere-ddc-sdk-js `packages/blockchain/src/papi/descriptors.ts:8-10` (RPC all cere.network).

**IMPACT.** The blockchain layer (chain, content identifiers, cluster and node registry) is real, but the physical storage and delivery layer is a Cere operated cloud. "Truly decentralized" describes a design goal, not the running network. Data availability, censorship resistance, and liveness today depend on one operator continuing to run ddc-dragon.com.

## Claim 2: Anyone can run a node (permissionless participation)

**CLAIM.** The Dragon 1 blog frames the network as one where "everyone can run a cluster having gone through Cere's cluster management governance process to be admitted to the Cere Protocol." Marketing pages describe the network as "COMMUNITY GOVERNED."

**REALITY: FALSE as marketed (participation is permissioned and the software is closed).** Three independent barriers make permissionless node operation impossible today.

First, admission is gated. The reference authorization model is an admin only whitelist ink! contract: `WhiteListAuthContract` holds a single `admin: AccountId`, and both `add_node_pub_key` and `remove_node_pub_key` revert with `OnlyAdmin` unless the caller is that admin (ddc-node-provider-auth-contracts, `white_list/lib.rs`). On the runtime side the chain includes `pallet_ddc_clusters_gov` (blockchain-node `runtime/cere/src/lib.rs:1437`), a governance gate for cluster and node admission, matching the blog's "governance process to be admitted" language. Admission by governance or by an admin key is the definition of permissioned.

Second, the storage and CDN node daemon is not open source. The public org publishes the client SDK, the chain, management UIs, and the authorization contracts, but there is no published storage or CDN node binary that a third party could actually run.

Third, even the chain cannot be fully built from public code: the pallets that perform the economically critical work are private on the current default branch. The repository default branch is `dev`, and there `pallet-ddc-payouts`, `pallet-ddc-verification`, and `ddc-dac-host` (the DAC, that is Data Activity Capture, host) are declared in `Cargo.toml` as external git dependencies whose repositories return HTTP 404 to unauthenticated readers. They were still in tree in the last public release (tag v7.3.16, mirrored on the `master` branch), which is the version cited throughout this report; the production versions on `dev` cannot be independently verified from public source. A would be operator therefore cannot reproduce the metering, verification, or payout path from the public repository.

**EVIDENCE.**
- ddc-node-provider-auth-contracts `white_list/lib.rs` (`struct WhiteListAuthContract { admin }`, `add_node_pub_key` and `remove_node_pub_key` gated by `OnlyAdmin`).
- blockchain-node `runtime/cere/src/lib.rs:1437` (`pallet_ddc_clusters_gov::Config`, cluster governance gate).
- blockchain-node `pallets/ddc-verification/src/lib.rs` returns 404 (not public); the payout pallet header cites off chain DAC validation (Claim 3).

**IMPACT.** "Anyone can participate" and "community governed" overstate reality. Node participation is whitelisted or governance gated, and the software needed to participate is not released, so the operator set is effectively Cere.

## Claim 3: On chain, trustless usage accounting

**CLAIM.** The DDC is presented as blockchain based accounting where storage and delivery are metered and settled on chain, making payments trustless.

**REALITY: OVERSTATED (settlement is on chain, metering and verification are off chain and trusted).** The payouts pallet states its own premise in the header: "The DDC Payouts pallet is used to distribute payouts based on DAC validation" (blockchain-node `pallets/ddc-payouts/src/lib.rs:3`). DAC, that is Data Activity Capture, is an off chain component. The pallet does not measure bytes stored or delivered; it ingests numbers that an off chain actor submits.

Concretely, usage is supplied as data to a single privileged account. The customer usage figures (`stored_bytes`, `transferred_bytes`, `number_of_puts`, `number_of_gets`) arrive as a submitted vector: `send_charging_customers_batch(cluster_id, era, payers: Vec<(T::AccountId, CustomerUsage)>)` (`pallets/ddc-payouts/src/lib.rs:434-439`). That call is restricted to one authorised caller: `ensure!(AuthorisedCaller::<T>::get() == Some(caller), Error::<T>::Unauthorised)` (`:442`). The authorised caller is set by root, that is by governance or sudo: `set_authorised_caller(...) { ensure_root(origin)?; }` with the in code comment "requires Governance approval" (`:359-363`). The chain then charges customers and rewards providers based on those submitted numbers.

The newer generation of this code does not remove the off chain trust, it only spreads it across a permissioned committee. On feature branches the payout state machine sits behind `pallet-ddc-verification`, whose runtime has an `offchain_worker` that reaches out over HTTP (`sp_runtime::offchain::http`) to DDC "collector" nodes to pull usage aggregates, builds Merkle Mountain Range roots over payers and payees, and admits only accounts in a `ValidatorSet` (every payout driving extrinsic is gated by `is_ocw_validator`), subject to configurable `AggregatorsQuorum` and `ValidatorsQuorum` percentages. So even in the newer design, the usage numbers are captured off chain by a DAC pipeline and attested by a permissioned validator set, and only the settlement is trustless. On the current default branch `dev` that verification code is in a private repository (404), so the production metering path is not publicly auditable at all.

So the on chain part is genuine and valuable, namely deposits, charges, and reward distribution are executed and recorded by the runtime, and I confirmed active billing on chain (`DdcPayouts.ActiveBillingReports` has 58 entries live). But the truth of the usage that drives those charges rests on an off chain DAC pipeline plus a single authorised caller (older generation) or a permissioned validator committee (newer generation), not on a trustless on chain measurement. Combined with the verification pallet being non public on `dev` (Claim 2), the "trustless" framing is overstated.

**EVIDENCE.**
- blockchain-node `pallets/ddc-payouts/src/lib.rs:3` ("payouts based on DAC validation").
- `pallets/ddc-payouts/src/lib.rs:434-439` (usage submitted as `Vec<(AccountId, CustomerUsage)>`).
- `pallets/ddc-payouts/src/lib.rs:442` (single `AuthorisedCaller` gate).
- `pallets/ddc-payouts/src/lib.rs:359-363` (`set_authorised_caller` requires `ensure_root`).
- Live `DdcPayouts.ActiveBillingReports` = 58 entries.

**IMPACT.** Users must trust Cere's off chain DAC pipeline and a single authorised submitter for the correctness of what they are charged and what providers are paid. This is a meaningful gap between "blockchain settled" (true) and "trustlessly measured on chain" (not true today).

## Claim 4: A real, decentralized Substrate mainnet with validators

**CLAIM.** Cere presents a Layer 1 mainnet secured by decentralized validators and nominators staking CERE.

**REALITY: CONFIRMED IN CODE that the chain and NPoS staking are real; OVERSTATED on how decentralized the validator set is in practice.** The runtime is a genuine Polkadot-SDK NPoS chain: `pallet_babe` (`runtime/cere/src/lib.rs:451`) for block authoring, `pallet_grandpa` (`:1151`) for finality, and `pallet_staking` with an inflationary `EraPayout = ConvertCurve<RewardCurve>` (`:666`, `:682`). To be fair to the code, it is permissionless by design: elections run through `ElectionProviderMultiPhase` with a `MaxValidators` bound of 5000, plus nominators, a bags list, and nomination pools. This is not hardcoded Proof of Authority.

The gap is between that open capable code and the live network's operation. First, on mainnet the active set is small, capped, and fully subscribed: `Session.Validators` holds 80 accounts, `Staking.ValidatorCount` reads 80, and `Staking.CounterForValidators` also reads 80, meaning there are exactly 80 validator candidates and all 80 are elected, with no surplus competing for slots. An open, competitive validator market normally shows more candidates than seats. That cap is a governance parameter rather than a code limitation, but the observable result today is a small, non competitive set. Second, the runtime includes `pallet_sudo` (`runtime/cere/src/lib.rs:1027`), a superuser key that can dispatch privileged calls, including setting the DDC payouts authorised caller. The chain also still identifies itself as "Cere Mainnet Beta." A real, open capable NPoS chain, yes; a fully decentralized, sudo free, competitive validator market in operation, not yet.

**EVIDENCE.**
- blockchain-node `runtime/cere/src/lib.rs:451` (BABE), `:1151` (GRANDPA), `:666`/`:682` (staking, inflationary reward curve), `:1027` (sudo).
- Live reads: `Session.Validators` = 80 accounts; `Staking.ValidatorCount` = 0x50 (80); `Staking.CounterForValidators` = 0x50 (80).
- `system_chain` = "Cere Mainnet Beta".

**IMPACT.** Chain security is real and staked, but it is a permissioned feeling 80 validator set with a live sudo key. Governance and the DDC billing authority ultimately trace back to privileged keys.

## Claim 5: CERE has real on chain utility

**CLAIM.** CERE is the economic token for storage payments, staking, and provider rewards across the DDC.

**REALITY: CONFIRMED IN CODE.** This claim holds up. Customers lock native CERE as deposits through a `LockableCurrency` ledger: `type Currency: LockableCurrency<...>` and `Ledger<T> = StorageMap<_, _, T::AccountId, AccountsLedger<T>>`, with dispatchables `deposit`, `deposit_extra`, and `withdraw_unlocked_deposit` (blockchain-node `pallets/ddc-customers/src/lib.rs:162`, `:176`, `:350`, `:367`). Providers are paid from those deposits in CERE per measured usage (payout structs `NodeReward` and `CustomerCharge` in `pallets/ddc-payouts/src/lib.rs:74-103`). Separately, `pallet_ddc_staking` bonds CERE for node and cluster participation, wired into the runtime (`runtime/cere/src/lib.rs:1392`). Live state confirms real economic activity: `DdcCustomers.Ledger` has 535 accounts with locked deposits and `DdcCustomers.Buckets` has 1,398 buckets.

**EVIDENCE.**
- blockchain-node `pallets/ddc-customers/src/lib.rs:162` (`LockableCurrency`), `:176` (`Ledger`), `:350`/`:367` (deposit calls).
- `pallets/ddc-payouts/src/lib.rs:74-103` (CERE denominated rewards and charges).
- `runtime/cere/src/lib.rs:1392` (`pallet_ddc_staking`).
- Live `DdcCustomers.Ledger` = 535, `DdcCustomers.Buckets` = 1,398.

**IMPACT.** Unlike many "utility token" narratives, CERE has concrete, exercised on chain roles: deposit collateral, staking bond, and payout unit. This is a genuine strength.

## Claim 6: Real usage and enterprise traction

**CLAIM.** Cere markets Fortune 1000 pilots, enterprise adoption, and a production DDC "for the AI data era."

**REALITY: OVERSTATED (real but modest, and single operator).** There is real usage, which is more than many projects can show: 1,398 customer buckets, 535 funded customer ledgers, and 58 active billing reports on chain. But the scale and framing are inflated. The entire DDC is one cluster of 68 nodes on a single Cere domain (Claim 1), the network still labels itself "Beta," and the tradable token is thin (aggregated market value near 2 million USD, roughly 556 USD of 24 hour volume per CoinGecko). "Enterprise cloud for the AI data era" oversells a single beta cluster whose infrastructure and billing authority are centrally held.

**EVIDENCE.** Live on chain counts above; CoinGecko market and volume figures; `system_chain` = "Cere Mainnet Beta"; Dragon 1 described as a "Beta Cluster" (blog.cere.network, 2024-05-16).

**IMPACT.** Investors and integrators should read the DDC as an operational but early, single operator managed service with on chain billing, not as a large scale decentralized cloud with independent enterprise scale traction.

---

## Positive Findings

- The ERC20 token contract is clean and low risk: fixed ten billion supply, verified source, no mint, no owner, no pause, only a `burn` function. Nothing in the token can be inflated or seized by an owner key.
- A genuine, running Polkadot-SDK NPoS Layer 1 exists (BABE plus GRANDPA plus staking), with 80 staked validators, not a testnet dressed up as a mainnet.
- The DDC pallets are not slideware: `pallet-ddc-customers`, `pallet-ddc-clusters`, `pallet-ddc-nodes`, `pallet-ddc-staking`, and `pallet-ddc-payouts` are all compiled into the live runtime and demonstrably in use (1,398 buckets, 535 funded ledgers, 58 billing reports, 68 registered nodes).
- CERE has real, exercised on chain utility as deposit collateral, staking bond, and payout unit.
- The public source is substantial and readable, and the on chain state is openly queryable, which is what allowed this audit to reach firm conclusions.

## Conclusion

Cere Network is a real project with a clean token and a working blockchain, whose central marketing claim, that its Decentralized Data Cloud is "truly decentralized," is materially overstated by the code and the chain. The ERC20 is a clean, fixed supply, non mintable, ownerless bridged token, and that cleanliness is entirely separate from, and should not be used to launder, the DDC decentralization claims. The chain is a genuine NPoS network and the DDC pallets are deployed and actually used, with real customer deposits and on chain billing denominated in CERE. But the storage and content delivery layer that the whole pitch rests on is, in the live network, a single cluster of 68 nodes on one Cere domain (ddc-dragon.com); node participation is admin whitelisted and the node daemon is closed source; usage metering is performed off chain by a DAC pipeline and submitted by a single authorised caller; and the chain still carries a sudo key and a capped, fully subscribed 80 validator set while labeling itself "Beta."

The net picture is a competently built, centrally operated data cloud with a token and an on chain billing ledger, presented as a decentralized, sovereign, community governed network. That gap is the reason the flagship decentralization and "trustless" claims are marked OVERSTATED (and permissionless participation FALSE), even though the token, the chain, and the token utility are CONFIRMED IN CODE. Because the token is clean and there is genuine, exercised engineering behind the product, the project clears the passing line, but the score is held down by the significant distance between "truly decentralized" marketing and the single operator, permissioned, sudo controlled reality.

**Score: 61/100, MEDIUM RISK (PASSED).** Confirmed in code: 4. Overstated: 4. False: 1.
