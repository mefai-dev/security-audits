# Bittensor: Whitepaper Claims vs Code Reality
**Score: 50/100, HIGH (Flagged)**
Date: 2026-08-06

Token (live, independently found and confirmed onchain):
- Canonical token is native TAO on the subtensor Substrate L1 (its own chain, not an ERC20). No contract address on CoinGecko; the asset is the native gas and staking token. Any wrapped or bridged TAO on other chains is non canonical.
- Decimals: 9. Confirmed in code. The base unit is RAO, 1 TAO equals 1e9 RAO. Runtime sets `const EVM_TO_SUBSTRATE_DECIMALS: u64 = 1_000_000_000` in `runtime/src/lib.rs`, and existential deposit is 500 RAO. SS58 prefix is 42.
- Total and max supply: 21,000,000 TAO, hard capped in code. `pallets/subtensor/src/lib.rs` defines `TotalSupply()` returning `21_000_000_000_000_000` RAO (21 quadrillion RAO = 21M TAO).
- Emission and authority: Bitcoin style logarithmic halving computed in `pallets/subtensor/src/coinbase/block_emission.rs` (`calculate_block_emission`), halving against half of total supply, and returning 0 once total issuance reaches the 21M cap. Base default emission is 1 TAO per block (`DefaultBlockEmission = 1_000_000_000` RAO, marked deprecated in favor of runtime calculation). There is NO direct mint extrinsic and `sudo_set_total_issuance` is deprecated. However a live root `pallet_sudo` remains in the runtime and can perform runtime upgrades (`System::set_code`) and force balance changes (`Balances::force_set_balance`), so the cap is enforced by current code, not by an immutable trustless guarantee.
- Market (CoinGecko, live): price about 195.73 USD, market cap about 1.88B USD, market cap rank 42, FDV about 4.11B USD, 24h volume about 80M USD, circulating supply about 9.60M TAO, total and max supply 21.00M TAO.

Websites:
- https://bittensor.com
- https://github.com/opentensor/subtensor (monorepo: chain, Python SDK, btcli, docs, website)
- https://github.com/opentensor/bittensor (Python SDK, now folded into the monorepo `sdk/python`)

### Verified in code and on chain (MEFAI deep source review)
- Repo is genuinely open source. `opentensor/subtensor` is a public monorepo containing the Substrate chain (`pallets/`, `runtime/`, `node/`), EVM precompiles (`precompiles/`), ink! contracts, the Python SDK and `btcli` (`sdk/`), docs, and integration tests. I fetched and read specific files, cited below.
- Supply cap is real and in code: `pallets/subtensor/src/lib.rs` `TotalSupply()` = `21_000_000_000_000_000` RAO. Emission returns 0 at the cap in `coinbase/block_emission.rs` via `if total_issuance >= TotalSupply::<T>::get() { return Ok(0) }`.
- Halving is real and in code: `coinbase/block_emission.rs` computes a logarithmic residual of issuance against half the total supply and uses a power of two divisor on the base emission, a Bitcoin style decay curve.
- Yuma consensus is real and in code: `pallets/subtensor/src/epoch/run_epoch.rs` and `epoch/math.rs` compute weights, active stake, consensus via `weighted_median_col`, ranks, validator trust, incentive, EMA bonds, and dividends via `matmul_transpose(&ema_bonds, &incentive)`, then split emission into `server_emission` (miners) and `validator_emission` (validators).
- Subnets and dTAO staking are real and in code: the runtime includes `Swap` (index 28), `AlphaAssets` (index 31), and `LimitOrders` (index 32) pallets, plus `coinbase/alpha.rs`, `coinbase/subnet_emissions.rs`, and the `swap/` and `staking/` source directories, implementing the dynamic TAO (dTAO) subnet token AMM and staking.
- EVM layer confirmed: runtime includes `Ethereum` (21), `EVM` (22), `EVMChainId` (23), `BaseFee` (25), plus ink! `Contracts` (29). TAO is the native EVM gas token; this is an execution layer on the same L1, not a separate token.
- AdminUtils is constrained on supply: `pallets/admin-utils/src/lib.rs` exposes root and subnet owner hyperparameter setters (take, owner cut, tempo, difficulty, adjustment alpha) but has NO mint, NO set balance, and `sudo_set_total_issuance` returns `Error::Deprecated`. It cannot by itself inflate beyond the schedule.
- Governance collective removed: `runtime/src/lib.rs` shows `pallet_collective` (Triumvirate, was index 8) and `pallet_membership` (Senate members, was index 10) commented out and removed, while `Sudo` (index 12) is retained. Root authority is therefore concentrated in the sudo key rather than a collective.
- Live sudo key exists: `node/src/chain_spec/finney.rs` sets the genesis sudo key to `5FCM3DBXWiGcwYYQtT8z4ZD93TqYpYxjaAfgv6aMStV1FTCT`. `Balances` (index 5) is present, so a Root origin can call `force_set_balance`, and `System::set_code` allows arbitrary runtime replacement.
- Transfers are standard: native TAO uses `pallet_balances` (index 5) and `TransactionPayment` (index 6). There is no owner settable transfer tax, no blacklist, and no honeypot logic on the native token.
- Market state verified live on CoinGecko: native token, no ERC20 platform, 21M total equals 21M max, about 9.6M circulating, rank 42, deep multi venue liquidity around 80M USD daily.

### Claim vs reality
- "Fixed maximum supply of 21 million TAO" is CONFIRMED IN CODE: `pallets/subtensor/src/lib.rs` `TotalSupply()` = `21_000_000_000_000_000` RAO and `coinbase/block_emission.rs` returns 0 emission at the cap.
- "Bitcoin style halving emission" is CONFIRMED IN CODE: logarithmic halving against half the supply in `coinbase/block_emission.rs`, base 1 TAO per block.
- "9 decimals, RAO base unit" is CONFIRMED IN CODE and on chain: `EVM_TO_SUBSTRATE_DECIMALS = 1_000_000_000` in `runtime/src/lib.rs`, CoinGecko max supply matches.
- "Yuma consensus rewards miners and validators by stake weighted agreement" is CONFIRMED IN CODE: `epoch/run_epoch.rs` computes consensus, incentive, bonds, dividends, and splits server and validator emission.
- "Subnets with their own dTAO staking markets" is CONFIRMED IN CODE: `Swap`, `AlphaAssets`, and `LimitOrders` pallets plus `coinbase/alpha.rs` and `swap/` implement the dynamic subnet token AMM.
- "No admin can mint or inflate TAO" is OVERSTATED: AdminUtils cannot mint and `sudo_set_total_issuance` is deprecated, but a live `pallet_sudo` root key (genesis key `5FCM3DBXWiGcwYYQtT8z4ZD93TqYpYxjaAfgv6aMStV1FTCT`) can upgrade the runtime and force set balances, so the no inflation property depends on the sudo holder, not on immutable code.
- "Decentralized network" is OVERSTATED for chain governance: validation and staking are open and permissionless, but the removal of the Triumvirate and Senate collectives leaves a single root sudo key with full upgrade authority. This is a real centralization vector at the protocol authority layer.
- "Whitepaper describes the tokenomics (subnets, Yuma, halving, dTAO)" is OVERSTATED: the public whitepaper at bittensor.com is the 2021 peer to peer intelligence market paper and does NOT mention subnets, Yuma by name, halving, the 21M cap, or dTAO. Those live in the docs and the code, not the original paper. The token mechanics are confirmed by code, so this is a documentation gap, not a false token claim.

### Severity
- CRITICAL: Live root `pallet_sudo` (genesis key `5FCM3DBXWiGcwYYQtT8z4ZD93TqYpYxjaAfgv6aMStV1FTCT`) can dispatch any Root call, including `System::set_code` (arbitrary runtime replacement) and `Balances::force_set_balance`. It can therefore in principle remove the 21M cap, mint, or freeze. The collective governance (Triumvirate and Senate) was removed, concentrating authority in this single key. This caps ownership control and Flags the project regardless of how legitimate the network is.
- HIGH: Broad privileged surface. `pallets/admin-utils` plus sudo can alter core network parameters and consensus inputs, including `swap_authorities` (block producers), `schedule_grandpa_change` (finality set), `sudo_set_tempo`, and difficulty, affecting emission distribution and validator set.
- MEDIUM: Large and complex attack surface. The runtime bundles an EVM (Frontier `Ethereum`, `EVM`, `BaseFee`), ink! `Contracts`, a `Swap` AMM for dTAO, `MevShield`, `LimitOrders`, `Drand`, and `Crowdloan`. Any bug in these components can affect user funds.
- MEDIUM: No on chain disclosure of the sudo holder and no visible timelock on sudo actions. The Sudo `BaseCallFilter` (`InsideBoth<SafeMode, NoNestingCallFilter>`) blocks nested batches but does not restrict the scope of Root dispatch.
- LOW: Documentation gap. The 2021 whitepaper omits the current tokenomics (subnets, Yuma, halving, 21M cap, dTAO), which live only in docs and code.
- INFO: Canonical TAO is native on the subtensor L1 with 9 decimals (RAO base unit); supply is hard capped at 21M with Bitcoin style halving confirmed in code; wrapped or bridged TAO on other chains is non canonical.
