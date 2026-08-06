# Akash Network: Whitepaper Claims vs Code Reality
**Score: 77/100, LOW (Passed)**
Date: 2026-08-06

Token (live, independently found and confirmed onchain):
- Symbol AKT, native base denom `uakt` (micro AKT, 6 decimals). Decimals confirmed on chain: total supply reads 296,512,863,216,948 `uakt` which equals about 296.5 million AKT, matching market listings.
- Chain: Akash, its own Cosmos SDK layer 1 using CometBFT proof of stake (Akash forks of `cosmos-sdk v0.53` and `cometbft v0.38`).
- Price about $0.4842, market cap about $143.6M, fully diluted valuation about $143.6M, rank about #200, 24h volume about $3.76M (CoinGecko, independently fetched).
- Circulating about 292.1M AKT, on chain total supply about 296.5M AKT. CoinGecko lists a "max supply" of 388,539,008 AKT, but this is a projected target and is not enforced anywhere in code.
- On chain mint: `mint_denom = uakt`, inflation_min 3%, inflation_max 4%, goal_bonded 67%, current inflation 4% (sits at the max because bonded stake is about 88.3M AKT, roughly 30% of supply, well under the 67% goal).

Websites:
- https://akash.network
- Docs: https://akash.network/docs
- GitHub org: https://github.com/akash-network (repos `node`, `provider`, `console`, `website`, `chain-sdk`)

### Verified in code and on chain (MEFAI deep source review)
- Repo and module layout fetched from `github.com/akash-network/node`. `go.mod` module path is `pkg.akt.dev/node/v2` on `cosmos-sdk v0.53.7-akash.2`, `cometbft v0.38.21-akash.1`, `wasmd v0.61.7`, `ibc-go v10`. Custom Akash forks of the base chain libraries are used.
- Custom modules under `x/`: `deployment`, `market`, `provider`, `escrow`, `cert`, `audit`, `take`, `bme`, `oracle`, `epochs`, `wasm`. Standard Cosmos modules (`mint`, `staking`, `distribution`, `gov`, `slashing`, `authz`, `feegrant`, `upgrade`, ibc `transfer`) are wired in `app/app.go`.
- Reverse auction marketplace is real and on chain. `x/market/keeper/keeper.go` implements `CreateOrder`, `CreateBid`, `CreateLease`, plus lifecycle handlers `OnBidMatched`, `OnBidLost`, `OnLeaseClosed`, `OnGroupClosed`. Orders open, providers post competing bids, the tenant selects a bid, a lease is created and paid per block from an escrow account.
- Take fee is in code. `x/take/keeper/keeper.go` `SubtractFees` calls `findRate(denom)` which reads `params.DefaultTakeRate` and per denom overrides `params.DenomTakeRates`, dividing the integer by 100 to get a percentage. Defaults from `akash-api` `go/node/take/v1beta3/params.go`: `DefaultTakeRate = 20` (20%) with `uakt` overridden to `2` (2%), so paying providers in AKT costs a much lower protocol fee. Validation rejects any rate above 100 and requires `uakt` to always be present.
- Inflation is standard Cosmos `x/mint`, governance parameterized, currently bounded 3% to 4%. There is no hard maximum supply enforced in code. Live params fetched from a public REST node confirm the values above.
- Module mint authority is limited. `app/mac.go` `ModuleAccountPerms` grants `Minter` to the `mint` module (staking inflation), `Minter` and `Burner` to ibc `transfer` (standard vouchers), and `Minter` and `Burner` to the `bme` module. `escrow`, `take` and `market` hold no mint permission.
- The `bme` module (Burn Mint Equilibrium) is a dual token layer. `x/bme/keeper/keeper.go` mints and burns a separate USD pegged compute credit `uact` against `uakt` using oracle prices. `mintACT` mints `uact` when a user burns AKT, `burnACT` burns `uact` and can mint AKT back, `prepareToBM` computes the swap rate as `priceFrom / priceTo` from oracle prices, applies a configurable spread, and enforces a collateral ratio and circuit breaker thresholds. Minting is algorithmic and oracle driven, not an arbitrary admin mint, but it is a second path that can increase AKT supply.
- Oracle prices come from permissioned feeders, not a single key. `x/oracle/keeper/keeper.go` only accepts prices from authorized sources ("source ... is not authorized oracle provider"), aggregates a time weighted average, and applies deviation and staleness health checks before a price is usable.
- CosmWasm is present but locked down. On chain `cosmwasm/wasm/v1` params return `code_upload_access.permission = "Nobody"` (only a governance authority can store new contract code) with `instantiate_default_permission = "Everybody"`. This limits arbitrary malicious code uploads.
- No single owner or admin key. Parameter changes (mint, take, oracle feeder set, bme spread and thresholds) and software upgrades run through on chain AKT weighted governance and the `upgrade` module, not a private wallet.

### Claim vs reality
- Claim: "decentralized cloud computing marketplace" with a reverse auction, on chain leases and escrow. CONFIRMED IN CODE. See `x/market` order, bid and lease flow and `x/escrow`.
- Claim: providers compete on price and users pick a provider, paying only for resources used. CONFIRMED IN CODE. Bids carry price and resource offers, tenant selects the winning bid, lease locks price.
- Claim: AKT is a staking and governance token with inflation. CONFIRMED IN CODE and ON CHAIN. Standard `mint`, `staking`, `gov`, `distribution` and `slashing` modules; live inflation 4%, goal bonded 67%.
- Claim: protocol take rate on lease settlement. CONFIRMED IN CODE. `x/take` default 20%, only 2% when settled in AKT.
- Claim: dual token model with a USD pegged compute credit (ACT) and AKT top up when a circuit breaker is in effect. CONFIRMED IN CODE, with risk. `bme` mints and burns `uact` versus `uakt` off oracle prices with a spread and collateral or circuit breaker logic. The peg is algorithmic and depends on a permissioned oracle feeder set, so it is not a hard collateralized peg.
- Claim: a fixed max supply of about 388M AKT. OVERSTATED. Nothing in code caps supply; inflation is governance set within min and max params that governance itself can change, and the `bme` module can also mint AKT. The 388M figure is a target, not an enforced cap.
- Claim: "up to 85% lower cost than traditional cloud" and "providers in 85+ countries". OVERSTATED as an audit matter. These are marketing and off chain metrics that cannot be verified from source code or chain state.
- Claim: censorship resistant, no single entity can shut down deployments. CONFIRMED IN CODE at the protocol level (decentralized validators and governance, freely transferable native coin, no blacklist or freeze on `uakt`), with the honest caveat that individual providers can drop a lease and the oracle feeder set and governance are points of coordination.

### Severity
- Critical: 0. No single key mint, no owner drain path, no hidden freeze or blacklist on the native coin.
- High: 0. There is no private admin key that can inflate or halt the chain; monetary control sits with on chain governance and bounded modules.
- Medium (3):
  1. Inflation is governance set with no hard supply cap in code. Bounds are currently 3% to 4% but governance can change the bounds, so the advertised max supply is not guaranteed.
  2. The `bme` module can mint AKT algorithmically based on a permissioned oracle feeder set. A faulty or captured feeder, or a peg or collateral bug, is a supply and solvency risk. Health checks and circuit breakers mitigate but do not remove it.
  3. Take rate is governance changeable and validation only caps it at 100%, a very loose ceiling. A governance capture could raise provider fees dramatically.
- Low (2):
  1. The chain runs custom forks of `cosmos-sdk` and `cometbft`, which adds review burden versus upstream releases.
  2. Liquidity is modest, about $3.76M daily volume on a $143M market cap (roughly 2.6% turnover), spread across CEXs, Osmosis and IBC.
- Informational (2):
  1. The `akash-api` types repo was archived on Jan 5, 2026 and superseded by the open source `chain-sdk`; generated types remain public.
  2. Newer modules such as `bme` lack package level doc comments, so intent relies on reading the keeper code.
