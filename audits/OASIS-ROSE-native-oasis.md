# Oasis Network (ROSE): Whitepaper Claims vs Code Reality

**Score: 82/100, LOW to MEDIUM RISK**

**Date:** 2026-08-05
**Token:** ROSE (fixed cap 10,000,000,000; consensus layer 9 decimals, Sapphire EVM 18 decimals)
**Networks:** Oasis consensus layer (CometBFT proof of stake) plus ParaTimes (Sapphire confidential EVM, Cipher, key manager runtime)
**Sapphire wROSE:** 0x8Bc2B030b299964eEfb5e1e0b36991352E56D2D3
**Websites:** oasis.net, docs.oasis.io
**GitHub:** github.com/oasisprotocol (oasis-core, sapphire-paratime, oasis-sdk)

---

## Severity Summary

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 2 |
| Medium | 3 |
| Low | 1 |
| Informational | 3 |

---

## Why This Report Exists

Most of the reports we publish document projects whose code contradicts their marketing. Oasis Network is the other kind of subject: a large, genuinely engineered stack that has shipped a confidential virtual machine, a real hardware attestation pipeline, and an on chain key management service in fully public open source. The discipline is the same. We read the actual source, claim by claim, credit what is genuinely implemented, and flag where the marketing language runs ahead of what the code can promise.

We are not making accusations. We are not spreading FUD. We read the source and show what is actually implemented. Oasis earns real credit on this pass. Sapphire really does encrypt contract state and calldata. The attestation stack really does verify Intel SGX and TDX quotes with genuine cryptography (unlike some competitors that outsource attestation to a closed third party, Oasis ships the DCAP and IAS verification code in the open). The key manager really is a sealed enclave service. ROSE really is a fixed supply asset.

The honest caveats that remain are the ones that always attach to confidential computing. Every privacy and verifiability guarantee in the system ultimately rests on trusting Intel and AMD silicon, plus the set of enclave measurements that governance and runtime owners choose to trust, plus the key manager that holds the master secret from which all Sapphire state keys derive. The frictionless word "trustless" hides those anchors.

---

## Method

For every major marketing and documentation claim we located the relevant code in the real repositories (oasis-core, sapphire-paratime, oasis-sdk), fetched raw files from GitHub, and read what is actually implemented. Each claim below is labelled CONFIRMED IN CODE, OVERSTATED, or FALSE, with a repository path, file, approximate line, and a short verbatim snippet. Line numbers track the current HEAD of branch `master` (oasis-core) and `main` (sapphire-paratime, oasis-sdk) as fetched on 2026-08-05.

---

## The Foundation: A Genuine, Heavily Engineered Layer 1

**CLAIM:**
> "Privacy, verifiability, and composability. For developers who need more than a blockchain can provide." Oasis describes a two layer architecture: a proof of stake consensus layer and a ParaTime layer of parallel runtimes, several of which run inside TEEs.

**REALITY:** CONFIRMED. This is a real, deep codebase, not a fork with a new logo. The consensus and registry logic is tens of thousands of lines of Go under `oasis-core/go/`, the enclave runtime and key manager are Rust under `oasis-core/runtime/` and `oasis-core/keymanager/`, the confidential EVM is Rust under `sapphire-paratime/runtime/` on top of the `oasis-sdk` runtime SDK, and the SGX and TDX attestation stack is implemented from first principles under `go/common/sgx/`. Everything that follows is credit or caveat on a system that genuinely exists.

**IMPACT:** Positive. Unlike the rebranded forks we usually document, Oasis is original engineering across consensus, runtime, attestation, key management, and a confidential EVM. Informational.

---

## Claim 1: Sapphire is a confidential EVM that keeps smart contract state private

**CLAIM:**
> "Onchain Privacy: Confidential smart contracts, encrypt data end to end while maintaining EVM compatibility." "Confidential state, end to end encryption, confidential randomness." The docs state that in a confidential ParaTime "data is decrypted, processed by the smart contract, and then encrypted before it is sent out of the TEE ... never leaked to the node operator or application developer."

**REALITY:** CONFIRMED IN CODE. Sapphire keeps two storage trees. Public contracts write a plaintext store under prefix `0x02`; confidential contracts write a separate store under prefix `0x04` whose keys and values are AEAD sealed with DeoxysII using a per contract key fetched from the key manager. Transaction calldata arrives as an X25519 plus DeoxysII sealed envelope that is opened inside the enclave. Sapphire turns confidentiality on with a single build flag. The plaintext only ever exists inside the running enclave; what lands in the Merkle state tree is ciphertext.

**EVIDENCE:**
```rust
// oasis-sdk runtime-sdk/modules/evm/src/state.rs (~L16-L100)
/// Prefix for Ethereum account storage in our confidential storage.
pub const CONFIDENTIAL_STORAGES: &[u8] = &[0x04];
...
pub fn with_confidential_storage<...>(ctx, address, f) -> R {
    let kmgr_client = ctx.key_manager()
        .expect("key manager must be available to use confidentiality");
    let keypair = kmgr_client.get_or_create_keys(key_id)...;
    let confidential_key = keypair.state_key;              // key from the key manager
    let mut confidential_storages = ConfidentialStore::new_with_key(
        contract_storages, confidential_key.0, ...);
```
```rust
// oasis-sdk runtime-sdk/src/storage/confidential.rs (~L48-L162)
/// A key-value store that encrypts all content with DeoxysII.
pub struct ConfidentialStore<S: Store> { inner: S, deoxys: deoxysii::DeoxysII, ... }
let enc_key   = self.deoxys.seal(&nonce, plain_key, vec![]);    // keys encrypted
let enc_value = self.deoxys.seal(&nonce, plain_value, vec![]);  // values encrypted
let plain = self.deoxys.open(nonce, enc, vec![])                // decrypt on read
    .map_err(|err| Error::DecryptionFailure(err.into()))?;
```
```rust
// sapphire-paratime runtime/src/lib.rs (~L70-L78)
impl module_evm::Config for Config {
    const CHAIN_ID: u64 = chain_id();
    const CONFIDENTIAL: bool = true;   // selects the encrypted store
}
```
```rust
// oasis-sdk runtime-sdk/src/callformat.rs (~L100-L155)
CallFormat::EncryptedX25519DeoxysII => {
    let key_manager = ctx.key_manager()
        .ok_or_else(|| Error::InvalidCallFormat(anyhow!("confidential txs unavailable")))?;
    let sk = keypair.input_keypair.sk;
    deoxysii::box_open(&envelope.nonce, envelope.data.clone(), vec![],
                       &envelope.pk.0, &sk.0)               // open the sealed calldata
```

**IMPACT:** Positive. The central product claim is real. Confidential contract storage and calldata are genuine ciphertext outside the enclave, keyed by material that only the key manager releases to an attested enclave. This is the strongest confirmation in the report. Informational.

---

## Claim 2: The confidentiality is hardware attested, so execution is verifiable without blind trust

**CLAIM:**
> "Verifiable Execution: Every execution produces cryptographic proof that users can verify without blind trust." "Run code inside hardware secured enclaves. Data stays encrypted even from server operators." The homepage frames the whole product as the answer to "Trust is the bottleneck."

**REALITY:** OVERSTATED, with strong credit. Two things are genuinely true and better than most peers. First, the attestation is real cryptography, not a stub: `go/common/sgx/pcs/` performs full ECDSA P256 DCAP verification of SGX and TDX quotes, including the PCK certificate chain, the Quoting Enclave report signature, the attestation key binding, and signed Intel TCB info with status enforcement, and the legacy `go/common/sgx/ias/` path verifies EPID reports against Intel's embedded root certificate authority. Second, an enclave measurement (MRENCLAVE and MRSIGNER) is enforced against an allowlist held in the on chain runtime descriptor, and the registry refuses an SGX deployment with an empty enclave list.

The overstatement is in the words "cryptographic proof ... without blind trust" and "verifiable." What the proof establishes is that a specific enclave measurement ran inside genuine Intel or AMD silicon. It does not remove trust; it relocates trust to the hardware vendor plus the set of enclave measurements that governance and the runtime owner decided to allow. If SGX or TDX is broken, or if an attacker controlled measurement is added to the allowlist, the guarantee collapses. A user cannot verify from pure cryptography that no one can see the plaintext; that link is asserted by Intel silicon and a governed allowlist.

**EVIDENCE:**
```go
// oasis-core go/common/sgx/pcs/quote.go (~L142-L207) real DCAP verification
func (q *Quote) Verify(policy *QuotePolicy, ts time.Time, tcb *TCBBundle) (*sgx.VerifiedQuote, error) {
    if policy.Disabled { return nil, fmt.Errorf("pcs/quote: PCS quotes are disabled by policy") }
    ...
    if !unsafeSkipVerify {
        err := q.signature.Verify(q.header, q.reportBody, ts, tcb, policy)  // ECDSA + PCK chain + TCB
    }
    return &sgx.VerifiedQuote{ Identity: q.reportBody.AsEnclaveIdentity() }, nil
}
```
```go
// oasis-core go/common/sgx/ias/certificates.go   Intel root of trust, compiled in
const iasTrustRootCert = `-----BEGIN CERTIFICATE-----` // Intel IAS report signing CA
var IntelTrustRoots = x509.NewCertPool()
```
```go
// oasis-core go/common/node/sgx.go (~L28-L40, L131-L134, L217-L243)
type SGXConstraints struct {
    Enclaves []sgx.EnclaveIdentity `json:"enclaves,omitempty"`  // allowed MRENCLAVE/MRSIGNER
    Policy   *quote.Policy         `json:"policy,omitempty"`
}
func (sa *SGXAttestation) Verify(...) error {
    verifiedQuote, err := sa.Quote.Verify(sc.Policy, ts)      // real quote verification
    if !sc.ContainsEnclave(verifiedQuote.Identity) {
        return ErrBadEnclaveIdentity                          // measurement must be on the allowlist
    }
    ...                                                       // report data must bind the node RAK
}
```
```go
// oasis-core go/registry/api/runtime.go (~L588-L605) ValidateDeployments
case node.TEEHardwareIntelSGX:
    if len(cs.Enclaves) == 0 {
        return fmt.Errorf("%w: invalid SGX TEE constraints", ErrNoEnclaveForRuntime)
    }
```

**IMPACT:** The proof is real and the attestation code is genuinely open source, which is a material advantage over closed attestation designs. But "verifiable execution without blind trust" is marketing shorthand for "trust Intel or AMD hardware plus the governed enclave allowlist." The confidentiality of every Sapphire contract reduces to that hardware and that allowlist, not to the blockchain or to cryptography that keeps the operator out unconditionally. High.

---

## Claim 3: On chain, hardware secured key management protects your keys

**CLAIM:**
> Oasis markets confidential ParaTimes as having "decentralized onchain key management" and "Programmable Policies ... enforced by hardware," with data that "stays encrypted even from server operators."

**REALITY:** CONFIRMED IN CODE that the mechanism exists and is well built, with a large caveat about where trust concentrates. The key manager is a Rust SGX enclave that holds a per generation master secret. Every Sapphire state key and calldata keypair is derived from that master secret with cSHAKE256 and KMAC256. The master secret is sealed at rest with an SGX hardware key bound to the exact enclave measurement (Keypolicy MRENCLAVE via EGETKEY), so only that measurement can unseal it. A compute runtime obtains keys only over an attested, mutually authenticated Noise session, and the key manager releases keys only if the caller's attested measurement appears in a signed policy allowlist. That policy must carry a threshold of signatures from a trusted signer set baked into the key manager enclave, and it is protected against rollback.

The caveat, which is the practical center of gravity of the whole confidentiality model, is that this is a trusted service. Whoever controls a threshold of the trusted signer keys, together with the key manager runtime's owning entity that submits the on chain `UpdatePolicy` transaction, can authorize an arbitrary new enclave measurement to fetch or replicate the master secret and thereby decrypt all state derived from it.

**EVIDENCE:**
```rust
// oasis-core keymanager/src/crypto/kdf.rs (~L180-L198) all state keys derive from the master secret
fn derive_keys(&self, secret: Secret, xof_custom: &[u8]) -> KeyPair {
    let mut xof = CShake::new_cshake256(&[], xof_custom);
    xof.update(secret.as_ref());
    let mut k = [0u8; 32]; xof.xof().squeeze(&mut k);
    let state_key = StateKey(k);                              // the Sapphire state encryption key
```
```rust
// oasis-core keymanager/src/crypto/kdf.rs (~L761-L812) master secret sealed to MRENCLAVE
let d2 = new_deoxysii(Keypolicy::MRENCLAVE, MASTER_SECRET_SEAL_CONTEXT);
let mut ciphertext = d2.seal(&nonce, secret, additional_data);      // store
let plaintext = d2.open(&nonce, ciphertext.to_vec(), additional_data).unwrap(); // load
// runtime/src/common/sgx/seal.rs (~L61): seal key derived from the EGETKEY instruction
```
```rust
// oasis-core keymanager/src/runtime/secrets.rs (~L495-L519) keys released only to an attested, allowed enclave
fn authorize_private_key_generation(ctx, runtime_id) -> Result<()> {
    if Policy::unsafe_skip() { return Ok(()); }              // unsafe builds only
    let remote_enclave = Self::authenticate(ctx)?;           // MRENCLAVE from the attested session
    Policy::global().may_get_or_create_keys(remote_enclave, runtime_id)
}
```
```rust
// oasis-core keymanager/src/policy/signers.rs (~L72-L84) policy needs a threshold of trusted signatures
if trusted.len() < self.threshold as usize {
    return Err(KeyManagerError::InsufficientSignatures.into());
}
```
```go
// oasis-core go/consensus/cometbft/apps/keymanager/secrets/txs.go (~L21-L36) only the KM owner publishes policy
if !kmRt.EntityID.Equal(ctx.TxSigner()) {
    return fmt.Errorf("keymanager: invalid update signer: %s", sigPol.Policy.ID)
}
```

**IMPACT:** The key management is genuine, sealed, attested, threshold governed, and rollback protected, which is serious engineering. But it is a trusted anchor, not a trustless one. The holders of the threshold signer keys plus the key manager owning entity are, in principle, able to grant a new enclave the ability to decrypt everything. This is the single most important trust assumption in the platform and the honest reading of "your data stays encrypted even from server operators" is "encrypted from any party that does not control the key manager policy." High.

---

## Claim 4: ROFL delivers verifiable offchain compute for AI

**CLAIM:**
> Oasis launched ROFL as a "Verifiable OffChain Compute Framework Powering AI Applications" that lets developers "run apps offchain with onchain attestation that verifies execution without exposing data," including "provable AI learning." The docs say Sapphire contracts can "verify the ROFL transaction origin."

**REALITY:** OVERSTATED. The on chain ROFL module verifies the identity and attestation of the enclave and the fact that it is a registered, endorsed instance. It does not verify the correctness of the computation. When a ROFL app registers, the chain checks exactly three things: that the transaction is signed by the enclave's Runtime Attestation Key, that the TEE quote verifies against the app's quote policy, and that the enclave measurement is on an allowlist the app admin defined, plus a node endorsement. When another contract "trusts" a ROFL output, it only asks whether the call originated from a registered instance of that app. The registration payload carries no result, no output commitment, and no execution proof. So "verifiable" here means "an approved binary ran inside a genuine TEE," not "the produced value (for example an AI inference) is provably correct." Correctness reduces to trusting the TEE hardware and the specific code whose measurement the admin approved.

**EVIDENCE:**
```rust
// oasis-sdk runtime-sdk/src/modules/rofl/mod.rs (~L380-L448) rofl.Register verifies identity, not output
if !signer_pks.contains(&body.ect.capability_tee.rak.into()) {
    return Err(Error::NotSignedByRAK);                       // 1) signed by the enclave RAK
}
let verified_ect = body.ect.verify(&cfg.policy.quotes)...?;  // 2) TEE quote is cryptographically valid
if !cfg.policy.enclaves
    .contains(&verified_ect.verified_attestation.quote.identity) {
    return Err(Error::UnknownEnclave);                       // 3) measurement is on the admin allowlist
}
let node = Cfg::EndorsementPolicyEvaluator::verify(ctx, &cfg.policy.endorsements, ...)?;
```
```rust
// oasis-sdk runtime-sdk/src/modules/rofl/policy.rs (~L30-L41) the trusted set is chosen by the app admin
pub struct AppAuthPolicy {
    pub quotes: QuotePolicy,
    pub enclaves: Vec<EnclaveIdentity>,       // admin defined allowlist of measurements
    pub endorsements: Vec<Box<AllowedEndorsement>>,
}
```
```rust
// oasis-sdk runtime-sdk/src/modules/rofl/mod.rs (~L147-L149) consumers verify origin only
fn is_authorized_origin(app: app_id::AppId) -> bool {
    Self::get_origin_registration(app).is_some()             // did this come from a registered instance
}
```
```rust
// oasis-sdk runtime-sdk/src/modules/rofl/types.rs (~L92-L106)
// Register carries: app id, endorsed TEE capability (ect/quote), expiration, extra_keys, metadata.
// No result, no output commitment, no proof of computation.
```

**IMPACT:** ROFL is a real, useful confidential compute framework and its origin attestation is genuine. But "verifiable offchain compute" and "provable AI" mean attestation of enclave identity, not a succinct proof that the output is correct. A malicious or buggy but correctly measured binary would still pass every on chain check. The accurate reading is "the code that ran is the approved code, inside a genuine TEE," which is integrity of the environment, not proof of the result. Medium.

---

## Claim 5: ROSE is a fixed supply asset capped at 10 billion

**CLAIM:**
> "The ROSE native token is a capped supply token." The total cap is fixed at 10,000,000,000 ROSE, with about 2.3 billion reserved as staking rewards that "will be automatically paid out." There is no new issuance, only newly distributed ROSE.

**REALITY:** CONFIRMED IN CODE. ROSE supply is genuinely fixed. Staking rewards are not minted; they are transferred out of a pre funded common pool into stakers' escrow, and the payout is skipped entirely if the common pool cannot cover it, which would be impossible if new tokens were being created. A staking sanity check enforces a conservation invariant: the sum of all account balances plus governance deposits plus the common pool plus last block fees must equal the total supply. There is no mint or coinbase function anywhere in the staking module.

**EVIDENCE:**
```go
// oasis-core go/consensus/cometbft/apps/staking/state/state.go (~L1192-L1265) AddRewards
commonPool, err := s.CommonPool(ctx)
if q.Cmp(commonPool) == 1 { continue }        // not enough in the pool: skip, never mint
quantity.Move(&ent.Escrow.Active.Balance, commonPool, q)   // move FROM the common pool
ctx.EmitEvent(... &staking.AddEscrowEvent{ Owner: staking.CommonPoolAddress, ... })
```
```go
// oasis-core go/staking/api/sanity_check.go (~L345-L353) total supply conservation invariant
_ = total.Add(&g.GovernanceDeposits)
_ = total.Add(&g.CommonPool)
_ = total.Add(&g.LastBlockFees)
if total.Cmp(&g.TotalSupply) != 0 {
    return fmt.Errorf("staking: sanity check failed: ... does not add up to total supply")
}
```
```go
// oasis-core go/staking/api/rewards.go   rewards are scaling factors drawn against the pool, not an emission rate
// RewardStep{ Until beacon.EpochTime, Scale quantity.Quantity }
// TotalSupply is a genesis field, never increased by rewards.
```

**IMPACT:** Positive. Supply is a genuine fixed cap disbursement model, exactly as marketed. Rewards debit a pre allocated common pool rather than inflating supply, and the conservation invariant makes over issuance impossible. One nuance worth stating: the consensus layer uses 9 decimals (1 ROSE equals 10 to the ninth base units, so 10 billion ROSE equals 10 to the nineteenth base units), while the Sapphire EVM represents ROSE with 18 decimals. Informational.

---

## Claim 6: wROSE is a standard wrapped native token

**CLAIM:**
> wROSE at 0x8Bc2B030b299964eEfb5e1e0b36991352E56D2D3 is the ERC20 wrapper for native ROSE on Sapphire, used across DeFi like wrapped ETH.

**REALITY:** CONFIRMED IN CODE. wROSE is a textbook WETH pattern wrapper built on OpenZeppelin `ERC20` and `ERC20Burnable`. Depositing native ROSE mints wROSE one to one, withdrawing burns wROSE and returns native ROSE, and it emits standard `Deposit` and `Withdrawal` events. There is no owner, no pause, no fee on transfer, and no mint path beyond the one to one deposit. Decimals default to 18, matching the Sapphire EVM native representation.

**EVIDENCE:**
```solidity
// sapphire-paratime contracts/contracts/WrappedROSE.sol (full file, 32 lines)
contract WrappedROSE is ERC20, ERC20Burnable {
    constructor() ERC20("Wrapped ROSE", "wROSE") {}
    function deposit() external payable { _deposit(); }
    function withdraw(uint256 amount) external {
        _burn(msg.sender, amount);
        payable(msg.sender).transfer(amount);
        emit Withdrawal(msg.sender, amount);
    }
    receive() external payable { _deposit(); }
    function _deposit() internal { _mint(msg.sender, msg.value); emit Deposit(msg.sender, msg.value); }
}
```

**IMPACT:** Positive. The wrapper is a standard, minimal, non custodial WETH clone with no surprising powers. Informational.

---

## Claim 7: The trusted code set is fixed and enforced by hardware, so the system is trustless and immutable

**CLAIM:**
> "Programmable Policies ... upgrade rules enforced by hardware," "Trustless Execution," and ROFL's "uncensorable registration, management and deployment" imply that what runs, and what can decrypt, is fixed and beyond operator control.

**REALITY:** OVERSTATED. The set of enclave measurements a runtime may run under lives in the on chain runtime descriptor and is mutable by whoever controls that runtime's governance model. For the common `GovernanceEntity` model this is the registering entity's key; alternatively a runtime can be placed under network governance. The key manager access policy is likewise mutable by the key manager owning entity through the `UpdatePolicy` transaction (bounded inside the enclave by the threshold signer check of Claim 3). Network governance can additionally push coordinated `Upgrade` proposals that halt the network and rotate the node and enclave software set, and can change consensus module parameters. In short, the trusted code set is governed and upgradeable, not immutable, and "trustless" understates the roles of the runtime owner, the key manager owner, and network governance.

**EVIDENCE:**
```go
// oasis-core go/consensus/cometbft/apps/registry/transactions.go (~L671-L704) who may change the enclave set
if !ctx.CallerAddress().Equal(*expectedAddr) {
    switch rtToCheck.GovernanceModel {
    case registry.GovernanceEntity:  return nil, registry.ErrIncorrectTxSigner  // controlling entity key
    case registry.GovernanceRuntime: return nil, registry.ErrForbidden
    }
}
```
```go
// oasis-core go/governance/api/proposal.go (~L61-L67) governance action set
type ProposalContent struct {
    Upgrade          *UpgradeProposal
    CancelUpgrade    *CancelUpgradeProposal
    ChangeParameters *ChangeParametersProposal
}
// UpgradeProposal embeds upgrade.Descriptor{Handler, Target ProtocolVersions, Epoch}:
// a coordinated halt and upgrade that rotates the trusted node/enclave software set.
```

**IMPACT:** The programmable policies and the trusted enclave set are real and hardware enforced at execution time, but they are set and can be changed by entity keys and by network governance. The honest description is "governed by runtime owners, the key manager owner, and on chain proof of stake governance," not "trustless and immutable." Medium.

---

## Additional Note: The debug and mock TEE bypasses fail closed

Worth stating plainly, because it is a point of credit rather than a finding. The codebase contains the usual developer escape hatches that disable attestation, allow debug enclaves, skip quote verification, or install a mock trusted signer set. We checked whether any are reachable on a production build, and they are not, protected by two independent locks. The Rust key manager bypasses are compile time `option_env!` flags and assert `!BUILD_INFO.is_secure`, so they cannot be toggled in a shipped secure binary. The Go bypasses require the hidden master flag `debug.dont_blame_oasis` plus a per feature flag, and the corresponding debug genesis parameters are rejected by sanity checks in a secure network.

**EVIDENCE:**
```rust
// oasis-core keymanager/src/policy/cached.rs (~L41-L47) compile time only
pub fn unsafe_skip() -> bool {
    option_env!("OASIS_UNSAFE_SKIP_KM_POLICY").is_some()
        && option_env!("OASIS_UNSAFE_ALLOW_DEBUG_ENCLAVES").is_some()
}
```
```go
// oasis-core go/oasis-node/cmd/common/common.go (~L222-L228) node flag gated, warns loudly
if flags.DebugDontBlameOasis() && viper.GetBool(CfgDebugAllowDebugEnclaves) {
    rootLog.Warn("`debug.allow_debug_enclaves` set, enclaves in debug mode will be allowed")
    ias.SetAllowDebugEnclaves(); pcs.SetAllowDebugEnclaves()
}
```
```go
// oasis-core go/registry/api/sanity_check.go (~L20-L22) production genesis rejects debug params
if !flags.DebugDontBlameOasis() {
    if p.DebugAllowUnroutableAddresses || p.DebugDeployImmediately {
        return fmt.Errorf("one or more unsafe debug flags set")
    }
}
```

**IMPACT:** Positive. Unlike projects that ship an `if true` bypass in production, Oasis's unsafe paths are gated by compile time secrecy of the build and by production genesis sanity checks. The residual trust assumptions are the intended ones, not accidental backdoors. Low.

---

## Conclusion

Oasis Network is a genuine confidential computing Layer 1 whose code substantially delivers what it markets. Sapphire really encrypts contract state and calldata with DeoxysII keyed from the key manager (Claim 1). The attestation stack really performs full DCAP and IAS quote verification in open source and enforces enclave measurements against an on chain allowlist (Claim 2). The key manager really is a sealed, attested, threshold governed enclave service (Claim 3). ROSE really is a fixed supply asset paid from a conserved common pool (Claim 5), and wROSE is a standard wrapper (Claim 6). Four load bearing claims are confirmed in code and none are false or fabricated.

The overstatements are the familiar gap between the word "trustless" and a design whose entire privacy and verifiability guarantee rests on three anchors: Intel and AMD TEE hardware as the physical root of trust, the set of enclave measurements that runtime owners and governance choose to trust, and the key manager that holds the master secret from which all Sapphire state keys derive. "Verifiable execution ... without blind trust" is attestation of enclave identity, not a proof of computational correctness (Claims 2 and 4). ROFL "verifiable offchain compute for AI" proves that an approved binary ran in a genuine TEE, not that its output is correct (Claim 4). And the trusted code set is governed and upgradeable, not immutable (Claim 7).

None of this is fraud. It is the difference between a strong, working confidential computing platform with honest hardware and governance trust assumptions, and the frictionless trustless narrative the marketing sometimes implies. Oasis actually scores better than most confidential compute peers on one axis that matters: its attestation and key management are fully open source and verifiable code, not a closed third party black box. Score 82 out of 100, LOW to MEDIUM RISK, driven entirely by TEE hardware trust and governance centralization caveats rather than by any dishonest or broken code.

| Claim | Verdict |
|-------|---------|
| Genuine, heavily engineered Layer 1 | CONFIRMED (foundation) |
| Sapphire confidential EVM keeps contract state private | CONFIRMED IN CODE |
| Confidentiality is hardware attested, verifiable without blind trust | OVERSTATED (trust root is Intel silicon plus a governed enclave allowlist) |
| On chain, hardware secured key management protects your keys | CONFIRMED IN CODE (mechanism real, central trust anchor) |
| ROFL delivers verifiable offchain compute for AI | OVERSTATED (identity attestation, not proof of output correctness) |
| ROSE is a fixed supply asset capped at 10 billion | CONFIRMED IN CODE |
| wROSE is a standard wrapped native token | CONFIRMED IN CODE |
| Trusted code set is fixed, trustless, and immutable | OVERSTATED (governed and upgradeable) |
| Debug and mock TEE bypasses | Not production reachable (credit) |

Tally: CONFIRMED IN CODE 4, OVERSTATED 3, FALSE 0.

---

## Verification and Sources (exact repositories and files read)

Sapphire confidential EVM (sapphire-paratime, branch main):
- runtime/src/lib.rs (CONFIDENTIAL flag, SGX target env, consensus trust root, trusted policy signers)
- runtime/src/main.rs, runtime/Makefile (debug-mock-sgx dev build path)
- contracts/contracts/WrappedROSE.sol (wROSE wrapper)

Runtime SDK and EVM module (oasis-sdk, branch main):
- runtime-sdk/modules/evm/src/state.rs (CONFIDENTIAL_STORAGES 0x04, with_confidential_storage, state_key)
- runtime-sdk/src/storage/confidential.rs (ConfidentialStore, DeoxysII seal and open)
- runtime-sdk/src/callformat.rs (EncryptedX25519DeoxysII envelope, box_open)
- runtime-sdk/src/keymanager.rs (get_or_create_keys client)
- runtime-sdk/src/modules/rofl/mod.rs (tx_register, is_authorized_origin), policy.rs (AppAuthPolicy), types.rs (Register)

Attestation, key manager, consensus, staking, governance (oasis-core, branch master):
- go/common/sgx/pcs/quote.go, pcs/tcb.go (DCAP and TDX quote and TCB verification)
- go/common/sgx/ias/avr.go, ias/quote.go, ias/certificates.go (EPID and IAS verification, Intel root CA)
- go/common/sgx/quote/quote.go, go/common/sgx/common.go (EnclaveIdentity, MRENCLAVE, MRSIGNER)
- go/common/node/sgx.go, node/node.go (SGXConstraints, ContainsEnclave, SGXAttestation.Verify)
- go/registry/api/runtime.go, api/sanity_check.go, go/consensus/cometbft/apps/registry/transactions.go (deployment enclave allowlist, governance model, debug param rejection)
- go/runtime/host/sgx/ecdsa.go, host/sgx/provisioner.go (host side quote generation)
- runtime/src/identity.rs, runtime/src/attestation.rs, runtime/src/common/sgx/seal.rs (RAK generation and binding, EGETKEY sealing)
- keymanager/src/crypto/kdf.rs (master secret, derive_keys, MRENCLAVE sealing)
- keymanager/src/runtime/secrets.rs (get_or_create_keys, authorize_private_key_generation, authenticate)
- keymanager/src/policy/cached.rs, policy/signers.rs, policy/global.rs (may_get_or_create_keys, threshold trusted signers, unsafe_skip)
- keymanager/src/secrets/provider.rs (master secret replication)
- go/keymanager/secrets/policy_sgx.go, secrets/api.go, go/consensus/cometbft/apps/keymanager/secrets/txs.go (PolicySGX, UpdatePolicy owner gate)
- go/consensus/cometbft/apps/staking/state/state.go, go/staking/api/sanity_check.go, staking/api/rewards.go, staking/api/api.go (AddRewards from common pool, supply conservation, reward schedule)
- go/governance/api/api.go, governance/api/proposal.go, go/upgrade/api/api.go (Upgrade, CancelUpgrade, ChangeParameters)
- go/oasis-node/cmd/common/common.go, cmd/common/flags/flags.go (debug.dont_blame_oasis gating of unsafe paths)

Documentation and public record:
- oasis.net homepage ("Build Apps Users Can Verify", "Verifiable Execution ... cryptographic proof ... without blind trust", "Trustless Execution", "hardware secured enclaves")
- docs.oasis.io build/sapphire ("Confidential state, end to end encryption"), general/oasis-network (TEE black box, data never leaked to node operator), general/oasis-network/token-metrics-and-distribution (capped 10,000,000,000 ROSE, about 2.3 billion staking rewards)
- ROFL mainnet launch press ("Verifiable OffChain Compute Framework Powering AI Applications")

---

## Disclaimer

This report documents the relationship between Oasis Network's public marketing and documentation claims and its publicly available open source code. All findings are based on source code read from the oasisprotocol GitHub organization and on the project's own documentation. Oasis is a genuine, long running project; this review credits what the code delivers and flags where marketing language runs ahead of the implementation. Read only review, no systems were accessed or modified.

**Report Date:** 2026-08-05
**Website:** https://mefai.io
