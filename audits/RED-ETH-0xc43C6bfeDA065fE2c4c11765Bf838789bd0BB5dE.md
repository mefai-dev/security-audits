# RedStone (RED): Whitepaper Claims vs Code Reality

**Score: 80/100, RISK: MEDIUM**

Date: 2026-08-05
Auditor: MEFAI Security (ICE/ION deep source audit, read only, public sources)

Token (live, independently confirmed via public RPC):
- Ethereum canonical: `0xc43C6bfeDA065fE2c4c11765Bf838789bd0BB5dE`, verified contract name `RedstoneToken`, name "Redstone", symbol RED, 18 decimals, totalSupply 597,000,000 RED. Not a proxy. Mintable by `minter()` = `0x5754e3ef8c88eC7A5DDC0eED1803da1C991ab397` (a Gnosis Safe multisig, `getThreshold()` returns 3).
- Base representation: `0x4eb92702ba4cfbf80561bad64d89c706ac824960`, symbol RED, 18 decimals, totalSupply 1,532,652 RED (bridged). This is an upgradeable `EIP-1967` proxy (implementation `0xe674be7f...`), `owner()` = the same Safe `0x5754e3ef...`, `minter()` = `0x84b4ecC4...e982` (Wormhole NTT manager).
- Market: about USD 0.13, market cap roughly USD 48M, circulating around 326M to 460M, max 1B. Listed on Binance (Launchpool project), Coinbase, and others. Actively traded and liquid.

Websites: redstone.finance, docs.redstone.finance, blog.redstone.finance
GitHub: github.com/redstone-finance (audited repo: `redstone-oracles-monorepo`, HEAD `f2f2a7b`)

## Severity Summary

| # | Claim | Verdict | Severity |
|---|-------|---------|----------|
| 1 | Pull based oracle: signed data delivered on demand and verified on chain | CONFIRMED IN CODE | Informational |
| 2 | Push feeds with a defined update authority | CONFIRMED IN CODE (nuance) | Low |
| 3 | EigenLayer AVS restaking secures the oracle | OVERSTATED | Medium |
| 4 | Many signers, median aggregation on chain | CONFIRMED IN CODE (source count off chain) | Low |
| C | Centralization: who signs and who can mint | 3 of 5 signer trust, mintable token | Medium |

Confirmed in code: 3. Overstated: 1. False: 0.

## Why This Report Exists

RedStone markets itself as a modular oracle for DeFi and AI that delivers price data through a pull model plus push feeds across more than 110 chains, secured by EigenLayer AVS restaking, protecting a Total Value Secured measured in billions. Those are strong claims. This report reads the actual public Solidity and TypeScript in `redstone-oracles-monorepo`, recovers the live RED token state directly from Ethereum and Base RPC, and grades each flagship claim against the code that actually runs. The goal is precision on the trust model: specifically, who signs the data that consumer contracts accept, and whether restaking is what secures the feeds most integrations use.

## Method

- Cloned `github.com/redstone-finance/redstone-oracles-monorepo` at HEAD `f2f2a7b` and read the on chain verification core (`packages/evm-connector`), the push adapters (`packages/evm-adapters`), the AVS operator package (`packages/restaking`), and the client SDK (`packages/sdk`).
- Queried live state from public RPC (`ethereum-rpc.publicnode.com`, `base-rpc.publicnode.com`): token metadata, total supply, storage slots for `EIP-1967`, bytecode function selectors, and the `minter`/`owner` addresses, then classified those addresses as contract or EOA and probed the Safe `getThreshold`.
- Cross checked market and integration facts against public explorers and aggregators. No private data, no team analysis, read only.

## The Foundation: A Genuine On Chain Signature Verifier

RedStone's design is unusual and, in the core, genuinely trust minimized. Prices are not written to storage by a privileged writer for the pull product. Instead, signed data packages are appended to the calldata of an ordinary user transaction, and the consumer contract recovers and verifies the signatures at read time. The verification code is real and public; the consumer bases are licensed `BUSL-1.1`, while the core `SignatureLib` and `NumericArrayLib` primitives that perform the signature recovery and median are MIT. The entire security argument reduces to one question that the code answers plainly: which signer keys are authorized, and how many are required. For the flagship `redstone-primary-prod` service the answer is 5 authorized keys with a threshold of 3, checked on chain. That is the true trust anchor, and it matters more than any restaking narrative.

## Claim 1: Pull based oracle, signed data verified on chain

CLAIM: "genuine pull based oracle where signed price data is delivered on demand and verified on chain."

REALITY: CONFIRMED IN CODE. The consumer base extracts each data package from calldata, recovers the ECDSA signer, requires the signer to be authorized, enforces a unique signer threshold, validates the timestamp, and aggregates by median. Signature malleability is guarded (v in {27,28}, s below secp256k1n/2, reject zero address).

EVIDENCE:
```
packages/evm-connector/contracts/libs/SignatureLib.sol:35-46
if (v != RECOVERY_ID_27 && v != RECOVERY_ID_28) { revert InvalidSignature(signedHash); }
if (uint256(s) > HALF_CURVE_ORDER) { revert InvalidSignature(signedHash); }
signerAddress = ecrecover(signedHash, v, r, s);
if (signerAddress == address(0)) { revert InvalidSignature(signedHash); }
```
```
packages/evm-connector/contracts/core/RedstoneConsumerBase.sol:238-243
// Verifying the off chain signature against on chain hashed data
signerAddress = SignatureLib.recoverSignerAddress(signedHash, calldataNegativeOffset + SIG_BS);
signerIndex = getAuthorisedSignerIndex(signerAddress); // reverts if signer not authorised
```
```
packages/evm-connector/contracts/core/RedstoneConsumerBase.sol:318-322
if (uniqueSignerCountForDataFeedIds[dataFeedIndex] < uniqueSignersThreshold) {
  revert InsufficientNumberOfUniqueSigners(uniqueSignerCountForDataFeedIds[dataFeedIndex], uniqueSignersThreshold);
}
```
```
packages/evm-connector/contracts/core/RedstoneDefaultsLib.sol:12-13,27-33
uint256 constant DEFAULT_MAX_DATA_TIMESTAMP_DELAY_SECONDS = 3 minutes;
uint256 constant DEFAULT_MAX_DATA_TIMESTAMP_AHEAD_SECONDS = 1 minutes;
// reverts TimestampIsTooOld / TimestampFromTooLongFuture outside that window
```

IMPACT: The pull oracle is real. A caller cannot inject a price unless it carries valid signatures from at least 3 distinct authorized signers, all sharing one timestamp inside a tight freshness window, and a per signer bitmap prevents one key from being counted twice. This is a legitimate on chain verifier, not a facade around a trusted setter.

## Claim 2: Push feeds and their update authority

CLAIM: RedStone also runs push "Classic" feeds where a relayer repeatedly writes prices to storage adapters.

REALITY: CONFIRMED IN CODE, with an important nuance about authority. The push adapters do exist (`RedstoneAdapterBase`, `MultiFeedAdapterWithoutRounds`) and they write validated values to storage. However the write path is not gated by a privileged relayer key in the open source contracts. `requireAuthorisedUpdater` defaults to a no op, and no production adapter in the repository overrides it. Security is instead inherited from Claim 1: the values written must themselves carry the 3 of 5 signatures, must be strictly newer than the last update, and must respect a minimum interval.

EVIDENCE:
```
packages/evm-adapters/contracts/core/RedstoneAdapterBase.sol:58-60
function requireAuthorisedUpdater(address updater) public view virtual {
  // By default, anyone can update data feed values, but it can be overridden
}
```
```
packages/evm-adapters/contracts/core/RedstoneAdapterBase.sol:93-104
function updateDataFeedsValues(uint256 dataPackagesTimestamp) public virtual {
  requireAuthorisedUpdater(msg.sender);
  _assertMinIntervalBetweenUpdatesPassed();          // MIN_INTERVAL_BETWEEN_UPDATES = 3 seconds
  validateProposedDataPackagesTimestamp(dataPackagesTimestamp); // must be newer than before
  ...
  uint256[] memory oracleValues = getOracleNumericValuesFromTxMsg(dataFeedsIdsArray); // signature check
```
Grep across the repository for overrides returned only the base definition and the interface, no production restriction:
```
grep -rn "requireAuthorisedUpdater" packages --include=*.sol
  RedstoneAdapterBase.sol (definition + call site), IRedstoneAdapter.sol (interface)  -> no override
```

IMPACT: The push feed authority is, by contract design, permissionless relay over signer authenticated data. That is a favorable property: no single "updater" key can post an unsigned or stale price. The residual trust is again the signer set, not the relayer. In practice RedStone operates the relayer service (`packages/on chain-relayer`, off chain TypeScript), and an integrator can optionally restrict the updater by overriding the hook, but the shipped contracts do not privilege any address.

## Claim 3: EigenLayer AVS restaking secures the oracle

CLAIM: "RedStone AVS ... leverages restaking ... securing up to USD 14 billion in economic security," with node slashing of ETH and RED for malicious data.

REALITY: OVERSTATED. The restaking exists as an operational deployment on the third party Othentic framework, but it is not the mechanism that secures the mainstream feeds, and RedStone's own repository contains no on chain AVS contracts. The entire `packages/restaking` tree is operator run configuration and marketing metadata, not slashing or service manager Solidity.

EVIDENCE:
```
packages/restaking/mainnet/  (full contents)
  avs-metadata.json         <- marketing description only
  redstone-logo.png
  operator/docker-compose.yml
  operator/operator{1,2,3}-metadata.json
  operator/README.md
```
```
packages/restaking/mainnet/operator/docker-compose.yml:6-18
image: public.ecr.aws/y7v2w8b2/avs-othentic-client:45e26119
command: ["node","attester", ... "--l1-chain","mainnet","--l2-chain","base", ...]
```
There are zero AVS or slashing `.sol` files in `packages/restaking` (RedStone does ship native, feed unwired token locking and dispute slashing elsewhere, `LockingRegistry.sol` and `DisputeResolutionEngine.sol` in `packages/eth-contracts`, but these are a RedStone token mechanism, not EigenLayer, and are not imported by any price feed consumer):
```
find packages/restaking -name '*.sol'   ->   (no results)
```
Meanwhile the contract that the flagship integrations actually import still trusts a fixed 5 key set with no restaking dependency:
```
packages/evm-connector/contracts/data-services/PrimaryProdDataServiceConsumerBase.sol:8-14
getDataServiceId() -> "redstone-primary-prod"
getUniqueSignersThreshold() -> 3
```

IMPACT: The AVS is a real, separately operated data path built on Othentic and EigenLayer contracts that are external to this codebase. It is not what backstops the classic pull and push feeds that most of RedStone's Total Value Secured relies on. Marketing that presents billions of restaked cryptoeconomic security as the protection for RedStone feeds overstates the role restaking plays in the trust model a typical integrator inherits today, which remains a 3 of 5 signature check. This is an overstatement of scope, not a fabrication: the AVS operators, P2P attester network, and mainnet operator configuration are genuinely present.

## Claim 4: Many data sources and on chain aggregation

CLAIM: RedStone aggregates a large number of independent data sources into each feed.

REALITY: CONFIRMED IN CODE for the aggregation method and the signer level redundancy; the raw source count is an off chain property that is not contained in this repository. On chain, values from the required distinct signers are combined by median. The registry that defines signers is public and shows the primary prod service growing beyond the original operators.

EVIDENCE:
```
packages/evm-connector/contracts/libs/NumericArrayLib.sol:15-30
function pickMedian(uint256[] memory arr) internal pure returns (uint256) {
  ... sort(arr); ... return arr.length % 2 == 0 ? (arr[m-1]+arr[m])/2 : arr[m];
}
```
```
packages/sdk/src/registry/initial-state.json  (redstone-primary-prod nodes, on RPC-confirmed keys)
0x8BB8F32Df04c8b654987DAaeD53D6B6091e3B774  altair    (2024-01-01)
0xdEB22f54738d54976C4c0fe5ce6d408E40d88499  wayfarer  (2024-01-01)
0x51Ce04Be4b3E32572C4Ec9135221d0691Ba7d202  morpheus  (2024-01-01)
0xDD682daEC5A90dD295d14DA4b0bec9281017b5bE  ciri      (2024-01-01)
0x9c5AE89C4Af6aA32cE58588DBaF90d18a855B6de  node-5    (2024-01-01)
+ 2025 external operators: kaiko, keyrock, auros, alchemy, teb, undefined-labs, kudasaijp
```
The off chain fetchers that scrape exchanges and aggregators (the "hundreds of sources" figure) live in RedStone's separate oracle node, not in this monorepo. This monorepo ships only the client side median and gateway routing (`packages/sdk/compute-median.ts`, `fetch-data-packages.ts`, `data-services-urls.ts`), and all read gateways are RedStone operated (`oracle-gateway-*.a.redstone.finance`).

IMPACT: On chain, a feed is the median of at least 3 independent signer submissions, which resists a single misbehaving or compromised source. The per source breadth beneath each signer is credible from RedStone's public reputation but cannot be verified from the audited source here, so it should be treated as an operational claim rather than a code guarantee.

## Centralization

- Who signs the data: the on chain flagship base `PrimaryProdDataServiceConsumerBase` hardcodes 5 RedStone registered signer addresses and requires 3 of them. This 3 of 5 signer trust is the real security boundary for the pull and push products. The public registry lists roughly 22 nodes under `redstone-primary-prod`, including reputable 2025 additions (Kaiko, Keyrock, Auros, Alchemy), which is meaningful decentralization progress on the gateway side, but the canonical on chain verifier most integrations import still recognizes the original 5.
- Relayer and update authority: permissionless by contract default, signature gated. No privileged pusher can inject unsigned data. RedStone runs the relayer operationally.
- Upgradeable proxies: the pull consumer bases are inherited libraries rather than standalone upgradeable contracts; push adapters are commonly deployed behind proxies by integrators. On the token side, the Base RED contract is an upgradeable `EIP-1967` proxy owned by the RedStone Safe.
- Token owner and mint: the Ethereum canonical `RedstoneToken` is not upgradeable and exposes no `owner`, `AccessControl`, `pause`, or `burn`, but it does expose `mint(address,uint256)` restricted to `minter()` = a Gnosis Safe multisig with threshold 3 (`0x5754e3ef...`). Supply is therefore not hard capped in code; the multisig can mint. The same Safe owns the upgradeable Base token, whose minter is a Wormhole NTT manager. Control concentrates in that one multisig, which is a reasonable posture (multisig, not an EOA) but is a centralization vector worth stating plainly.

## Positive Findings

- Real, auditable on chain ECDSA verification with signature malleability protection and zero address rejection.
- Unique signer threshold with a per data feed bitmap prevents a single key from satisfying the quorum, and median aggregation resists a single outlier signer.
- Freshness is enforced three ways: bounded timestamp window, strictly monotonic data timestamps on push, and a minimum interval between updates.
- The push write path privileges no relayer key in the shipped contracts, so there is no trusted setter that can post unsigned prices.
- Ethereum canonical token is verified and non upgradeable, and its mint authority is a multisig rather than a single key.
- Massive and verifiable product reality: roughly USD 8B to 10B Total Value Secured, 170 plus protocol integrations (Morpho, Spark, Ethena, Pendle, Compound and others), broad multichain coverage, and a liquid, Binance and Coinbase listed token.
- Genuine decentralization trajectory: professional external signers added through 2025.

## Conclusion

RedStone is a real, heavily integrated oracle whose flagship pull based design is not marketing gloss but working, public, trust minimized Solidity. Signed data is verified on chain, quorum and freshness are enforced, aggregation is a median, and the push path adds no privileged writer. The token is live, verified, and liquid. The precise trust model, however, is narrower than the messaging implies: security rests on a 3 of 5 set of RedStone registered signer keys, not on restaking. The EigenLayer AVS is genuinely deployed on the Othentic framework but is a separate, parallel path with no on chain contracts in this repository, and it does not currently backstop the classic feeds that carry most of the secured value, so the restaking security claim is overstated. Token supply is mintable by a threshold 3 multisig and the Base representation is an upgradeable proxy, standard but worth noting. Net assessment: a strong and honest core with one overstated security narrative and ordinary oracle centralization. Score 80 of 100, Passed, risk MEDIUM.
