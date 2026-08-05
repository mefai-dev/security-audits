# Cortex (CTXC): Whitepaper Claims vs Code Reality

**Score: 58/100, MEDIUM RISK**

**Date:** 2026-08-05
**Coin:** CTXC (native coin of the Cortex chain; total supply 299,792,458, the speed of light in m/s; second unit "Endorphin" prices inference gas)
**Chain:** Cortex mainnet "Arnold" (2019), an EVM compatible go-ethereum fork with added AI opcodes
**Websites:** cortexlabs.ai, zkmatrix.cortexlabs.ai
**GitHub:** github.com/CortexFoundation (CortexTheseus, torrentfs, inference, cvm-runtime, zkcvm-mono)

---

## Severity Summary

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 2 |
| Medium | 3 |
| Low | 1 |
| Informational | 2 |

---

## Why This Report Exists

Cortex is one of the oldest "AI on blockchain" projects, launched in 2018 with backing from Bitmain and FBG Capital. Its central promise was radical for its time: run real machine learning models deterministically inside the virtual machine, so that a smart contract can call an AI model and every node agrees on the output. That is a hard problem, and most projects that claimed it never wrote the code. Cortex did.

This report reads the actual public source, claim by claim, and applies the same discipline we apply to projects whose code contradicts their marketing. The result is mixed in an unusual way. The flagship engineering is genuinely present and genuinely clever, and we credit it in full. But the live marketing today describes a thriving world computer and a launching Layer 2, while the reality is a dormant network with an untradeable, delisted coin, an AI engine frozen at 2022 to 2023 versions, and a "ZkMatrix" Layer 2 that returns an error page instead of a running rollup.

We are not making accusations and we are not spreading FUD. We read the source and show what is actually implemented.

## Method

For every major claim on cortexlabs.ai we located the relevant code in the real repositories, fetched raw files from GitHub, and read what is implemented. We traced the full on chain inference path from the CVM opcode down to the native inference library call, we read the go.mod pins to learn which versions the live client actually links, and we checked the ZkMatrix repositories and site against the "launching" language. Each claim is labelled CONFIRMED IN CODE, OVERSTATED, or FALSE, with a repository path, file, line, and a short snippet. Market and delisting facts were cross checked against exchange announcements and price aggregators.

---

## The Foundation: A go-ethereum Fork Carrying a Real AI Extension, on a Dead Market

**CLAIM:**
> "The first decentralized world computer capable of running AI and AI-powered dApps."

**REALITY:** Two separate truths sit here. First, the base client is a go-ethereum fork. The recent commit stream in CortexTheseus is dominated by upstream geth cherry picks (for example `p2p/nat: server list contains IPv6 servers (#35084)`, `accounts/abi: fix wrong want count for events (#35077)`, `rpc: reject empty batch in BatchCallContext (#34985)`, `crypto/ecies: correctly return ErrInvalidMessage (#35037)`). Those are go-ethereum pull request numbers and go-ethereum commit message prefixes. The virtual machine itself is geth's EVM renamed to CVM (the file is `core/vm/cvm.go`). This is normal and not deceptive, but it means the "active development" a casual observer sees is largely the base layer tracking Ethereum, not AI feature work.

Second, the coin the world computer runs on is effectively dead. CTXC was delisted by Binance on 2025-04-16 (first "Vote to Delist" batch) and by OKX on 2025-06-20 (cited: low volume and liquidity). Price is roughly $0.0008, market cap roughly $0.19M, rank roughly #4,800, and CoinGecko reports no trades in the last 24 hours and no active tickers, a fall of roughly 99.97% from the 2018 all time high of $2.39. The lifetime low was set on 2026-05-31, weeks before this review.

**IMPACT:** The phrase "world computer capable of running AI" describes a real capability (see Claim 1) on a chain that almost nobody trades or transacts on anymore. Informational for the fork provenance; the market status is context that makes several present tense marketing claims read as aspirational rather than current.

---

## Claim 1: The CVM runs AI model inference on chain via the Synapse engine

**CLAIM:**
> "The CVM is EVM-compatible with added support for on chain AI inference" and "utilizes the GPU instead of the CPU to execute nontrivial AI models."

**REALITY:** CONFIRMED IN CODE. This is the real thing, and it is the strongest confirmation in the report. The CVM defines two extra opcodes, INFER (0xc0) and INFERARRAY (0xc1), wires them into the live instruction set, validates the referenced model and input, then calls a native inference library that actually executes a quantized neural network and writes the result back into contract memory. The whole path exists end to end and is not a stub.

**EVIDENCE:**
```go
// core/vm/opcodes_infer.go:~20
INFER      OpCode = 0xc0
INFERARRAY OpCode = 0xc1
// NNFORWARD 0xc2  (commented out)
```
```go
// core/vm/jump_table.go:185-192  (opcodes are live in the instruction set)
instructionSet[INFER]      = &operation{ execute: opInfer,      gasCost: gasInfer, ... }
instructionSet[INFERARRAY] = &operation{ execute: opInferArray, gasCost: gasInferArray, ... }
```
```go
// core/vm/instructions_infer.go:92-134  opInfer(...)
modelMeta, err := checkModel(cvm, modelAddr)      // uploaded, mature, not expired, gas <= limit
inputMeta, err := checkInputMeta(cvm, inputAddr)
// shape match enforced (L107-121)
output, err := cvm.Infer(modelMeta.Hash.Hex(), inputMeta.Hash.Hex(), modelMeta.RawSize, inputMeta.RawSize) // L123
scope.Memory.WriteSolidityUint256Array(int64(_outputOffset.Uint64()), output)                              // L128
```
```go
// core/vm/cvm_infer.go:104-121  Infer(...) delegates to the Synapse engine
import "github.com/CortexFoundation/inference/synapse"   // L24
inferRes, errRes = synapse.Engine().InferByInfoHashWithSize(model, input, cvmVersion, cvm.chainConfig.ChainID.Int64()) // L121
```
```go
// inference/synapse/local_infer.go:195-219  the actual native execution
modelJson, _   := s.config.Storagefs.GetFileWithSize(ctx, modelHash, modelSize, SYMBOL_PATH)
modelParams, _ := s.config.Storagefs.GetFileWithSize(ctx, modelHash, modelSize, PARAM_PATH)
model, status  := kernel.New(s.lib, modelJson, modelParams, deviceType, s.config.DeviceId) // L209
result, status  = model.Predict(inputContent, cvmVersion)                                   // L219 (native cgo call)
```
```go
// inference/synapse/synapse.go:61,78-89  the native library that runs the model
lib *kernel.LibCVM
lib, status = kernel.LibOpen(PLUGIN_PATH + PLUGIN_POST_FIX)  // "plugins/" + "libcvm_runtime.so"
```
The native library `libcvm_runtime.so` is built from the `cvm-runtime` repository, a substantial C++ and CUDA machine learning runtime (1,409 commits, with `src/`, `include/`, `kernel/`, and `python/` directories) whose README lists real measured latencies for resnet50, yolo, vgg16, vgg19, mobilenet, squeezenet, and shufflenet across CPU, Jetson Nano, and 1080Ti GPUs.

**IMPACT:** Positive and load bearing. On chain AI inference is not vaporware here. A contract really can reference an uploaded model and an input by their storage hashes, and the node really loads the model into a native runtime and runs a forward pass as part of transaction execution. Very few chains have ever shipped this. Informational (this is the credit side of the report).

---

## Claim 2: Synapse gives deterministic inference, identical across every node

**CLAIM:**
> "A deterministic inference engine that guarantees exactly the same AI inference result across heterogenetic computing environments," enabling "deterministic on chain AI inference without resorting to off chain solutions."

**REALITY:** CONFIRMED IN CODE. Determinism is the whole reason this can be a consensus operation, and the design that delivers it is visible in the code. Models are stored as a fixed symbol graph plus fixed integer parameters and are executed by a single shared native library (`libcvm_runtime.so`) using integer quantized operators, so the forward pass is reproducible rather than floating point dependent. The engine keys its result cache on the RLP hash of the model hash concatenated with the input hash, which only makes sense if a given model plus input is expected to always produce the same output. The opcode gas is also derived deterministically from the model graph rather than from wall clock cost.

**EVIDENCE:**
```go
// inference/synapse/local_infer.go:126  deterministic result key (model + input -> one output)
cacheKey := RLPHashString(modelHash + "_" + inputHash)
if v, ok := s.simpleCache.Load(cacheKey); ok { return v.([]byte), nil }   // L132-136
```
```go
// inference/synapse/local_infer.go:63-86  gas fixed by the model graph, not by runtime
gas, status := kernel.GetModelGasFromGraphFile(s.lib, modelJson)
```
```go
// core/vm/instructions_infer.go:36-64  checkModel enforces maturity so all nodes see the same model state
matureBlockNumber := cvm.ChainConfig().GetMatureBlock()
if blockNum.Int64() > cvm.Context.BlockNumber.Int64()-matureBlockNumber { return nil, ErrMetaInfoNotMature }
if blockNum.Int64() < cvm.Context.BlockNumber.Int64()-params.ExpiredBlks { return nil, errMetaInfoExpired }
```
Model and input files are content addressed and distributed over the project's own torrent file system (`github.com/CortexFoundation/torrentfs`), so every node fetches the identical bytes by infohash before inference.

**IMPACT:** Positive. The determinism claim is honest and is backed by a genuine integer quantized runtime plus content addressed model storage. This is the technical heart of the project and it holds up. Informational.

---

## Claim 3: A live world computer where AI dApps actually run on chain

**CLAIM:**
> "The first decentralized world computer capable of running AI and AI-powered dApps." The site presents on chain inference as a working, in use product.

**REALITY:** OVERSTATED. The mechanism is real (Claims 1 and 2), but there is no verifiable production usage. The only concrete dApp references are old marketing artifacts (a game called Digital Clash and a conceptual AI enhanced CryptoKitties). Both the 2022 and the 2025 official project updates contain zero usage, dApp, or inference statistics; they describe foundational research and development only. The market data reinforces this: a chain with no trades in 24 hours and a roughly $0.19M cap is not hosting meaningful AI dApp activity. The code proves the feature can run; it does not show that anyone is running it.

**EVIDENCE:**
- The inference feature works exactly as coded (Claim 1), but usage is external evidence, and none is available. DappRadar's Cortex page could not be retrieved (HTTP 403), and no block explorer metric, user count, or inference count is published by the project.
- The differentiating opcode has not been meaningfully touched in years. In the CortexTheseus repository the `inference/` directory's last substantive commit is "inference independent" dated 2020-08-14, while the surrounding client keeps merging 2026 go-ethereum changes.

**IMPACT:** The capability is genuine but the "world computer running AI dApps" framing describes adoption that the evidence does not support. Read it as "a chain that can run on chain inference," not "a chain where on chain inference is actively used." High, because the gap between the marketed present tense and the observable reality is large.

---

## Claim 4: An actively developed, cutting edge on chain AI platform

**CLAIM:**
> Ongoing marketing and roadmap language presents Cortex as an actively advancing AI blockchain (TVM and PyTorch frontends, planned LLM support).

**REALITY:** OVERSTATED. The base client is maintained, but the AI specific stack that makes Cortex distinctive is frozen at old versions. The live client does not build against a current inference engine; it pins pseudo versions from 2022 and 2023. In other words, the part of the code that runs AI has not moved in years even though the geth base is current (go 1.25.7). Releases have also slowed markedly (the latest tagged release, v1.10.72 "Liquid", is dated 2025-02-25).

**EVIDENCE:**
```gomod
// go.mod:7   the AI inference engine the live client links is a March 2023 snapshot
github.com/CortexFoundation/inference v1.0.2-0.20230307032835-9197d586a4e8
// go.mod:83  the native model runtime is a November 2022 snapshot
github.com/CortexFoundation/cvm-runtime v0.0.0-20221117094012-b5a251885572 // indirect
```
```go
// go.mod:3   the base is kept modern while the AI dependencies are not
go 1.25.7
```
The pseudo version timestamps (`20230307`, `20221117`) are the load bearing fact: the running node executes an inference engine last updated in early 2023 and a model runtime last updated in late 2022.

**IMPACT:** The base blockchain is maintained, but the AI platform that is the entire point of Cortex is effectively in maintenance freeze. "Actively developed AI platform" overstates a stack whose inference components are pinned to 2022 and 2023. Medium.

---

## Claim 5: ZkMatrix, a launching Layer 2 zkRollup, live at up to 2000 TPS

**CLAIM:**
> "Launching ZkMatrix- Layer2 ZkRollup Solution," "ZkMatrix is a Layer2 Solution on Cortex Blockchain," with a live "Go to ZkMatrix" link to zkmatrix.cortexlabs.ai and a claimed target of up to 2000 TPS.

**REALITY:** FALSE as presented. There is no working ZkMatrix Layer 2. The linked application at zkmatrix.cortexlabs.ai loads a landing shell and then fails with a "Wrong MainNetwork" error, showing no explorer data, no transactions, and no TPS. It offers no mainnet confirmation and no launch date. On the code side there is no dedicated ZkMatrix repository in the CortexFoundation organization at all. The only original ZK code is `zkcvm-mono`, described as "Zk solution for Scaling Cortex," which has a single commit, zero stars, a placeholder README containing only the word ZKCVM, four experimental directories (`axon-vm`, `axon`, `axon_circuits`, `vetric`), and a last push of 2024-06-07. The adjacent `zkml-tvm` is a fork of the TVM deep learning compiler, not a rollup. ZkMatrix was announced in February 2022, listed as "V1 in preparation" in September 2022, and is still referenced only as a "TestNet tool link" in the November 2025 roadmap.

**EVIDENCE:**
- Live site check: zkmatrix.cortexlabs.ai returns "Wrong MainNetwork" with a non functional "Launch App" button and no on chain data.
- Repository check: no `CortexFoundation/zkmatrix` repository exists (probed, 404). The only original ZK repo, `zkcvm-mono`, is a single commit experimental prototype (0 stars, 0 forks, a placeholder README containing only the word ZKCVM, last push 2024-06-07). `zkml-tvm` is a third party TVM fork.
- Timeline: announced Feb 2022, "in preparation" Sept 2022, still "TestNet tool link" as of the Nov 2025 roadmap, with roadmap milestones on the blockchain page only running through 2023 Q4.

**IMPACT:** A Layer 2 marketed with present tense "launching" language and a live app link, but which returns an error and has no mainnet, no working rollup code in the open repositories, and a years long "in preparation" status, is not a real product. The 2000 TPS figure is an unverified target, not a measured live throughput. High.

---

## Additional Note: The model storage layer is real

Worth stating because it supports Claim 1. Cortex did not fake the hard dependency either. Models and inputs are not pasted into calldata; they are content addressed blobs distributed by the project's own peer to peer file system, `github.com/CortexFoundation/torrentfs` (a maintained Go repository, 69 stars, active in 2026). The CVM references them by infohash and the Synapse engine fetches the symbol graph, the parameters, and the input by that hash before running inference. This is a coherent, genuinely built storage plus compute design, not a facade.

**IMPACT:** Positive design disclosure. It is part of why the on chain inference claim is credible rather than cosmetic. Low.

---

## Conclusion

Cortex is the rare subject where the flagship, hardest to fake claim is the one that is real. On chain AI inference through the CVM's INFER opcodes and the Synapse engine genuinely exists, is wired into the live instruction set, and executes quantized models through a real native C++ and CUDA runtime with reproducible, content addressed inputs. Determinism, the property that lets inference be a consensus operation, is honestly implemented. Two claims are confirmed in code and they are the important ones.

The overstatements are about time and adoption, not about whether the core idea was built. The AI stack is frozen at 2022 and 2023 versions while the geth base ticks forward, there is no verifiable production usage of the inference feature, and the coin that powers it has been delisted by Binance and OKX and now trades at a fraction of a cent with no daily volume. The one outright false present tense claim is ZkMatrix: marketed as a launching Layer 2 zkRollup with a live app link, it returns a "Wrong MainNetwork" error, has no mainnet, and is backed in open source only by a single commit experimental prototype.

None of this is a scam in the ICE sense. There are no hidden backdoors, no plaintext keys, no bypassed security checks. It is an older, pioneering project whose engineering was real and whose marketing has not been updated to match a dormant network. Score 58 out of 100, MEDIUM RISK, driven by staleness, absent usage, a delisted coin, and one false "live" Layer 2 claim, and lifted well above the scam range by a genuinely implemented inference engine.

| Claim | Verdict |
|-------|---------|
| CVM runs on chain AI inference via Synapse | CONFIRMED IN CODE |
| Deterministic inference identical across nodes | CONFIRMED IN CODE |
| Live world computer with AI dApps actually running | OVERSTATED (no verifiable usage; feature dormant) |
| Actively developed cutting edge AI platform | OVERSTATED (inference stack pinned to 2022 and 2023) |
| ZkMatrix live Layer 2 zkRollup at up to 2000 TPS | FALSE (error page, no mainnet, single commit prototype) |

Tally: CONFIRMED IN CODE 2, OVERSTATED 2, FALSE 1.

---

## Verification and Sources (exact repositories and files read)

Client and CVM (CortexTheseus, branch master):
- core/vm/opcodes_infer.go (INFER 0xc0, INFERARRAY 0xc1, NNFORWARD 0xc2 commented out)
- core/vm/jump_table.go (INFER and INFERARRAY wired into instructionSet, lines 185 to 192)
- core/vm/instructions_infer.go (opInfer, opInferArray, checkModel, checkInputMeta, shape checks)
- core/vm/cvm_infer.go (Infer, InferArray, OpsInfer delegate to synapse.Engine(); import inference/synapse at line 24)
- core/vm/cvm.go (the EVM renamed to CVM; go-ethereum fork provenance)
- go.mod (module github.com/CortexFoundation/CortexTheseus; go 1.25.7; inference pinned v1.0.2-0.20230307032835; cvm-runtime pinned v0.0.0-20221117094012)
- recent commit stream: upstream go-ethereum merges #35084, #35077, #34985, #35037
- inference/ directory last substantive commit "inference independent" 2020-08-14
- latest release v1.10.72 "Liquid" 2025-02-25

Inference engine (inference, branch master):
- synapse/synapse.go (Engine singleton, LibCVM, kernel.LibOpen of plugins/libcvm_runtime.so, remote vs local branch)
- synapse/local_infer.go (infer(): fetch symbol/params/input from torrentfs by infohash, kernel.New line 209, model.Predict line 219, deterministic cache key line 126, GetModelGasFromGraphFile)
- synapse/remote_infer.go, errors.go, result.go, utils.go (present)

Native runtime (cvm-runtime, branch master):
- repository structure src/, include/, kernel/, python/; 1,409 commits; README latency table (resnet50, yolo, vgg16, vgg19, mobilenet, squeezenet, shufflenet on CPU/Jetson/1080Ti)
- consumed by the live client at the November 2022 pseudo version

Storage:
- torrentfs (peer to peer content addressed model and input storage, active in 2026)

ZK and Layer 2:
- zkcvm-mono ("Zk solution for Scaling Cortex", 1 commit, 0 stars, placeholder README containing only the word ZKCVM, dirs axon-vm/axon/axon_circuits/vetric, last push 2024-06-07)
- zkml-tvm (third party TVM compiler fork)
- no CortexFoundation/zkmatrix repository (probed, 404)
- zkmatrix.cortexlabs.ai returns "Wrong MainNetwork", no mainnet, no explorer data

Market and public record:
- cortexlabs.ai (CVM, Synapse, and ZkMatrix marketing wording)
- Binance delisting announcement, CTXC in the first Vote to Delist batch, effective 2025-04-16
- OKX delisting of CTXC spot pairs effective 2025-06-20 (low volume and liquidity)
- CoinGecko cortex page and API: price roughly $0.0008, cap roughly $0.19M, rank roughly #4,800, no trades in 24h, no active tickers, all time high $2.39 on 2018-04-29, all time low 2026-05-31
- Cortex project updates and half year roadmap (Sept 2022 update #105; Nov 2025 roadmap): ZkMatrix "in preparation" then "TestNet tool link"; no usage or dApp statistics

---

## Disclaimer

This report documents the relationship between Cortex's public marketing claims and its publicly available open source code. All findings are based on source code read from the CortexFoundation GitHub organization, on the project's own site and updates, and on public exchange and market records. Cortex is an older project whose core on chain inference engine is genuinely implemented; this review credits what the code delivers and flags where the marketing runs ahead of a now dormant network. Read only review, no systems were accessed or modified.

**Report Date:** 2026-08-05
**Website:** https://mefai.io
