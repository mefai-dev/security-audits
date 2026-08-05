# Allora (ALLO): Whitepaper Claims vs Code Reality

**Score: 80/100, RISK MEDIUM**

Date: 2026-08-05
Token (live): native ALLO, base denom `uallo`, 18 decimals, on `allora-mainnet-1`. Maximum supply 1,000,000,000 ALLO (read on chain as `max_supply = 1000000000000000000000000000` uallo). Total minted at time of audit roughly 787.77M ALLO (bank supply `787774321800748230440743393` uallo), about 79 percent of cap, most of it minted at genesis into vesting and foundation accounts. Emission is algorithmic in `BeginBlock`. ALLO trades on Binance, Coinbase and Kraken, listed on the mainnet launch day of 11 November 2025. Launch circulating supply was 200.5M (20.05 percent), fully diluted valuation near 468M USD at listing, with a sharp post launch price decline.
Websites: allora.network, docs.allora.network, explorer via Mintscan and provider dashboards (Polkachu, Ankr).
GitHub: github.com/allora-network (`allora-chain` is the L1; the off chain worker software has since moved from `allora-inference-base` to the current offchain node repos).

## Severity Summary

| Area | Finding | Verdict | Severity |
| --- | --- | --- | --- |
| Inference synthesis | Workers and forecasters aggregated by regret weighted scoring, live on chain | CONFIRMED IN CODE | Info |
| Accuracy based rewards | Scores derived from measured one out losses vs combined network loss | CONFIRMED IN CODE | Info |
| Topics and actor roles | Topic, worker, forecaster, reputer roles are real Cosmos modules with separate handlers | CONFIRMED IN CODE | Info |
| Consensus and staking | CometBFT plus Cosmos SDK staking, dual staking with reputers, live | CONFIRMED IN CODE | Info |
| Permissioned participation | All five whitelists enabled on mainnet; topic creation, worker and reputer entry gated | OVERSTATED (decentralization) | HIGH |
| Parameter and mint authority | Whitelist admins can rewrite emissions and mint params outside token governance | OVERSTATED (immutable schedule) | MEDIUM |
| Token custody and supply | Investor and team tokens centrally custodied off chain; about 79 percent minted | Disclosed in code | MEDIUM |

## Why This Report Exists

Allora markets itself as a decentralized machine intelligence network, an L1 where independent workers submit predictions, forecasters predict who is right, and a network scoring mechanism aggregates everything into a network inference whose quality drives rewards. Those are strong, testable claims. Buyers of ALLO and builders on topics deserve to know whether the flagship mechanism actually runs in the state machine or is a marketing abstraction over an ordinary chain, and whether "decentralized" describes the production network or an aspiration. This report reads the actual Go modules that mainnet runs and cross checks them against live chain state, rather than trusting the litepaper or the token page.

## Method

Read only. No team analysis. The audit pulled the public source of `allora-chain` and pinned the exact release running in production. Live `node_info` reports `allorad` application version `v0.16.0` (git commit `8cbbba6`), CometBFT `0.38.19`, network `allora-mainnet-1` at block height about 10,306,351. All source citations below are from the `v0.16.0` tag so that file and line references match the deployed binary. Claims were then tested against live REST endpoints (Polkachu public API) for parameters, supply, topic count and topic state, and against public market data for tradeability. Where the shipped default and the live on chain value could differ, the live value was queried and is reported.

## The Foundation: a real Cosmos SDK L1 with a genuine inference engine

`allora-chain` is a substantial, heavily tested Cosmos SDK chain, not a fork with a slogan. The intelligence lives in `x/emissions`, with a dedicated `inference_synthesis` package (network inferences, forecast implied inferences, regrets, losses, weights), a `rewards` package implementing the litepaper reward math, and a `mint` module implementing a capped emission schedule. The emissions `EndBlocker` orchestrates the whole epoch loop: it updates active topic weights, advances nonces, and emits rewards every block.

```
x/emissions/module/abci.go  (EndBlocker)
weights, sumWeight, totalRevenue, err := rewards.GetAndUpdateActiveTopicWeights(sdkCtx, am.keeper, blockHeight)
err = rewards.EmitRewards(rewards.EmitRewardsArgs{ Ctx: sdkCtx, K: am.keeper, ModuleParams: moduleParams, BlockHeight: blockHeight, Weights: weights, SumWeight: sumWeight, TotalRevenue: totalRevenue })
```

The test suites are large (for example `rewards_test.go` is over 130KB, `network_inference_builder_test.go` over 59KB), which is consistent with production grade engineering rather than a demo.

## Claim 1: On chain synthesis of worker and forecaster predictions into a network inference

CLAIM: "thousands of machine learning models collaborate to generate stronger, more reliable intelligence" and forecasters predict the accuracy of inferers, with the network combining them by a scoring and weighting mechanism.

REALITY: CONFIRMED IN CODE.

The synthesis is real and matches the litepaper. Inferers submit point inferences; forecasters submit forecasts that are converted into forecast implied inferences; each actor carries a network regret; the combined network inference is a regret weighted average using a p norm gradient. `GetNetworkInferences` pulls inferences, the latest reputer supplied network loss, the forecasts, and each actor regret, then computes forecast implied inferences and the weighted combination.

```
x/emissions/keeper/inference_synthesis/weight.go:237
// I_i = Σ_l w_il I_il / Σ_l w_il
// w_il = φ'_p(\hatR_i-1,l)
// \hatR_i-1,l = R_i-1,l / |max_{l'}(R_i-1,l')|
func calcWeightedInference(args calcWeightedInferenceArgs) (InferenceValue, error) { ... }
```

```
x/emissions/keeper/inference_synthesis/network_inferences.go  (calcNetworkInferencesMultiple)
forecastImpliedInferencesByWorker, _, _, err := CalcForecastImpliedInferences(...)
networkInferences, weights, err := CalcNetworkInferences(calcArgs)
```

Weights come from normalized regrets passed through `alloraMath.Gradient(pNorm, cNorm, normalizedRegret)`, with explicit upper and lower bounds and a zero weight threshold (`CalcWeightFromNormalizedRegret`). This is exactly the mechanism the litepaper describes, implemented in fixed point decimal math. Live proof it runs: topic 1 on mainnet is metadata "BTC/USD - Log Returns - 8h", is active, and shows a nonzero accumulated `initial_regret` of `-0.0815`, which only exists after real epochs of synthesis.

IMPACT: The headline mechanism is not vaporware. The network genuinely fuses many submissions into one output on chain, and the forecaster role (predicting which inferers are right) is implemented, not just described.

## Claim 2: Rewards are tied to measured accuracy on chain

CLAIM: participants earn "rewards based on prediction quality," and reputers score the accuracy of workers.

REALITY: CONFIRMED IN CODE.

A worker inference score is the marginal accuracy contribution: the network loss with that worker held out (`OneOutInfererValues`) minus the combined network loss. A larger positive difference means the network is worse without you, so your score is higher.

```
x/emissions/module/rewards/scores.go:227  (GenerateInferenceScores)
workerNewScore, err := oneOutLoss.Value.Sub(networkLosses.CombinedValue)
```

Forecaster scores use the same leave out logic across one out and one in losses, and reputer scores are computed by a stake weighted consensus with a gradient descent over reputer reported losses and listening coefficients.

```
x/emissions/module/rewards/scores.go  (GenerateReputerScores)
scores, newCoefficients, err := GetAllReputersOutput(losses, reputerStakes, reputerListeningCoefficients, ... params.LearningRate, params.GradientDescentMaxIters, ...)
```

Rewards then flow from these scores. Topic level reward splits between inference, forecasting and reputer tasks follow the litepaper entropy formulas, and each worker reward is its score fraction times the task pool.

```
x/emissions/module/rewards/worker_rewards.go:417
// U_i = ((1 - χ) * γ * F_i * E_i ) / (F_i + G_i + H_i)
func GetRewardForInferenceTaskInTopic(...) (alloraMath.Dec, error) { ... }
// GetRewardPerWorker: reward = fraction.Mul(totalRewards)
```

IMPACT: The economic loop is accuracy driven end to end, from measured loss to score to token reward. There is no shortcut where rewards are flat or purely stake based for workers and forecasters.

## Claim 3: Topics and the reputer, worker, forecaster roles are real Cosmos modules

CLAIM: the network is organized into topics, with distinct worker, forecaster and reputer roles.

REALITY: CONFIRMED IN CODE.

Topics are first class state objects created through `CreateNewTopic`, carrying epoch length, loss method, p norm, quantiles and regret parameters. Roles have separate, independently validated message handlers: `msg_server_worker_payload.go` for inferer and forecaster submissions, `msg_server_reputer_payload.go` for reputer loss bundles, plus registration and stake handlers. On chain there are 23 topics (`next_topic_id = 24`), and topic 1 shows real configuration (`epoch_length 75`, `loss_method "czar"`, recent `epoch_last_ended 10306300`).

```
x/emissions/keeper/msgserver/msg_server_worker_payload.go:32
canSubmit, err := ms.wlk.CanSubmitWorkerPayload(ctx, msg.WorkerDataBundle.TopicId, msg.WorkerDataBundle.Worker)
```

```
x/emissions/keeper/msgserver/msg_server_reputer_payload.go:32
canSubmit, err := ms.wlk.CanSubmitReputerPayload(ctx, rvb.ValueBundle.TopicId, rvb.ValueBundle.Reputer)
```

IMPACT: The role separation the litepaper promises is enforced by the state machine, with distinct scoring and reward paths for each role.

## Claim 4: Consensus is CometBFT plus staking, a decentralized L1

CLAIM: Allora is a "DPoS based Cosmos SDK blockchain."

REALITY: CONFIRMED IN CODE for the consensus and staking machinery; the "decentralized" framing is addressed separately in Claim 5.

The chain wires standard Cosmos SDK `x/staking`, `x/upgrade` and `x/gov`, and runs CometBFT (live `node_info` reports CometBFT `0.38.19`). Security uses dual staking: validator stake and reputer stake both count toward network security, as the mint module sums both.

```
x/mint/keeper/emissions.go  (GetNumStakedTokens)
cosmosValidatorsStaked, err := k.CosmosValidatorStakedSupply(ctx)
reputersStaked, err := k.GetEmissionsKeeperTotalStake(ctx)
```

IMPACT: The base layer is a legitimate proof of stake Cosmos chain producing blocks in production at height over 10.3M.

## Claim 5: Open, decentralized participation

CLAIM: implied throughout the marketing that anyone can register as a worker or reputer and that the network is decentralized.

REALITY: OVERSTATED. In production the network is permissioned by whitelist, and the whitelists are on.

Every participation path is gated by a whitelist qualifier. Topic creation checks `CanCreateTopic`, worker submission checks `CanSubmitWorkerPayload`, reputer submission checks `CanSubmitReputerPayload`. Each qualifier is a kill switch: if the relevant "enabled" parameter is false the action is open, if true only whitelisted addresses pass.

```
x/emissions/keeper/whitelist.go:393
func (k *WhitelistsKeeper) CanCreateTopic(ctx context.Context, actor ActorId) (bool, error) {
    isTopicCreator, err := k.IsEnabledWhitelistedTopicCreator(ctx, actor) ...
    return k.IsEnabledGlobalActor(ctx, actor)
}
```

The shipped defaults set all of these to true:

```
x/emissions/types/params.go:59
GlobalWhitelistEnabled:         true,
TopicCreatorWhitelistEnabled:   true,
GlobalWorkerWhitelistEnabled:   true,
GlobalReputerWhitelistEnabled:  true,
GlobalAdminWhitelistAppended:   true,
```

Critically, this is not just a default. Live mainnet parameters at the time of audit return every flag still enabled:

```
GET /emissions/v9/params  (allora-mainnet-1)
"global_whitelist_enabled": true
"topic_creator_whitelist_enabled": true
"global_worker_whitelist_enabled": true
"global_reputer_whitelist_enabled": true
"global_admin_whitelist_appended": true
```

IMPACT: HIGH for the decentralization narrative. As of this audit, roughly nine months after mainnet, you cannot permissionlessly create a topic, submit inferences as a worker, or act as a reputer. Entry is controlled by whitelist admins. This is a reasonable training wheels posture for a young L1, and it is disclosed in the code, but it is materially different from an open network and should be stated plainly to anyone valuing ALLO on a decentralization thesis.

## Claim 6: Sound, fixed, Bitcoin like tokenomics

CLAIM: "a Bitcoin-like schedule, where the rate of new token creation for rewards decreases over time," with a 1 billion cap.

REALITY: CONFIRMED IN CODE for the mechanism, OVERSTATED on immutability and decentralization of control.

The cap is real and enforced (`max_supply` on chain equals 1,000,000,000 ALLO). Emission is algorithmic in `BeginBlock` through `RecalculateTargetEmission` and `MintCoins`, with a target emission per unit staked token that declines as supply is consumed, plus a genuine vesting schedule with a one year cliff and three year vesting for investor and team allocations.

```
x/mint/module/abci.go:81
blockEmission, _, err = keeper.RecalculateTargetEmission(...)
...
err = k.MintCoins(sdkCtx, coins)
```

Two honesty caveats, both visible in the source. First, token custody is centralized by design; the code says so:

```
x/mint/keeper/emissions.go:19
// these tokens will be custodied by a centralized actor off chain.
```

Second, the schedule is not fixed in the Bitcoin sense. Mint parameters (including emission inputs and supply related values) are mutable by a whitelist admin, and emissions module parameters likewise. There is no token holder governance gate on these; the authority is the admin set.

```
x/emissions/keeper/whitelist.go:367
func (k *WhitelistsKeeper) CanUpdateParams(ctx context.Context, actor ActorId) (bool, error) {
    return k.IsWhitelistAdmin(ctx, actor)
}
```

```
x/mint/keeper/msg_server.go:33
isAdmin, err := ms.IsWhitelistAdmin(sdkCtx, msg.Sender)
```

The admin set is bootstrapped from hardcoded genesis addresses:

```
x/emissions/types/genesis.go:113  (DefaultCoreTeamAddresses)
"allo16270t36amc3y6wk2wqupg6gvg26x6dc2nr5xwl",
"allo1xm0jg40dcvccqvzqwv5skxlpc7t6eku69kfz6y", ... (ten addresses)
```

IMPACT: MEDIUM. The emission engine is credible and capped, but "fixed Bitcoin like schedule" oversells immutability. A small admin set can retune emissions and mint parameters and flip the permissioning switches without a token holder vote. About 79 percent of the 1 billion cap is already minted (largely genesis allocations and vesting), so future issuance is a smaller lever than the raw cap suggests, but control over it is concentrated.

## Positive Findings

- The flagship on chain inference synthesis is genuinely implemented and running, with forecast implied inferences and regret weighted combination matching the litepaper math, not a facade.
- Rewards are truly accuracy conditioned through leave out marginal loss scoring for workers and forecasters and a stake weighted gradient descent consensus for reputers.
- Engineering quality is high: fixed point decimal arithmetic, extensive unit tests, upgrade handlers per version (`v0_3_0` through `v0_9_0` and beyond), and clean module separation.
- The chain is unquestionably live: `allora-mainnet-1`, CometBFT `0.38.19`, `allorad v0.16.0`, height above 10.3M, 23 topics, at least the flagship price prediction topic active with recent epochs.
- The token is real and liquid, listed on Binance, Coinbase and Kraken from launch, with a hard 1 billion cap verifiable on chain.
- Centralization is not hidden. The whitelist gates, admin authority, and centralized custody are all explicit in the public source.

## Conclusion

Allora is a technically legitimate project. The claims that matter most, a real on chain synthesis of worker and forecaster predictions and rewards tied to measured accuracy, are CONFIRMED IN CODE and verified running on a live mainnet with real topics. The base layer is a proper CometBFT plus Cosmos SDK proof of stake chain, and the tokenomics engine is capped and algorithmic. The gap between marketing and reality is about decentralization, not about whether the technology exists. In production the network is permissioned: all participation whitelists are enabled on chain, and a small whitelist admin set, seeded from hardcoded genesis addresses, can rewrite emissions and mint parameters and toggle permissioning outside of token holder governance. That, plus centralized off chain custody of investor and team tokens and roughly 79 percent of supply already minted, is why the risk is MEDIUM rather than LOW despite strong code. Verdict: PASSED, with the clear caveat that "decentralized" currently describes the roadmap, not the live control structure.

Confirmed in code: 4. Overstated: 2. False: 0.
