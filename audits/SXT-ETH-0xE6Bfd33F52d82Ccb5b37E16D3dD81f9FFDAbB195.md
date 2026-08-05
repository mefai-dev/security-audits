# Space and Time (SXT): Whitepaper Claims vs Code Reality

**Score: 80/100, LOW to MEDIUM RISK**

**Date:** 2026-08-05
**Token:** SXT (ERC20, Ethereum mainnet `0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195`, fixed 5,000,000,000 supply, 18 decimals, non mintable, pausable)
**Chain:** SXT Chain (Substrate / polkadot-sdk, BABE + GRANDPA + NPoS staking)
**Websites:** spaceandtime.io, docs.spaceandtime.io
**GitHub:** github.com/spaceandtimefdn (sxt-proof-of-sql, sxt-node, sxt-token, sxt-node-op-contracts, blitzar, sxt-dory)

---

## Severity Summary

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 3 |
| Low | 3 |
| Informational | 3 |

---

## Why This Report Exists

Most of the reports we publish document projects whose code contradicts their marketing. Space and Time is the other kind of subject: a project that has shipped a large body of genuine, sophisticated cryptography in the open. The discipline is the same. We read the actual public source, claim by claim, credit what is truly implemented, and flag where the marketing language runs ahead of the code or where the trust model is more centralized than the words "decentralized" and "trustless" imply.

We are not making accusations. We are not spreading FUD. We read the source and show what is actually implemented. Space and Time earns real credit here: Proof of SQL is a genuine, high quality zero knowledge argument system with a sound prover and verifier, the SXT Chain is a real Substrate chain with genuine BFT consensus and staking, and the SXT token is a genuine fixed supply asset that we verified live on chain. The caveats that remain are honest ones about maturity and centralization, and they matter for anyone who reads "decentralized verifiable database" literally.

## Method

For every major claim we located the relevant code in the real repositories (sxt-proof-of-sql, sxt-node, sxt-token, sxt-node-op-contracts), fetched raw files from GitHub, and read what is actually implemented. We additionally verified the token live on Ethereum mainnet with `eth_call`. Each claim is labelled CONFIRMED IN CODE, OVERSTATED, or FALSE, with a repository path, file, line, and a short verbatim snippet.

---

## The Foundation: A Genuine Open Source Cryptography Stack, Recently Launched

**CLAIM:**
> Space and Time is building a decentralized, verifiable data warehouse powered by an original zero knowledge prover.

**REALITY:** CONFIRMED, with a maturity caveat. Unlike the rebranded forks we usually review, the core of Space and Time is real, original, and substantial. The Proof of SQL prover (`sxt-proof-of-sql`, ~5,400 stars) is a large Rust codebase implementing a full ZK argument system from primitives up: a sumcheck protocol, the Dory transparent polynomial commitment scheme, a HyperKZG commitment scheme, inner product arguments, and Keccak256 Fiat Shamir transcripts. GPU acceleration lives in a separate C++ library (`blitzar`). The chain (`sxt-node`) is a real polkadot-sdk runtime. The honest caveat is age: the token, chain, and staking contracts were all first published on 2025-04-29 and the network runs `mainnet-v1.33.x`, so this is an early stage deployment, not a battle tested one.

**EVIDENCE:**
```
sxt-proof-of-sql/crates/proof-of-sql/src/proof_primitive/
  sumcheck/proof.rs, dory/*.rs, hyperkzg/*.rs, inner_product/inner_product_proof.rs
sxt-node/Cargo.toml  -> polkadot-sdk = "0.9.0", 25+ workspace crates incl. pallets/{staking-via-runtime,rewards,zkpay,permissions,commitments,tables}
sxt-token created 2025-04-29 ; sxt-node mainnet image ghcr.io/spaceandtimefdn/sxt-node:mainnet-v1.33.1
```

**IMPACT:** Positive, with a time caveat. The foundation is genuine engineering, not a cosmetic fork. Treat every confirmation below as real, and every centralization caveat as the normal profile of a young network still under a founding operator. Informational.

---

## Claim 1: Proof of SQL cryptographically guarantees SQL query results (real prover and verifier)

**CLAIM:**
> "Proof of SQL cryptographically guarantees SQL queries were computed accurately against untampered data." A sub second ZK prover with both onchain and offchain verification.

**REALITY:** CONFIRMED IN CODE. The prover and verifier are both fully implemented and cryptographically coherent. `QueryProof::new` builds a two round protocol: it commits to intermediate multilinear extensions (MLEs), binds everything into a Keccak256 Fiat Shamir transcript, runs a sumcheck over the query constraint subpolynomials, and produces a polynomial commitment evaluation proof. `QueryProof::verify` independently rebuilds the identical transcript, verifies the sumcheck, checks the result MLE evaluations, checks the sumcheck evaluation, and finally verifies the batched commitment evaluation proof, returning a hard `VerificationError` on any mismatch. This is a genuine succinct argument, not a stub.

**EVIDENCE:**
```rust
// sxt-proof-of-sql/crates/proof-of-sql/src/sql/proof/query_proof.rs:152-233  prove
let mut transcript: Keccak256Transcript = Transcript::new();
...
let state = make_sumcheck_prover_state(final_round_builder.sumcheck_subpolynomials(), ...);
// L305: let evaluation_proof = CP::new(&mut transcript, &folded_mle, &evaluation_point, ...);
```
```rust
// query_proof.rs:327 verify(...) ; the checks are enforced, not decorative:
// L425:  let subclaim = self.sumcheck_proof.verify_without_evaluation(&mut transcript, ...)?;
// L507:  if verifier_evaluations.column_evals() != result_evaluations { Err("result evaluation check failed") }
// L516:  if builder.sumcheck_evaluation() != subclaim.expected_evaluation { Err("sumcheck evaluation check failed") }
// L532:  self.evaluation_proof.verify_batched_proof(...).map_err(|_e| "Inner product proof of MLE evaluations failed")?;
```
The commitment layer is real and transparent (no trusted setup for the default Dory scheme):
```
proof_primitive/dory/dory_commitment_evaluation_proof.rs, setup.rs, public_parameters.rs
proof_primitive/sumcheck/proof.rs (prover_round.rs, prover_state.rs)
```

**IMPACT:** Positive. This is the single strongest confirmation in the report. The flagship claim, that Proof of SQL produces a real cryptographic proof that a SQL result was computed correctly against committed data, is genuinely implemented and the verifier genuinely rejects bad proofs. Informational.

---

## Claim 2: The proofs are "zero knowledge"

**CLAIM:**
> Proof of SQL is a "zero knowledge (ZK) prover." The ZK branding implies the data is kept private.

**REALITY:** OVERSTATED (as a privacy claim). The system is a succinct, verifiable computation argument. Its guarantee is soundness and integrity: the query was executed correctly against the data that was committed. It is not a confidentiality technology in the everyday sense of "zero knowledge equals your data stays hidden." The column commitments the verifier consumes are public on chain state, and the query result is revealed in cleartext to the verifier (the verifier is handed `result: OwnedTable` and checks it). The default Dory path and the examples do not blind the underlying table from a party that holds the commitments and result. "ZK" here is accurate in the academic sense of a succinct argument built on commitments and Fiat Shamir, but it should not be read as "the prover hides the data."

**EVIDENCE:**
```rust
// query_proof.rs:327-333  the verifier is given the plaintext result and the public commitments
pub fn verify(self, expr, accessor: &impl CommitmentAccessor<CP::Commitment>,
              result: OwnedTable<CP::Scalar>, setup, params) -> QueryResult<CP::Scalar>
// L505: let result_evaluations = result.mle_evaluations(&subclaim.evaluation_point);
```
```
// README: "cryptographically guarantees SQL queries were computed accurately" -> integrity, not privacy
// commitments are stored as public on chain state (sxt-node/pallets/commitments)
```

**IMPACT:** The property delivered is verifiable correctness (you can trust an untrusted operator computed the query honestly), which is exactly what the DeFi and smart contract use cases need. It is not input privacy. Readers should take "ZK" as "provably correct," not "confidential." Low.

---

## Claim 3: SXT Chain reaches BFT consensus with validator staking and slashing

**CLAIM:**
> "SXT Chain validators witness inserts via BFT consensus" and secure the network as a decentralized validator set.

**REALITY:** CONFIRMED IN CODE. The runtime is a standard, genuine polkadot-sdk nominated proof of stake stack: BABE for block production, GRANDPA for BFT finality, `pallet_staking` for validator and nominator (delegator) staking with a Phragmen election, real bonding and unbonding periods, offence and equivocation reporting, and slashing. This is substantive protocol engineering, not a claim without code.

**EVIDENCE:**
```rust
// sxt-node/runtime/src/lib.rs
// L103-147 construct_runtime: pallet_babe, pallet_grandpa, pallet_session, pallet_staking, pallet_offences, ...
// L573 impl pallet_staking::Config for Runtime { ... }
// L562-568 SessionsPerEra=24, BondingDuration=7 eras, SlashDeferDuration=6, OffendingValidatorsThreshold=17%
// L450 pallet_grandpa::EquivocationReportSystem (slashing on equivocation)
// L536 impl pallet_staking::EraPayout ... (staking reward emission)
```

**IMPACT:** Positive. The consensus, staking, election, and slashing the whitepaper describes exist in the deployed runtime. Informational.

---

## Claim 4: SXT Chain is a live, decentralized database

**CLAIM:**
> "SXT Chain is the decentralized validator set ... a decentralized database" that is "trustless."

**REALITY:** OVERSTATED. The consensus design is decentralized, but the operating reality is a young, operator gated network with several central control points:

1. A `pallet_sudo` root superuser exists and is the admin origin for staking. Root can force eras, override permissions, and administer the chain.
2. A `pallet_permissions` role based access control layer gates privileged on chain actions (creating and updating tables, inserts, permission changes). Permissions are granted by root or by an account already holding `UpdatePermissions`.
3. Validator and staker onboarding is not native to the chain. It runs through Ethereum L1 contracts owned by Space and Time, and is bridged onto the chain. The public docs list six bootnodes, all "hosted by Space and Time and trusted partners."

**EVIDENCE:**
```rust
// runtime/src/lib.rs:116,492 pallet_sudo ; L592 type AdminOrigin = frame_system::EnsureRoot<Self::AccountId>; // Admin is sudo
```
```rust
// pallets/permissions/src/lib.rs:88-100 set_permissions(...)
Self::ensure_root_or_permissioned(origin.clone(), &PermissionLevel::UpdatePermissions)?;
```
```markdown
// sxt-node/docs/mainnet.md
"The new Space and Time Mainnet does not use Substrate keys. Stakers and Node operators will only use
 Ethereum keys ... to interact with Space and Time's Staking and Messaging contracts. Transactions ...
 will then be reflected on chain."
"The six bootnodes listed below are hosted by Space and Time and trusted partners"
"You need at least 100 SXT in order to onboard a validator node."
```

**IMPACT:** The chain is decentralizable by design and centrally governed in practice today: a root key, an RBAC permission gate, an L1 staking contract set owned by the founding entity, and a small operator run bootnode set. "Decentralized database" is the roadmap and the architecture, not yet the operational posture. Medium.

---

## Claim 5: SXT is the validator and delegator staking asset

**CLAIM:**
> SXT is staked by validators and delegated (nominated) by holders to secure the chain.

**REALITY:** CONFIRMED IN CODE. Staking exists in two coordinated layers. On the chain, `pallet_staking` provides validator and nominator staking with a fixed nomination quota. On Ethereum L1, the `sxt-node-op-contracts` repository ("SXT staking contracts, for staking SXT tokens, nominating validators and unstaking") holds the actual SXT and mirrors stake and nominations onto the chain through a messaging contract and a Substrate signature validator.

**EVIDENCE:**
```rust
// runtime/src/lib.rs:583 type NominationsQuota = pallet_staking::FixedNominationsQuota<MAX_QUOTA_NOMINATIONS>; // 16
// L807 impl pallet_session::Config ... ValidatorIdOf = pallet_staking::StashOf<Self>
```
```solidity
// sxt-node-op-contracts/src/  Staking.sol, StakingPool.sol, CollaborativeStaking.sol,
//   CollaborativeStakingFactory.sol, SXTChainMessaging.sol, SubstrateSignatureValidator.sol
// Staking.sol:13 contract Staking is IStaking, Ownable, Pausable
```

**IMPACT:** Positive. SXT is genuinely the staking and nomination asset, exactly as marketed. The caveat, that the L1 staking contract is `Ownable` and `Pausable`, is covered in Claim 8. Informational.

---

## Claim 6: SXT is the gas that pays for chain queries and table updates

**CLAIM:**
> "SXT gas is spent by clients of the chain to create and update tables" and to pay for queries.

**REALITY:** OVERSTATED (as an SXT exclusive toll). Query and table payment is real and implemented through the ZKPay system: an L1 ZKPay contract emits payment events (`SendPayment`, `NewQueryPayment`, `PaymentSettled`, `QueryFulfilled`) that the chain ingests and settles in `pallet_zkpay`. But payment is explicitly multi asset. ZKPay maintains a registry of supported assets, each with its own Chainlink style price feed and staleness threshold, and the supported asset set, treasury, and price feeds are all configured by root. So a client can pay query fees in whatever assets the operator has enabled (priced via oracle), not necessarily in SXT. The precise statement is that SXT is a supported and intended payment and staking asset, not the sole mandatory unit for every query.

**EVIDENCE:**
```rust
// sxt-node/pallets/zkpay/src/lib.rs
// L111-123 Asset { allowed_payment_types (bitmask), price_feed, stale_price_threshold_in_seconds }
// L136-197 set_supported_asset / set_treasury / remove_supported_asset -> all ensure_root(origin)?
// L203-229 process_send_payment / process_new_query_payment / process_payment_settled / process_query_fulfilled
```

**IMPACT:** Query payment is genuinely implemented, but it is an oracle priced, multi asset, admin configured fee system. "SXT gas pays for everything" is true as an intention and false as an exclusivity claim. Medium.

---

## Claim 7: SXT is a fixed 5,000,000,000 supply token that can never be minted

**CLAIM:**
> SXT has a fixed supply of 5 billion, with no minting.

**REALITY:** CONFIRMED IN CODE for the Ethereum ERC20, with one honest nuance for the chain. The `SpaceAndTime` contract mints the entire 5,000,000,000 supply once in its constructor and exposes no `mint` function anywhere, so the L1 token is genuinely fixed and non inflationary. We verified the deployed contract live on Ethereum mainnet. The nuance is that the SXT Chain native token is a distinct, inflationary balance: `pallet_staking` mints new native units as staking rewards at roughly 9.7 percent of total staked per year. So "no SXT is ever minted" is accurate for the L1 contract but not for the protocol as a whole.

**EVIDENCE:**
```solidity
// sxt-token/src/SpaceAndTime.sol:16-22
constructor(address defaultAdmin, address pauser, address recipient) ... {
    _grantRole(DEFAULT_ADMIN_ROLE, defaultAdmin);
    _grantRole(PAUSER_ROLE, pauser);
    _mint(recipient, 5_000_000_000 * 10 ** decimals());   // only mint, in the constructor; no mint() function exists
}
```
Live on chain verification (Ethereum mainnet `0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195`, via `eth_call`):
```
totalSupply() = 0x1027e72f1f12813088000000 = 5,000,000,000 x 10^18   (fixed)
decimals()    = 18
symbol()      = "SXT"
paused()      = false
PAUSER_ROLE() = 0x65d7a28e...440d862a  (== keccak256("PAUSER_ROLE"), confirms the pausable role-gated contract)
```
Chain side inflation (distinct native token):
```rust
// sxt-node/runtime/src/lib.rs:548-551
let base_rate = FixedU128::from_rational(97, 1000);        // ~9.7% / year
let yearly_emission = base_rate.saturating_mul_int(total_staked);
// L590 type Reward = (); // Rewards are minted not transfered
```

**IMPACT:** The tradeable L1 SXT asset is genuinely fixed at 5 billion and cannot be minted, which we confirmed on chain. Anyone reading "fixed supply, no mint" as covering all SXT everywhere should note the chain level native staking token emits roughly 9.7 percent annually to reward validators. Low.

---

## Claim 8: The token is a plain, trust minimized ERC20

**CLAIM:**
> SXT is a standard ERC20 token.

**REALITY:** CONFIRMED, and it carries admin powers worth stating plainly. `SpaceAndTime` is `ERC20Pausable` plus `AccessControl` plus `ERC20Votes`. A holder of `PAUSER_ROLE` can pause the token, and while paused every transfer reverts because `_update` routes through `ERC20Pausable`. A holder of `DEFAULT_ADMIN_ROLE` controls all role assignments and can grant itself the pauser role. This is a legitimate, disclosed design (the project's own Aderyn static analysis flags it as "L-1: Centralization Risk for trusted owners"), but it means SXT is not an immutable, ungovernable token: a privileged key can freeze all transfers network wide.

**EVIDENCE:**
```solidity
// sxt-token/src/SpaceAndTime.sol:11  contract SpaceAndTime is ERC20, ERC20Pausable, AccessControl, ERC20Permit, ERC20Votes
// L12  bytes32 public constant PAUSER_ROLE = keccak256("PAUSER_ROLE");
// L24-30 function pause() public onlyRole(PAUSER_ROLE) { _pause(); }  // unpause likewise
// L32-34 _update(...) override(ERC20, ERC20Pausable, ERC20Votes)  // paused => every transfer reverts
```
```
// sxt-token/report.md  (project's own audit output)
"L-1: Centralization Risk for trusted owners ... privileged rights to perform admin tasks"
```
The L1 staking contract carries the same posture: `Staking.sol` is `Ownable, Pausable`, starts paused in its constructor, and only the owner can unpause staking or unstaking, so the owner can in principle halt withdrawals.
```solidity
// sxt-node-op-contracts/src/Staking.sol:13 contract Staking is IStaking, Ownable, Pausable
// L113 _pause();  // constructor  ; L228-229 function unpauseUnstaking() external onlyOwner { _unpause(); }
```

**IMPACT:** SXT and its staking contract are governed, pausable instruments controlled by privileged keys. The token is not currently paused (verified on chain), but the power to freeze all transfers, and to freeze unstaking, exists and rests with an admin or owner key. This is standard for a young protocol but it is a real centralization vector, not a trustless one. Medium (token freeze capability), with the staking freeze as a Low.

---

## Additional Note: The prover and the indexed data layer are operated off chain

Worth stating plainly because it frames the whole "decentralized verifiable database" narrative. The chain witnesses and stores cryptographic commitments to tables, and Proof of SQL lets anyone verify a query against those commitments. But the parties that actually index the source data, hold the full tables, and run the (GPU accelerated) Proof of SQL prover are off chain operators, today primarily Space and Time and its indexer and attestor nodes. The on chain guarantee is that a returned result is consistent with the committed data. The availability, completeness, and timeliness of the underlying data, and the operation of the prover, are operator responsibilities, not properties enforced by a permissionless validator set.

**EVIDENCE:**
```
sxt-node/pallets/{indexing,commitments,attestation,prover_db_indexer}  // indexer + attestor + commitment roles
docs/{indexer.md, attestor.md}  // separate indexer and attestor onboarding, operator run
README: prover accelerated on NVIDIA GPUs via Blitzar ; run by data operators, not by chain validators
```

**IMPACT:** The verifiability guarantee (a result matches the committed data) is real and cryptographic. The "decentralized database" guarantee (that no single operator controls the data and prover) is aspirational at this stage. Informational.

---

## Conclusion

Space and Time is a genuine, high quality cryptographic project whose code substantially delivers what it markets. Proof of SQL is a real, sound zero knowledge argument system with a fully implemented prover and a verifier that genuinely rejects invalid proofs (Claim 1). The SXT Chain runs authentic BABE and GRANDPA consensus with real staking, election, and slashing (Claim 3). SXT is the real staking and nomination asset (Claim 5), and the L1 token is a genuinely fixed 5,000,000,000 supply asset with no mint path, which we confirmed live on chain (Claim 7). Nothing in this review is fabricated or a lie. There are zero FALSE findings.

The overstatements are the familiar gap between "decentralized and trustless" marketing and a young network that today leans on central control points: a root `sudo` key, an RBAC permission gate, an L1 staking contract set owned and pausable by the founding entity, a small operator run bootnode set, and a prover and data indexing layer run off chain by Space and Time. The "zero knowledge" branding delivers verifiable correctness rather than data privacy, "SXT gas" is an oracle priced multi asset fee system rather than an SXT only toll, and the token, while fixed on L1, is pausable by an admin and shadowed by an inflationary chain side staking token.

None of this is fraud. It is the difference between a strong, working, openly published cryptography and staking stack in its early operating phase, and the fully decentralized, trustless end state the marketing describes. Score 80 out of 100, LOW to MEDIUM RISK, driven entirely by maturity and centralization caveats rather than by any dishonest or broken code.

| Claim | Verdict |
|-------|---------|
| Proof of SQL is a real ZK prover and verifier for SQL | CONFIRMED IN CODE |
| The proofs are "zero knowledge" (private) | OVERSTATED (verifiable correctness, results and commitments public) |
| SXT Chain reaches BFT consensus with staking and slashing | CONFIRMED IN CODE |
| SXT Chain is a live, decentralized, trustless database | OVERSTATED (sudo, RBAC, L1 owned staking, 6 SxT/partner bootnodes) |
| SXT is the validator and delegator staking asset | CONFIRMED IN CODE |
| SXT is the gas for every query and table update | OVERSTATED (multi asset, oracle priced, admin configured ZKPay) |
| Fixed 5B supply, no mint (L1 ERC20) | CONFIRMED IN CODE (verified on chain; chain side 9.7% staking inflation nuance) |
| Plain, trust minimized token | CONFIRMED, admin pausable (privileged key can freeze all transfers) |

Tally: CONFIRMED IN CODE 5, OVERSTATED 3, FALSE 0.

---

## Verification and Sources (exact repositories and files read)

Proof of SQL (`sxt-proof-of-sql`, branch `main`):
- crates/proof-of-sql/src/sql/proof/query_proof.rs (prove L107-321, verify L327-555; sumcheck verify L425, evaluation-proof verify L532, error checks L507/L516/L532)
- crates/proof-of-sql/src/proof_primitive/sumcheck/{proof.rs, prover_round.rs, prover_state.rs}
- crates/proof-of-sql/src/proof_primitive/dory/{dory_commitment_evaluation_proof.rs, setup.rs, public_parameters.rs}
- crates/proof-of-sql/src/proof_primitive/{hyperkzg/*, inner_product/inner_product_proof.rs}
- crates/proof-of-sql/src/base/proof/{keccak256_transcript.rs, transcript.rs}
- crates/proof-of-sql/README.md

Chain (`sxt-node`, branch `main`):
- runtime/src/lib.rs (construct_runtime; pallet_babe L521, pallet_grandpa L442, pallet_staking L573, EraPayout L536-553, AdminOrigin/sudo L492/L592, pallet_session L807, pallet_permissions L837)
- pallets/permissions/src/lib.rs (set_permissions ensure_root_or_permissioned L88-100)
- pallets/zkpay/src/lib.rs (Asset registry L111-123, ensure_root setters L136-197, payment processing L198-229)
- Cargo.toml (workspace members), README.md, docs/mainnet.md (Ethereum-key staking, 6 bootnodes, 100 SXT onboarding)

Token (`sxt-token`, branch `main`):
- src/SpaceAndTime.sol (ERC20Pausable + AccessControl + ERC20Votes; 5B mint L21; PAUSER_ROLE L12; pause L24-30)
- src/SXTDeployer.sol (defaultAdmin / pauserAdmin config), report.md (Aderyn "L-1: Centralization Risk for trusted owners")

Staking contracts (`sxt-node-op-contracts`, branch `main`):
- src/Staking.sol (Ownable + Pausable, constructor _pause L113, unpauseUnstaking onlyOwner L228)
- src/{StakingPool.sol, CollaborativeStaking.sol, CollaborativeStakingFactory.sol, SXTChainMessaging.sol, SubstrateSignatureValidator.sol}, README.md

On chain (Ethereum mainnet, `eth_call` to a public RPC):
- SXT `0xE6Bfd33F52d82Ccb5b37E16D3dD81f9FFDAbB195`: totalSupply = 5,000,000,000 x 10^18, decimals 18, symbol "SXT", paused false, PAUSER_ROLE = keccak256("PAUSER_ROLE")

Documentation:
- docs.spaceandtime.io, spaceandtime.io (Proof of SQL, SXT Chain, staking, and gas descriptions)

---

## Disclaimer

This report documents the relationship between Space and Time's public marketing and documentation claims and its publicly available open source code, together with one live read of the deployed token on Ethereum mainnet. All findings are based on source read from the spaceandtimefdn GitHub organization and on the project's own documentation. Space and Time is a genuine project with substantial original cryptography; this review credits what the code delivers and flags where marketing language runs ahead of the implementation or where the trust model is more centralized than "decentralized" implies. Read only review, no systems were accessed or modified.

**Report Date:** 2026-08-05
**Website:** https://mefai.io
