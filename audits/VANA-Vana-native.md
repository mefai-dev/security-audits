# Vana: Whitepaper Claims vs Code Reality
**Score: 73/100, MEDIUM (Passed)**
Date: 2026-08-06

Token (live, independently found and confirmed onchain):
- Canonical VANA is the native gas coin of the Vana L1, an EVM chain with chain id `1480` (RPC `https://rpc.vana.org`, explorer vanascan.io). Because it is the native coin there is no token contract, so the canonical address is native. Decimals 18.
- Supply: max 120,000,000 VANA (CoinMarketCap and CoinGecko agree), circulating about 30.1M, total about 112.6M to 120M depending on source. FDV about $103M is consistent with 120M times the live price.
- A bridged ERC20 representation of VANA exists on Ethereum at `0x7ff7fa94b8b66ef313f7970d4eebd2cb3103a2c0` (name Vana, symbol VANA, decimals 18, holders about 939), and the SAME address is listed on Base, BNB Chain, Polygon, Arbitrum and Optimism. On Ethereum this contract holds only about 74,186 VANA (verified live via `eth_call totalSupply` returning `0x0fb5a78b75ad71a19000` and cross checked on ethplorer), which is a tiny bridged slice, not the canonical 120M issuance. Its `owner` is a 3 of N Gnosis Safe v1.4.1 at `0xddf71a6ddf3fcabbba4d607b57f0f6fc0265bb84` (verified on Ethereum: `getThreshold()` returns 3, `VERSION()` returns `1.4.1`). This Safe is the Ethereum bridge and mint authority.
- Live market as of 2026 08 06: price about $0.86, market cap about $26M (rank about #545 on CoinMarketCap and about #698 on CoinGecko), FDV about $103M, 24h volume about $3.5M, MC to FDV about 0.26. Deepest venue is Binance VANA/USDT at about $0.98M in 24h; liquidity is centralized exchange dominated and on chain DEX depth is thin.

Websites:
- https://www.vana.org
- https://docs.vana.org
- https://github.com/vana-com (repo `vana-com/vana-smart-contracts`)
- https://vanascan.io

### Verified in code and on chain (MEFAI deep source review)

Source: `github.com/vana-com/vana-smart-contracts`, MIT license, with an `audits/` folder holding three Hashlock PDFs and six Nethermind PDFs. All Solidity below was fetched from branch `main` and all addresses came from the repo's own `deployments-official/vana` artifacts, then confirmed live by RPC.

Every core contract is a UUPS implementation behind an ERC1967 proxy. Deployed and live on Vana mainnet (chain `1480`, confirmed by `eth_getCode` returning the ERC1967 proxy with the standard implementation slot constant), with live usage counters read directly by `eth_call`:
- DataRegistry `0x8C8788f98385F6ba1adD4234e551ABba0f82Cb7C`, `version()` = 2, `filesCount()` = 20,073,985.
- TeePool `0x3c92fD91639b41f13338CE62f19131e7d19eaa0D`, `jobsCount()` = 17,779.
- DLPRegistry `0x4D59880a924526d1dD33260552Ff4328b1E18a43`, `dlpsCount()` = 44, `dlpRegistrationDepositAmount()` = 0.
- VanaEpoch `0x2063cFF0609D59bCCc196E20Eb58A8696a6b15A0`, `epochsCount()` = 7, `epochRewardAmount()` = 75,000 VANA.
- VanaPoolStaking `0x641C18E2F286c86f96CE95C8ec1EB9fC0415Ca0e`.
- DLPRewardDeployer `0xEFD0F9Ba9De70586b7c4189971cF754adC923B04`.

Proof of contribution is genuinely on chain. `contracts/data/dataRegistry/DataRegistryImplementation.sol` exposes permissionless `addFile` variants and records `addProof(uint256 fileId, Proof memory proof)` including `dlpId`, `score` and `proofUrl`. `contracts/data/teePool/TeePoolImplementation.sol` assigns a Trusted Execution Environment via `requestContributionProof` and only the assigned active node writes the proof back through `addProof(...) external override onlyActiveTee`. The 20,073,985 registered files and 17,779 TEE jobs are hard on chain evidence of real usage.

Upgrade authority is a multisig, not a single key. Every implementation gates `_authorizeUpgrade(address) internal override onlyRole(DEFAULT_ADMIN_ROLE)`. Reading role history and current state on chain, the current sole `DEFAULT_ADMIN_ROLE` holder on ALL six core proxies is a 3 of 7 Gnosis Safe v1.4.1 at `0x5eca5208f29e32879a711467916965b2d753baf4` (`getThreshold()` = 3, `getOwners()` returns 7 owners, `VERSION()` = `1.4.1`, `nonce()` = 59). The `RoleGranted` and `RoleRevoked` logs show the deployer EOA `0x2ac93684679a5bda03c6160def908cdb8d46792f` and two intermediate keys were granted then revoked (revocations at blocks 4467520 and 5422426), so no live single EOA controls upgrades.

Emissions are treasury funded and per epoch capped, not freshly minted. `contracts/dlpRewards/vanaEpoch/VanaEpochImplementation.sol` enforces `if (totalRewardAmount > epoch.rewardAmount) revert EpochRewardExceeded(...)` and requires near full allocation. `contracts/dlpRewards/dlpRewardDeployer/DLPRewardDeployerImplementation.sol` pays rewards with `treasury.transfer(...)` then `dlpRewardSwap.splitRewardSwap{value: trancheAmount}(...)` under `distributeRewards(...) onlyRole(REWARD_DEPLOYER_ROLE)`. There is no uncapped VANA `mint` in the core contracts.

The DataDAO token template does have owner minting. `contracts/dlpTemplates/dat/DAT.sol` is OpenZeppelin `ERC20Capped` plus `ERC20Votes` plus `ERC20Burnable`, with `mint(address,uint256) onlyRole(MINTER_ROLE)`, a cap that becomes `type(uint256).max` when initialized to 0, plus an admin blocklist and pausable transfers. This is per DataDAO token centralization, deployed as clones by `DATFactoryImplementation.sol`.

### Claim vs reality

1. User owned data DAOs that pool data on chain. CONFIRMED IN CODE and on chain. DLPRegistry holds 44 DLPs, DataRegistry holds 20,073,985 files registered through permissionless `addFile`, and DataDAO tokens are minted by the `DATFactory` clones.

2. Proof of contribution validates contributions via TEEs on chain. CONFIRMED IN CODE, with a caveat. TeePool plus `DataRegistry.addProof` implement it and 17,779 TEE jobs have run. Caveat: `DataRegistry.addProof` itself carries only `whenNotPaused` and no caller restriction, so the on chain function that stores a score is not strictly limited to TeePool, which OVERSTATES the strength of that specific guard even though the intended flow routes through TeePool.

3. VANA is the native token of the Vana L1 with a 120M max supply. CONFIRMED. Canonical VANA is native on chain `1480`; the Ethereum and other chain versions are bridged representations holding only tiny balances. Any framing of the Ethereum contract as the canonical token would be OVERSTATED.

4. DLP rewards and VANA emissions run through epochs. CONFIRMED IN CODE. VanaEpoch caps each epoch at `epochRewardAmount` (currently 75,000 VANA, 7 epochs recorded) and DLPRewardDeployer pays from treasury. Caveat: there is no global lifetime cap inside VanaEpoch and admin can change the per epoch amount, so a claim of fixed or immutable emissions would be OVERSTATED. The 120M ceiling is a protocol level property, not enforced by these reward contracts.

5. VANA staking. CONFIRMED IN CODE. `VanaPoolStakingImplementation.sol` stakes native VANA with a bonding period, slippage guard and forfeiture of rewards on early exit, payouts from the pool treasury.

6. Decentralized, non custodial and trustless. OVERSTATED. All core logic is upgradeable by one 3 of 7 Gnosis Safe, DLP verification and eligibility are gated by `MAINTAINER_ROLE`, and the Ethereum bridge mint authority is another multisig. This is credible multisig governance with good key hygiene, but it is a trust assumption, not trustlessness.

7. Open source and audited. CONFIRMED. MIT license, `vana-com/vana-smart-contracts`, with Hashlock and Nethermind reports committed in the repo.

### Severity

High (1)
- Universal upgradeability and cross chain mint authority concentrate power in multisigs. The 3 of 7 Safe `0x5eca5208...` can replace the logic of every core contract on Vana L1, and a separate 3 of N Safe `0xddf71a...` can mint the bridged VANA on Ethereum. Mitigated by the 3 of 7 threshold and by verified revocation of the deployer and intermediate EOA keys, but it remains the single most material risk.

Medium (3)
- `DataRegistry.addProof` has no caller access control beyond `whenNotPaused`, so any address can attach an arbitrary score, dlpId and proofUrl to any fileId at the registry level.
- The `DAT` DataDAO token template grants the deploying owner both admin and `MINTER_ROLE`, defaults the cap to unlimited when set to 0, and adds an admin blocklist and pausable transfers, which is meaningful centralization at the per DataDAO token level.
- Liquidity and float risk. Volume is centralized exchange dominated at about $3.5M in 24h with thin on chain DEX depth, and MC to FDV of about 0.26 signals a large unlock overhang.

Low (2)
- Production hygiene. `VanaEpochImplementation.sol` ships with `import "hardhat/console.sol"` and a leftover `error Test(uint256,uint256)`, and `DLPRewardDeployerImplementation.sol` carries commented out dead rollover code.
- DLP verification and reward eligibility are centralized under `MAINTAINER_ROLE` in DLPRegistry, so a maintainer decides which DataDAOs become reward eligible.

Informational (2)
- Documentation is fragmented after a site revamp toward data portability, the README has no address table and contains an accidental AI chat paste artifact, while the authoritative addresses live in `deployments-official`.
- VANA on Ethereum, Base, BNB, Polygon, Arbitrum and Optimism is a bridged representation sharing one address across chains and holding only about 74,186 VANA; the canonical supply is the native coin on Vana L1.
