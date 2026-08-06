# io.net: Whitepaper Claims vs Code Reality
**Score: 73/100, MEDIUM (Passed)**
Date: 2026-08-06

Token (live, independently found and confirmed on chain):
- SPL mint `BZLbGTNCSFfoth2GYDtwr7e4imWzpR5jqcUuGEwr646K`, cross confirmed via Solana RPC `getAccountInfo`, DexScreener, and CoinGecko contract lookup.
- Program owner `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA`, the standard SPL Token program. This is not Token 2022, so no transfer fee, no transfer hook, and no on chain tax are possible.
- Decimals: 8.
- mintAuthority: `null`. Minting is renounced, so no new IO can ever be created on chain.
- freezeAuthority: `null`. Holder balances cannot be frozen or seized. No single key can lock user tokens.
- On chain supply: 79878885501046031 base units, which is 798,788,855.01 IO, roughly 99.85 percent of the 800,000,000 max supply already minted.
- Circulating supply about 378.4M IO, roughly 47.4 percent of total. The remaining roughly 420M sits outside circulation in centrally held reserve and reward pools.
- Price about 0.1249 USD, market cap about 47.26M USD, FDV about 99.76M USD, market cap rank about 437, 24h volume about 5.72M USD across centralized venues.
- All time high 6.43 USD, so the token trades near its all time low of about 0.093 USD, down roughly 98 percent from peak.
- Deepest on chain pool is Raydium IO/USDC CLMM at about 62K USD liquidity. Total on chain DEX liquidity across 16 pools is only about 72.7K USD. Reported off chain volume concentrates on Binance, Upbit, LBank, WhiteBIT, Pionex, and BTCC.

Websites:
- Product and app: io.net
- Docs: io.net/docs (redirected from docs.io.net), machine index at io.net/docs/llms.txt
- GitHub organization: github.com/ionet-official

### Verified in code and on chain (MEFAI deep source review)
- On chain mint state is clean and independently reproduced. `getAccountInfo` on the mint returns `mintAuthority: null`, `freezeAuthority: null`, `decimals: 8`, `supply: 79878885501046031`, owner `TokenkegQ...` standard SPL Token. Renounced mint and renounced freeze are the strongest token level facts here.
- The 800M hard cap is enforced on chain by the renounced mint authority. Because minting is dead, the whitepaper 300M reward emission is not fresh on chain minting. It is distribution from a pre minted reserve, since 99.85 percent of the cap is already minted and sits in treasury and reward pools.
- The public GitHub org `ionet-official` holds 9 repositories. The ones fetched are supporting pieces, not the core: `docs` (Mintlify MDX), `io_launch_binaries` (precompiled worker launch binaries for Linux, macOS, and Windows), `io-net-official-setup-script` (shell), `cc-attestation-agent-api` (FastAPI remote attestation for Intel TDX and NVIDIA H200 confidential VMs, MIT), `io-ray-serve-chat-demo` (a Ray Serve demo, Apache 2.0), `speaches`, `io-chatbot`, `monitor-image-releases`, and `vc-version`.
- `io_launch_binaries` confirms the worker is shipped as precompiled executables (`io_net_launch_binary_linux`, `io_net_launch_binary_mac`, `io_net_launch_binary_windows.exe`) that pull and run Docker containers. The compute orchestration, reward accounting, and device verification logic are closed source.
- Docs (io.net/docs/llms.txt) describe IO Cloud (VM on demand, containers, bare metal, Kubernetes), IO Worker onboarding, clustering built on Ray, a Proof of Work plus device reliability scoring approach, Solana or Aptos wallet linkage, staking with solo and co staking, and the 300M over 20 years emission moving from an hourly to a monthly disinflation schedule.
- Market and liquidity independently pulled from DexScreener and CoinGecko. The mismatch between about 47M USD market cap and about 72.7K USD of on chain DEX liquidity is real and is the sharpest on chain risk for anyone transacting on Solana rather than on a centralized exchange.

### Claim vs reality
- Claim: a decentralized network of pooled GPUs and CPUs (IO Cloud and IO Worker). Verdict: OVERSTATED. The product is real and worker onboarding is documented and shippable, but the orchestration, scheduling, and reward layer is closed source binaries and centrally operated, and roughly 52.6 percent of supply is centrally held. Decentralization of hardware supply does not equal decentralization of control.
- Claim: Ray based clustering for distributed workloads. Verdict: CONFIRMED with a transparency caveat. Ray is a genuine open source dependency (Anyscale) and io.net ships a working `io-ray-serve-chat-demo`. The proprietary glue that turns supplied devices into billable clusters is not published.
- Claim: Proof of Work and verification guarantee that supplied GPUs are genuine and perform as intended. Verdict: OVERSTATED. Docs assert active verification and a device reliability score, but the verification logic is closed and unauditable, and this exact layer was bypassed in the April 2024 fake GPU spoofing incident where attackers injected large numbers of counterfeit device entries. The open `cc-attestation-agent-api` provides real hardware attestation, but only for the confidential VM path, not the whole fleet.
- Claim: IO tokenomics, 800M max supply with a capped 300M reward emission. Verdict: CONFIRMED IN CODE. The 800M cap is hard enforced because mintAuthority is null and 99.85 percent is already minted. The emission is a distribution schedule from pre minted reserves, not open ended inflation.
- Claim: holders keep custody and control. Verdict: CONFIRMED. Standard SPL token, mint and freeze both renounced, no tax and no transfer hook, so no key can seize, freeze, dilute, or tax holders.

### Severity
- High: core compute, reward, and verification stack is closed source and centrally operated, and the historical fake GPU spoofing incident hit exactly this unauditable layer. The claim of trustless verification cannot be independently checked.
- Medium: on chain DEX liquidity is extremely thin, about 72.7K USD total against a roughly 47M USD market cap, so on chain trades face severe slippage and price manipulation risk even though centralized venues are deeper.
- Medium: about 52.6 percent of supply, roughly 420M IO, is not circulating and is centrally controlled for reserves and rewards, a standing distribution and sell pressure overhang.
- Low: price sits near the all time low, down roughly 98 percent from the 6.43 USD high, and LP lock status for the shallow Solana pools is unverified.
- Informational: token contract itself is materially safe. Renounced mint, renounced freeze, standard SPL Token program, no tax. The April 2024 incident was a network integrity and metadata problem, not a token exploit, and the current on chain mint state shows no compromise.
