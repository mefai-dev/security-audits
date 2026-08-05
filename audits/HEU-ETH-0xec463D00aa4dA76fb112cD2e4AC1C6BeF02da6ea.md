# Heurist (HEU): Whitepaper Claims vs Code Reality

**Score: 70/100, MEDIUM RISK**

**Date:** 2026-08-05
**Token:** HEU (ERC20, 18 decimals, maximum supply 1,000,000,000, canonical on Ethereum mainnet 0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea; bridged copies on ZKsync Era and Base)
**Networks referenced by the code:** Ethereum mainnet (token), ZKsync Era Sepolia testnet (miner identity registry), Base (staking and payment)
**Websites:** heurist.ai, docs.heurist.ai
**GitHub:** github.com/heurist-network

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

Most of the projects we review sell a security story that their own code contradicts. Heurist is a more honest subject than most. There are no `if true` bypasses, no private keys checked into the repository, no fake cryptography. The open source miner genuinely loads real models onto real GPUs and serves inference. The purpose here is the same discipline applied fairly. We read the actual public source, claim by claim, credit what the code truly delivers, and flag where the word "decentralized" in the marketing runs ahead of an architecture that is, in practice, a central coordinator with off chain accounting and a token controlled by one key.

We are not making accusations. We are not spreading FUD. We read the source and show what is actually implemented.

## Method

For every major marketing and documentation claim we located the relevant code in the public repository (heurist-network/miner-release), fetched raw files from GitHub, read the deployed HEU token source, and queried the live token contract over a public Ethereum RPC. Each claim below is labelled CONFIRMED IN CODE, OVERSTATED, or FALSE, with a file, line, and a short verbatim snippet. Note at the outset which code is public: only the miner client is open source. The two components that actually run the network, the job dispatcher named in the config as the "sequencer" and the LLM gateway, are closed source and were not available to read.

---

## The Foundation: A Real Compute Network Under a Central Coordinator

**CLAIM:**
> Heurist is a decentralized AI inference network where independent GPU miners serve image and LLM models.

**REALITY:** Half confirmed, half overstated, and best stated plainly up front. The miners are real and independent (Claim 1). The network that coordinates them is not decentralized: every job is requested from, and every result is submitted to, a single central server that the project's own configuration calls the sequencer, over plain HTTP. Image outputs are uploaded to a Heurist owned Amazon S3 bucket. There is no peer to peer job market, no on chain job book, and no on chain settlement in the loop.

**EVIDENCE:**
```toml
# miner-release/config.toml:1-4,17
[service]
base_url = "http://sequencer.heurist.xyz"
llm_url  = "http://localhost"
signal_url = "https://d2k7cjzmjgpm6p.cloudfront.net/prod"
# [storage]
s3_bucket = 'heurist-images'
```
```python
# miner-release/llm_mining_core/utils/requests_utils.py:38,56
url = f"{config.base_url}/miner_request"          # ask the central sequencer for a job
response = config.session.post(url, json=request_data)
```
Repository enumeration of the heurist-network organization returns miner-release but no `sequencer`, `heurist-gateway`, or `llm-gateway` repository. The orchestrator is not public.

**IMPACT:** The physical compute is genuinely distributed across independent operators, which is real and creditable. The control plane is a single operator run server. This is a centrally orchestrated distributed compute pool, which is materially different from the trustless, decentralized network the marketing implies. Informational at the foundation level; the specific consequences are scored in the claims below.

---

## Claim 1: GPU miners actually run image and LLM models on independent hardware

**CLAIM:**
> "Contribute your GPU to perform AI inference tasks on the Heurist network." Miners host Stable Diffusion and Large Language Models.

**REALITY:** CONFIRMED IN CODE. This is the strongest and most honest part of the project. The LLM miner launches a real vLLM OpenAI compatible inference server against a Hugging Face model on the operator's own GPU, then runs genuine chat completions. The image miner loads Stable Diffusion and Flux pipelines and generates images. Nothing about the inference is faked or stubbed.

**EVIDENCE:**
```python
# miner-release/llm_mining_core/config/server.py:31-42  start_llm_server()
cmd = [
    "python", "-m", "vllm.entrypoints.openai.api_server",
    "--model", self.model_id,
    "--served-model-name", self.served_model_name,
    "--tensor-parallel-size", str(self.num_gpus),
    "--gpu-memory-utilization", self.gpu_memory_util,
]
self.process = subprocess.Popen(cmd)
```
```python
# miner-release/llm-miner.py:153-166  real inference against the local vLLM server
response = client.chat.completions.create(**params)
chat_completion = ChatCompletion(**response.model_dump())
total_tokens = response.usage.total_tokens
```
```python
# miner-release/sd_mining_core/utils/request_utils.py:53  real diffusion inference
image_data, inference_latency, loading_latency = execute_model(
    config, job['model_id'], job['model_input']['SD']['prompt'], ... )
```

**IMPACT:** Positive. Operators run their own hardware, pull their own model weights, and produce real inference output. The load bearing claim that this is a working GPU compute network is true. Informational.

---

## Claim 2: It is a decentralized inference network

**CLAIM:**
> Credits "are used to access Heurist's decentralized AI compute resources." The network is decentralized.

**REALITY:** OVERSTATED. Independent workers do the compute, but the entire task lifecycle is mediated by one central sequencer. The miner polls the sequencer for a job, the sequencer supplies the full model input (prompt, temperature, seed, max tokens), the miner returns the result to the sequencer, and image results are pushed to a Heurist owned S3 bucket using temporary AWS credentials handed down for the job. The worker never talks to an end user and never touches a decentralized ledger during the job. If the round trip is slow the miner is simply told it earned nothing, by the same central server.

**EVIDENCE:**
```python
# miner-release/llm-miner.py:210-221  the sequencer assigns work and its parameters
job, request_latency = send_miner_request(base_config, miner_id, base_config.served_model_name)
prompt      = job['model_input']['LLM']['prompt']
temperature = job['model_input']['LLM']['temperature']
```
```python
# miner-release/sd_mining_core/utils/request_utils.py:48-57,84  result to S3 + central endpoint
s3 = boto3.client('s3', aws_access_key_id=temp_credentials[0], ... )
upload_image_to_s3(s3, image_data, config.s3_bucket, s3_key)   # bucket 'heurist-images'
response = requests.post(config.base_url + "/miner_submit", json=result)
```
```python
# miner-release/llm-miner.py:252  reward verdict comes from the central server
"Warning: the previous request timed out. You will not earn points."
```

**IMPACT:** The compute is distributed; the coordination, the data path, and the accept or reject decision are centralized in one server reached over unencrypted HTTP. "Decentralized AI compute" is accurate about the GPUs and overstated about the network that runs them. Medium.

---

## Claim 3: An OpenAI compatible LLM gateway serving decentralized LLMs

**CLAIM:**
> "Heurist LLM Gateway ... fully compatible with the OpenAI SDK, allowing you to use the same familiar interface to interact with decentralized LLMs." "Decentralized AI at Low Cost." Replace the base URL with `https://llm-gateway.heurist.xyz`.

**REALITY:** OVERSTATED. The OpenAI compatibility is real and useful, and worth crediting: a caller points the OpenAI SDK at one Heurist URL and gets streaming chat completions back. But the gateway is a central reverse proxy, not a decentralized endpoint. Traffic flows user to the Heurist gateway to the Heurist sequencer to a miner and back. The miner even streams tokens back to the sequencer, not to the caller. The gateway and sequencer code that perform routing, authentication, metering, and payment are closed source, so the "decentralized" part of this claim cannot be verified and the observable design is a single branded proxy.

**EVIDENCE:**
```python
# miner-release/llm-miner.py:118-123  miner streams the answer back to the sequencer
response = session.post(
    f"{base_config.base_url}/miner_submit_stream",
    headers={'job_id': str(job_id), 'miner_id': str(miner_id), ...},
    data=generate_data(stream), stream=True)
```
```toml
# miner-release/config.toml:2   the only "gateway" the miner knows is the central sequencer
base_url = "http://sequencer.heurist.xyz"
```
Docs, llm-gateway/introduction.md:9,13-14: single ingress `https://llm-gateway.heurist.xyz`, marketed as "decentralized LLMs" and "Decentralized AI at Low Cost." No gateway source is published in the org.

**IMPACT:** Users get a real OpenAI drop in, backed by a real fleet of GPUs, but they are trusting one company's proxy for routing, billing, and the integrity of the returned completion. "Decentralized LLMs" describes the supply side, not the endpoint the user actually calls. Medium.

---

## Claim 4: HEU pays for inference and rewards miners on chain

**CLAIM:**
> HEU is used to "Pay for AI services" and to reward miners; the network settles inference in HEU. Documentation tells miners "we are tracking your rewards behind the scenes."

**REALITY:** FALSE as an on chain settlement claim. Nothing in the miner pays HEU, transfers a token, or settles a job on any chain. Rewards are off chain points computed by the central sequencer and shown on a web portal. The historical HEU that miners received was distributed as two one time snapshot airdrops (Phase 1 fifty million, Phase 2 ten million), and the documentation now states plainly that mining is over. The only blockchain the miner touches during operation is a ZKsync Era Sepolia testnet contract used purely to read an identity binding (Claim 5); that contract's ABI contains no payment, reward, escrow, or settlement function at all.

**EVIDENCE:**
```toml
# miner-release/config.toml:74-76   the sole on chain contract the miner uses is on a TESTNET
[contract]
rpc = "https://sepolia.era.zksync.dev/"
address = "0x7798de1aE119b76037299F9B063e39760D530C10"
```
```
# auth/abi.json  full function set (ERC-721 identity registry, no value transfer of HEU):
bind(address,address), mintAndBind(address,address), identityAddress(address),
rewardAddress(address), tokenURI, ownerOf, transferFrom, owner ...
# there is no pay(), settle(), claimRewards(), or HEU transfer anywhere in the miner
```
```md
# docs/protocol-overview/tokenomics.md:39-43
Phase 1: 50M HEU airdropped ... snapshot ... July 19th, 2024
Phase 2: 10M HEU airdropped ...
Mining is no longer available. Stay tuned for future reward opportunities.
```

**IMPACT:** The phrase "HEU pays for inference and rewards miners" describes an off chain points ledger operated by Heurist plus discretionary token airdrops that have already ended, not a protocol that settles inference in HEU on a blockchain. A miner's earnings depend entirely on trusting the central sequencer's private accounting. This is the single largest gap between the token utility narrative and the code. High.

---

## Claim 5: Miner identity and authentication are secured on chain

**CLAIM:**
> Miner identity is bound on chain; work is cryptographically attributed to a reward wallet.

**REALITY:** OVERSTATED. There is a genuine on chain identity registry, and cryptographic signing is genuine, but two caveats matter. First, the registry the miner reads and the documentation tells operators to write to is deployed on ZKsync Era Sepolia, a testnet, not on any mainnet. Second, the per submission signature (a personal_sign over `rewardwallet-hourlytimestamp`) is verified off chain by the same central sequencer that pays the rewards; the chain is used to look up a binding, not to verify or settle anything. The identity wallet is explicitly described in code as authentication only.

**EVIDENCE:**
```python
# miner-release/auth/generator.py:50-51,161-165  on chain read + off chain signature
def fetch_iw_address(self, rw_address):
    return self.contract.functions.identityAddress(Web3.to_checksum_address(rw_address)).call()
message = f"{reward_wallet}-{hourly_time}"                 # signed with the identity key
signature = self.w3.eth.account.sign_message(encode_defunct(text=message), private_key=private_key)
```
```python
# miner-release/auth/generator.py:38  the on chain identity wallet is auth only
"... The identity wallet is used for authentication only. DO NOT deposit funds into the identity wallet."
```
```python
# miner-release/auth/generator.py:187  operators are pointed at the ZKsync testnet contract
"https://zksync.explorer/0x7798de1aE119b76037299F9B063e39760D530C10/writeContract"
```

**IMPACT:** The identity model is real and reasonable, but "secured on chain" is generous for a binding registry on a public testnet whose signatures are trusted and settled by a central server. It authenticates who a miner is; it does not decentralize trust in the reward accounting. Medium.

---

## Claim 6: HEU has a fixed maximum supply of one billion

**CLAIM:**
> "The maximum supply is 1,000,000,000." HEU is a fixed supply asset.

**REALITY:** CONFIRMED IN CODE. The deployed Ethereum token enforces a hard cap of one billion units and the cap is already fully minted, so no further inflation is possible without first reducing supply. Live reads of the contract confirm the numbers exactly.

**EVIDENCE:**
```solidity
// HEU.sol (verified source, Ethereum 0xec463D...da6ea)
uint256 public constant MAXIMUM_SUPPLY = 1_000_000_000e18;
function mint(address recipient, uint256 amount) external onlyOwner {
    if (totalSupply() + amount > MAXIMUM_SUPPLY) revert HEU__CanNotExceedMaximumSupply();
    _mint(recipient, amount);
}
constructor() ERC20("Heurist", "HEU") Ownable(msg.sender) {}
```
```
# live eth_call on mainnet
name()        = "Heurist"
symbol()      = "HEU"
decimals()    = 18
totalSupply() = 1,000,000,000e18   (0x033b2e3c9fd0803ce8000000)  == MAXIMUM_SUPPLY  -> mint exhausted
```

**IMPACT:** Positive. The fixed supply claim is true and the mint is exhausted at the cap. There is no live inflation path today. Informational.

---

## Claim 7: HEU secures a decentralized protocol

**CLAIM:**
> HEU is used to "Stake to secure the network," "Vote on governance decisions," and the protocol is decentralized.

**REALITY:** OVERSTATED. The token is administered by a single externally owned account. The mint function is onlyOwner, ownership is transferable, and, notably, the standard escape hatch is disabled: renounceOwnership is overridden to revert, so control can never be relinquished to nobody. The owner address holds no contract code, confirming it is a plain private key rather than a multisig or timelock. Staking rewards advertised as "50% APR from protocol emissions" cannot come from minting, because the cap is exhausted (Claim 6); they are paid from a pre minted allocation held and released centrally.

**EVIDENCE:**
```solidity
// HEU.sol  ownership cannot be renounced; owner is a single key
function renounceOwnership() public view override onlyOwner { revert HEU__RenounceOwnershipIsNotAllowed(); }
```
```
# live checks
owner()                              = 0xfB93bEE230a72a241534F70d85b76E07f35cd33f
eth_getCode(0xfB93bEE2...cd33f)      = 0x            -> externally owned account, not a contract
```
```md
# docs/protocol-overview/tokenomics.md:47-53 ; stake.md:17
Base rewards: 50% APR from protocol emissions ; auto-compounding stHEU ; 30-day unstake lockup
```

**IMPACT:** No live inflation is a genuine positive, but "secure the network" and "governance" sit on top of a token whose administrative key belongs to one account that can never renounce control and that hand distributes the staking and mining allocation. The security and governance narrative is centralized in practice. High.

---

## Claim 8: Heurist Chain is a live ZK Layer 2 and HEU is its gas token

**CLAIM:**
> "Heurist Chain: Our ZK Layer 2 infrastructure powers the payment rails for autonomous AI systems." HEU serves "as the gas token for transactions in the Heurist Chain."

**REALITY:** OVERSTATED and forward looking. Nothing in the public source demonstrates a live Heurist Chain L2 or HEU functioning as its gas token. The only chain the miner code interacts with is a ZKsync Era Sepolia testnet. Payment and staking contracts referenced in the docs live on Base and other existing chains, not on a Heurist operated L2. The L2 as a payment rail is presented in the present tense in marketing while the reviewable code treats it as testnet stage or unbuilt.

**EVIDENCE:**
```md
# docs/introduction.md:21
Heurist Chain: Our ZK Layer 2 infrastructure powers the payment rails for autonomous AI systems.
# docs/protocol-overview/tokenomics.md:9 (utility) : "gas token for transactions in the Heurist Chain"
```
```toml
# miner-release/config.toml:75  the only chain the code actually points at is a testnet
rpc = "https://sepolia.era.zksync.dev/"
```

**IMPACT:** A reasonable roadmap item stated as a shipped product. Anyone reading "gas token for the Heurist Chain" should understand it as aspirational relative to the code available today. Low.

---

## Additional Note: the trust critical components are closed source

Worth stating plainly because it underlies Claims 2, 3, and 4. The miner client is fully open, but the sequencer that dispatches every job and computes every reward, and the gateway that fronts every user request, are not published in the heurist-network organization. The economic heart of the system, who gets which job and who is credited how much HEU, runs in code no one outside Heurist can read. The open miner tells us the workers are honest machinery; it does not let anyone verify the accounting that pays them.

---

## Conclusion

Heurist is a real and comparatively honest project. The miner genuinely runs Stable Diffusion, Flux, and vLLM served LLMs on independent operator GPUs (Claim 1), the OpenAI compatible gateway is a real drop in (Claim 3), and HEU is a genuine fixed supply asset whose mint is exhausted at one billion (Claim 6). There are no planted bypasses, no leaked keys, and no faked cryptography. That is why this scores well above the projects whose code contradicts their whitepaper.

The overstatements are all variations on one theme: the marketing word "decentralized" describes a system whose control plane is centralized. Every job is dispatched and every reward is decided by a single central sequencer reached over plain HTTP, with image results parked in a Heurist S3 bucket (Claim 2). The user facing gateway is a single branded proxy (Claim 3). "HEU pays for inference and rewards miners" is, in the code, an off chain points ledger plus two already finished airdrops, with the only in loop blockchain being a testnet identity registry that has no payment function at all (Claim 4, the most serious gap, and mining is now discontinued). Miner identity is bound on a testnet and verified off chain (Claim 5). The token, while non inflationary today, is owned by a single externally owned account that can never renounce control (Claim 7), and the advertised ZK Layer 2 gas token is not evidenced in the reviewable code (Claim 8). Finally, the two components that actually run the network are closed source, so the accounting cannot be independently audited.

None of this is fraud. It is the difference between a working, distributed GPU compute product with a centralized coordinator and off chain settlement, and the trustless decentralized network the branding implies. Score 70 out of 100, MEDIUM RISK, driven by centralization of the control plane, off chain reward accounting, a single key token owner, and closed source orchestration, not by any dishonest or broken code.

| Claim | Verdict |
|-------|---------|
| GPU miners run real image and LLM models on independent hardware | CONFIRMED IN CODE |
| It is a decentralized inference network | OVERSTATED (central sequencer dispatch, S3, off chain accept or reject) |
| OpenAI compatible gateway serving decentralized LLMs | OVERSTATED (real OpenAI drop in, but a central proxy; orchestrator closed source) |
| HEU pays for inference and rewards miners on chain | FALSE (off chain points, finished airdrops, testnet identity registry only) |
| Miner identity and auth are secured on chain | OVERSTATED (real registry, but on a testnet and verified off chain) |
| HEU fixed maximum supply of one billion | CONFIRMED IN CODE (mint exhausted at cap) |
| HEU secures a decentralized protocol | OVERSTATED (single permanent EOA owner, onlyOwner mint) |
| Heurist Chain live ZK L2 with HEU gas token | OVERSTATED (aspirational; only a testnet in code) |

Tally: CONFIRMED IN CODE 2, OVERSTATED 5, FALSE 1.

---

## Verification and Sources (exact files read)

Miner client (heurist-network/miner-release, branch main):
- config.toml (base_url sequencer.heurist.xyz, signal_url CloudFront, s3_bucket heurist-images, [contract] ZKsync Sepolia 0x7798...)
- llm-miner.py (worker loop, send_miner_request, chat.completions.create, /miner_submit and /miner_submit_stream, signature attach)
- llm_mining_core/config/server.py (start_llm_server -> vllm.entrypoints.openai.api_server, health_check)
- llm_mining_core/config/base.py (base_url, signal_url, session)
- llm_mining_core/utils/requests_utils.py (send_miner_request -> /miner_request, send_model_info_signal -> /miner_signal)
- sd-miner.py, sd_mining_core/base/config.py, sd_mining_core/utils/request_utils.py (S3 upload via boto3, submit_job_result -> /miner_submit, "you will not earn points")
- auth/generator.py (local seed phrase identity wallet, identityAddress read, personal_sign of rewardwallet-hourlytimestamp, "authentication only", ZKsync testnet bind instructions)
- auth/abi.json (ERC-721 identity registry: bind, mintAndBind, identityAddress, rewardAddress; no payment or settlement function)

Token (Ethereum mainnet 0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea):
- verified HEU.sol source (MAXIMUM_SUPPLY 1_000_000_000e18, mint onlyOwner, Ownable(msg.sender), renounceOwnership reverts)
- live eth_call reads: name "Heurist", symbol "HEU", decimals 18, totalSupply == MAXIMUM_SUPPLY, owner 0xfB93bEE230a72a241534F70d85b76E07f35cd33f, eth_getCode of owner = 0x (externally owned account)

Documentation (docs.heurist.ai):
- introduction.md (Heurist Chain ZK L2 payment rails)
- llm-gateway/introduction.md (OpenAI compatible, "decentralized LLMs", llm-gateway.heurist.xyz)
- protocol-overview/tokenomics.md (utilities, distribution, Phase 1 and Phase 2 airdrops, "Mining is no longer available", 50% APR emissions)
- protocol-overview/stake.md, credits.md ("decentralized AI compute resources"), contract-addresses.md (HEU on ETH, ZKsync, Base; staking and payment on Base and others)

Repository enumeration: heurist-network organization public repos include miner-release; no public sequencer, heurist-gateway, or llm-gateway repository was found.

---

## Disclaimer

This report documents the relationship between Heurist's public marketing and documentation claims and its publicly available open source code and deployed contracts. All findings are based on source read from the heurist-network GitHub organization, the verified HEU token source, the project documentation, and read only calls to a public Ethereum RPC node. Heurist is a genuine, working project; this review credits what the code delivers and flags where the marketing language runs ahead of the implementation. Read only review; no systems were accessed or modified.

**Report Date:** 2026-08-05
