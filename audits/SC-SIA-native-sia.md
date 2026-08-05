# Sia (SC): Whitepaper Claims vs Code Reality

**Score: 87/100, RISK LOW**

Date: 2026-08-05

Token (live): SC (Siacoin), native coin on its own UTXO proof of work chain (not an ERC20). Price roughly $0.0005 USD, market capitalization roughly $25M, rank near #713. Circulating supply roughly 56 billion SC out of roughly 62 billion minted; no maximum cap. Tip height roughly 587,297. Siafunds (SF) are a separate 10,000 unit security asset that captures contract revenue.

Websites: sia.tech (product), docs.sia.tech (docs), siascan.com (explorer)

GitHub: github.com/SiaFoundation (core, coreutils, hostd, renterd, walletd); legacy github.com/NebulousLabs/Sia

## Severity Summary

| Severity | Count | Items |
|---|---|---|
| Critical | 0 | None |
| High | 0 | None |
| Medium | 1 | SC is permanently inflationary with no cap; new issuance is roughly 2.8 percent per year today |
| Low | 2 | v2 contracts levy a 4 percent Siafund tax, not the historically advertised 3.9 percent; renter object metadata lives in a local off chain database, so a lost seed or database can orphan stored data |
| Informational | 2 | The storage yield accrues to Siafund holders, not to ordinary SC holders; network storage utilization sits near 23 percent, indicating niche adoption |

## Why This Report Exists

Sia markets itself as a genuinely decentralized cloud storage network where storage is enforced by cryptography and settled on a public blockchain, in explicit contrast to systems such as Storj that rely on a central Satellite for coordination. Those are strong, testable claims. This report reads the actual public Go source in the SiaFoundation repositories and checks each flagship claim against the code that consensus nodes, hosts, and renters really run, then cross checks the live chain state on a public explorer. The token is treated as secondary but is verified independently.

## Method

Read only. All four active service repositories plus the shared libraries were cloned at depth 1: `core` (consensus, types, transaction validation), `coreutils` (the mainnet network definition and genesis), `hostd` (the storage provider daemon), `renterd` (the renter daemon and erasure coding), and `walletd`. The legacy `NebulousLabs/Sia` monorepo was also fetched for lineage. Each claim was traced to concrete files and line numbers. Live coin data came from a public market API; live network data (host counts, storage, block height) came from the siascan explorer on 2026-08-05. No private endpoints, no write operations.

## The Foundation: contracts, proofs, and collateral are first class consensus objects

Sia does not bolt storage onto a payment coin. The file contract, the storage proof, and the Siafund are native transaction elements defined in the type system and enforced by the consensus validator itself. That is the architectural fact everything else rests on, and it is visible directly in `core/types/types.go` and `core/consensus`. A renter and a host agree on a `FileContract` (or the newer `V2FileContract`) that names the data by its Merkle root, locks host collateral, and predefines exactly who gets paid if the host proves storage versus if it fails. The host then broadcasts a storage proof on chain to claim payment. There is no server in the middle deciding any of this; the rules run on every full node.

## Claim 1: Genuine storage enforced by on chain file contracts and cryptographic proof of storage

CLAIM: "Storage providers must cryptographically prove that they are hosting the required data, and if they fail to uphold the storage contract, their collateral is forfeited." (Sia docs, network model)

REALITY: CONFIRMED IN CODE. A `FileContract` binds the stored data by `FileMerkleRoot` and defines split payouts for the valid and missed cases. Storage proofs are validated in consensus against a leaf index that is derived from the proof window block ID, so the host cannot know in advance which 64 byte leaf it must reveal; it must actually hold the data. A wrong proof is rejected.

EVIDENCE:
```go
// core/types/types.go:280
type FileContract struct {
	Filesize           uint64          `json:"filesize"`
	FileMerkleRoot     Hash256         `json:"fileMerkleRoot"`
	WindowStart        uint64          `json:"windowStart"`
	WindowEnd          uint64          `json:"windowEnd"`
	Payout             Currency        `json:"payout"`
	ValidProofOutputs  []SiacoinOutput `json:"validProofOutputs"`
	MissedProofOutputs []SiacoinOutput `json:"missedProofOutputs"`
	...
}
// core/types/types.go:377
// A StorageProof asserts the presence of a randomly-selected leaf within the
// Merkle tree of a FileContract's data.
type StorageProof struct {
	ParentID FileContractID
	Leaf     [64]byte
	Proof    []Hash256
}
```
```go
// core/consensus/validation.go:370  (storage proof rejected if root does not match)
leafIndex := ms.base.StorageProofLeafIndex(fc.Filesize, windowID, sp.ParentID)
leaf := storageProofLeaf(leafIndex, fc.Filesize, sp.Leaf)
...
} else if storageProofRoot(leafIndex, fc.Filesize, leaf, sp.Proof) != fc.FileMerkleRoot {
	return fmt.Errorf("storage proof %v has root that does not match contract Merkle root", i)
}
```
```go
// core/consensus/state.go:403  (which leaf is unpredictable: seeded by the window block ID)
func (s State) StorageProofLeafIndex(filesize uint64, windowID types.BlockID, fcid types.FileContractID) uint64 {
	...
	seed := hashAll(windowID, fcid)
	var r uint64
	for i := 0; i < len(seed); i += 8 {
		_, r = bits.Div64(r, binary.BigEndian.Uint64(seed[i:]), numLeaves)
	}
	return r
}
```

IMPACT: The strongest claim Sia makes is also its most solidly implemented one. Proof of storage is a real Merkle inclusion proof over a leaf the host cannot predict, checked by consensus. This is not a metaphor for storage; it is enforced storage.

## Claim 2: Host collateral and renter payment are escrowed and settled on chain

CLAIM: Hosts lock collateral that is forfeited on failure, and renter funds are held in escrow and released against proof, all settled on the blockchain rather than by a trusted operator.

REALITY: CONFIRMED IN CODE. The `V2FileContract` carries explicit `TotalCollateral` and `MissedHostValue` fields, plus separate host and renter outputs. When the proof window opens, the host daemon builds a real storage proof and broadcasts a `V2FileContractResolution` transaction that settles the contract on chain. If it fails to prove, the missed path pays the host only its unrisked collateral, forfeiting the rest.

EVIDENCE:
```go
// core/types/types.go:524  (collateral is a native contract field, not an app level ledger)
type V2FileContract struct {
	...
	HostOutput      SiacoinOutput `json:"hostOutput"`      // revenue + contract price + collateral
	MissedHostValue Currency      `json:"missedHostValue"` // collateral not yet risked
	TotalCollateral Currency      `json:"totalCollateral"` // risked + unrisked collateral
	...
}
```
```go
// hostd/host/contracts/update.go:194  (host settles the contract by broadcasting the proof on chain)
func (cm *Manager) broadcastV2StorageProof(cs consensus.State, proofBasis types.ChainIndex, ele V2ProofElement, log *zap.Logger) error {
	sp, err := cm.buildV2StorageProof(cs, ele, log.Named("proof"))
	...
	resolution := types.V2FileContractResolution{Parent: ele.V2FileContractElement, Resolution: &sp}
	...
	} else if err := cm.wallet.BroadcastV2TransactionSet(basis, resolutionTxnSet); err != nil {
```

IMPACT: Escrow and collateral are consensus enforced, not promises kept by a company. The economic penalty for a lying host is coded into the missed output path and cannot be waived by any coordinator.

## Claim 3: Redundancy via client side erasure coding

CLAIM: Data is split with erasure coding on the client so it survives host failure without full replication.

REALITY: CONFIRMED IN CODE. The renter daemon uses Reed Solomon erasure coding (the `reedsolomon` library) to split each slab into data and parity shards before upload, and reconstructs from any sufficient subset on download. Default mainnet redundancy is 10 data shards of 30 total, a 3x factor that tolerates loss of 20 of 30 hosts per slab.

EVIDENCE:
```go
// renterd/object/slab.go:92
func (s Slab) Encode(buf []byte, shards [][]byte) {
	...
	stripedSplit(buf, shards[:s.MinShards])
	rsc, _ := reedsolomon.New(int(s.MinShards), len(shards)-int(s.MinShards))
	if err := rsc.Encode(shards); err != nil { panic(err) }
}
// renterd/object/slab.go:109
func (s Slab) Reconstruct(shards [][]byte) error {
	...
	rsc, _ := reedsolomon.New(int(s.MinShards), len(shards)-int(s.MinShards))
	if err := rsc.Reconstruct(shards); err != nil { return err }
	return nil
}
```
```go
// renterd/api/setting.go:67  (mainnet default: 10 of 30)
rs := RedundancySettings{
	MinShards:   10,
	TotalShards: 30,
}
```

IMPACT: Redundancy is genuine forward error correction performed on the renter side, and encryption also happens client side, so hosts store opaque shards. This is materially stronger than naive replication and keeps the host set trustless.

## Claim 4: Coordination happens on chain, unlike Storj's central Satellite

CLAIM: Sia has no central coordinator; hosts announce themselves and contracts are formed and settled on the public chain, in contrast to Storj, which routes discovery, auditing, and payment through a company run Satellite.

REALITY: CONFIRMED IN CODE, with one honest caveat. Host discovery is on chain: a host advertises its network address by broadcasting a signed attestation transaction to the chain, so any renter can find hosts by scanning the blockchain rather than querying a company. Contracts, collateral, proofs, and payouts are all consensus objects as shown above. There is no Satellite equivalent anywhere in the codebase. The caveat is that the renter's object metadata (which slabs map to which hosts and sectors) is kept in the renter's own local database, off chain; that is a self sovereign design choice, not a central service, but it does mean the renter is responsible for its own metadata.

EVIDENCE:
```go
// hostd/host/settings/announce.go:88  (host publishes its address on chain, no registry service)
func (m *ConfigManager) Announce() error {
	...
	txn := types.V2Transaction{
		Attestations: []types.Attestation{
			chain.V2HostAnnouncement(m.RHP4NetAddresses()).ToAttestation(cs, m.hostKey),
		},
		MinerFee: minerFee,
	}
	...
	} else if err := m.wallet.BroadcastV2TransactionSet(cs.Index, txnset); err != nil {
```

IMPACT: The comparison to Storj holds where it matters: trust critical functions (discovery, escrow, proof, settlement) require no trusted third party. Renters do bear the burden of their own metadata custody, which is the price of removing the coordinator.

## Claim 5: SC is inflationary with no hard cap

CLAIM: "The total supply of Siacoins (SC) is unlimited; there will never be a cap." (Sia docs, supply)

REALITY: CONFIRMED IN CODE. Block reward starts at 300,000 SC and decreases by 1 SC per block, but is floored at a permanent `MinimumCoinbase` of 30,000 SC per block. Because the floor never reaches zero, issuance continues forever.

EVIDENCE:
```go
// core/consensus/state.go:245
func (s State) BlockReward() types.Currency {
	r, underflow := s.Network.InitialCoinbase.SubWithUnderflow(types.Siacoins(uint32(s.childHeight())))
	if underflow || r.Cmp(s.Network.MinimumCoinbase) < 0 {
		return s.Network.MinimumCoinbase
	}
	return r
}
```
```go
// coreutils/chain/network.go:25  (mainnet parameters)
InitialCoinbase: types.Siacoins(300000),
MinimumCoinbase: types.Siacoins(30000),
```

IMPACT: At roughly 52,560 blocks per year and 30,000 SC per block, new issuance is roughly 1.58 billion SC per year, or about 2.8 percent of the roughly 56 billion circulating today. This declines as a percentage over time but never stops. Marketing that frames this as negligible is OVERSTATED; it is modest and shrinking in relative terms, but it is a permanent structural dilution and should be stated plainly.

## Claim 6: Siafunds take a 3.9 percent fee on contract revenue

CLAIM: Siafund holders receive a 3.9 percent share of the payout of every completed file contract.

REALITY: CONFIRMED IN CODE for legacy v1 contracts, with a LOW severity nuance for v2. The v1 `FileContractTax` multiplies contract payout by exactly 0.039 (expressed post hardfork as 39/1000) and rounds down to a multiple of the 10,000 Siafund supply. The newer v2 contract tax is 4 percent, not 3.9 percent, so the frequently repeated 3.9 percent figure is slightly outdated for contracts formed under the current protocol.

EVIDENCE:
```go
// core/consensus/state.go:373
func (s State) FileContractTax(fc types.FileContract) types.Currency {
	i := fc.Payout.Big()
	if s.childHeight() < s.Network.HardforkTax.Height {
		r := new(big.Rat).SetInt(i)
		r.Mul(r, new(big.Rat).SetFloat64(0.039))   // 3.9 percent
		i.Div(r.Num(), r.Denom())
	} else {
		i.Mul(i, big.NewInt(39)); i.Div(i, big.NewInt(1000))  // 3.9 percent
	}
	i.Sub(i, new(big.Int).Mod(i, big.NewInt(int64(s.SiafundCount()))))
	...
}
// core/consensus/state.go:395
func (s State) V2FileContractTax(fc types.V2FileContract) types.Currency {
	return fc.RenterOutput.Value.Add(fc.HostOutput.Value).Div64(25) // 4%
}
```
```go
// core/consensus/state.go:269  (fixed 10,000 Siafunds)
func (s State) SiafundCount() uint64 { return 10000 }
```

IMPACT: The Siafund revenue mechanism is real and consensus enforced. Two honest points follow. First, the current rate is 4 percent on v2 contracts, so update the 3.9 percent talking point. Second, and more important for token buyers: this yield accrues to the 10,000 Siafunds, not to ordinary SC holders. Buying SC is buying the network's gas and collateral medium, not its cash flow.

## Claim 7: The product is live, stores real data, has real hosts, and is tradeable

CLAIM: Sia is a live, in production storage network with real usage.

REALITY: CONFIRMED. The explorer on 2026-08-05 shows a live chain at height roughly 587,297 with real utilization: roughly 1.9 PB of data actually stored against roughly 8.1 PB of advertised capacity (about 23 percent utilization), served by roughly 55 active hosts of about 7,031 announced historically. SC trades on major venues with a market capitalization near $25M and daily volume in the millions. The protocol is mature (the codebase traces back to the NebulousLabs era and has since been rebuilt as the modular hostd, renterd, and walletd stack with a v2 hardfork).

EVIDENCE: siascan.com network view (height 587,297; roughly 1.9 PB used of 8.1 PB; roughly 55 active hosts of about 7,031 announced) and public market data (price roughly $0.0005, cap roughly $25M) as of 2026-08-05.

IMPACT: This is a working network with paying storage demand, not a testnet or a whitepaper. The honest qualifier is scale: about 23 percent utilization and a few dozen active hosts place Sia in the niche tier of decentralized storage, well behind centralized clouds and modest even among Web3 storage peers.

## Positive Findings

- Storage is enforced by consensus, not by a server. File contracts, storage proofs, and collateral are native transaction elements validated by every node.
- The proof of storage is cryptographically sound: a Merkle inclusion proof over a leaf whose index is seeded by the proof window block ID, so a host cannot fake it without holding the data.
- Host collateral and renter escrow are real economic instruments in the contract, with a coded penalty on the missed path.
- Redundancy uses real Reed Solomon erasure coding client side (default 10 of 30) plus client side encryption, keeping hosts trustless and opaque.
- Host discovery is fully decentralized via on chain announcements, with no Satellite style central coordinator anywhere in the stack. The Storj contrast is fair.
- The network is genuinely live with petabytes of real stored data and dozens of active hosts, and the coin is liquid.

## Conclusion

Sia is the rare project where the flagship claims survive contact with the source. Proof of storage, on chain file contracts, host collateral, client side erasure coding, and decentralized on chain coordination are all present, concrete, and consensus enforced, traced here to specific Go files and lines. There is no central coordinator, and the comparison to Storj's Satellite is accurate. The honest deductions are economic rather than technical: SC is permanently inflationary with no cap (roughly 2.8 percent per year today and declining in relative terms), the storage yield accrues to the separate 10,000 Siafunds rather than to SC holders, the advertised Siafund rate is now 4 percent on v2 contracts rather than 3.9 percent, and adoption remains niche at roughly 23 percent utilization and a few dozen active hosts. None of these are integrity failures; they are the trade offs of a mature, genuinely decentralized storage protocol. Verdict: Passed, 87/100, RISK LOW.
