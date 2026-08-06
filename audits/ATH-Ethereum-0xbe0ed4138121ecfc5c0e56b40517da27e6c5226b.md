# Aethir: Whitepaper Claims vs Code Reality
**Score: 72/100, MEDIUM (Passed)**
Date: 2026-08-06

Token (live, independently found and confirmed onchain):
- Symbol ATH, ERC20. Primary canonical contract on Ethereum: `0xbe0ed4138121ecfc5c0e56b40517da27e6c5226b`. Verified source read on chain: contract name `AethirToken`, Solidity 0.8.18, `ERC20Permit, Ownable`, not a proxy (immutable logic).
- Also deployed on Arbitrum One `0xc87b37a581ec3257b734886d9d3a581f5a9d056c` (GoPlus is_proxy = 1, an upgradeable proxy, roughly 3.2B supply, about 140,502 holders, 0 tax) and Solana `Dm5BxyMetG3Aq5PaG1BrG7rBYqEMtnkjvPNMExfacVk7`.
- Owner of the Ethereum token: `0x1246ae66c4c5e51f0b985f8e6716481c2eb422bd`. This is a contract, not an EOA. It was created by `0xa6b71e26c5e0845f74c812102ca7114b6a896ab2` (the canonical Gnosis Safe Proxy Factory 1.3.0, itself deployed deterministically via the Arachnid CREATE2 deployer `0x4e59b44847b379578588920ca78fbf26c0b4956c`), so the mint owner is a Gnosis Safe 2 of 3 multisig, not a single key.
- Supply: total = max = 42,000,000,000 ATH and already fully minted. Circulating roughly 20.13B. `mint(uint256) onlyOwner` is hard capped by `require(totalSupply() + amount <= MAX_SUPPLY)` with `MAX_SUPPLY = 42_000_000_000e18`; since total already equals max, mint is exhausted, and because there is no burn function total supply can never fall, so no further minting is ever possible. Supply is effectively permanently fixed.
- Market (live): price about $0.00403, market cap about $81.2M, FDV about $169.5M, market cap rank about #291, 24h volume about $5.3M. Deepest venues are centralized: LBank ATH/USDT (about $1.76M per 24h), then HTX and CoinW; deepest on chain pool is Uniswap V3 ATH to WETH.
- Safety checks: GoPlus reports no honeypot, buy/sell/transfer tax all 0, not pausable, no blacklist, non proxy on Ethereum, is_mintable = 1, is_whitelisted = 1. honeypot.is reports not a honeypot, risk level low, simulation success, and 0 failed sells across 1,805 sampled holders. About 54,808 holders on Ethereum.

Websites:
- https://aethir.com
- https://docs.aethir.com
- GitHub organization: https://github.com/AethirCloud

### Verified in code and on chain (MEFAI deep source review)
- Ethereum ATH source was fetched and read (via Blockscout getsourcecode). It is a compact OpenZeppelin based `ERC20Permit, Ownable` token. Confirmed features: `mint(uint256 amount) public onlyOwner` with a hard `MAX_SUPPLY` cap of 42B; no `Pausable`, no blacklist, no transfer fee or tax, no `ERC20Capped`, and no burn function. A whitelist governs treasury distribution only: `mint` sends new tokens to the contract itself and `transferToWhitelisted` lets the owner push those treasury tokens to pre approved addresses up to a per address cap. AethirToken does not override `transfer` or `_beforeTokenTransfer`, so ordinary holder to holder transfers are unrestricted, which matches honeypot.is showing 1,805 of 1,805 sampled holders able to sell. This matches GoPlus is_whitelisted = 1.
- Owner is a Gnosis Safe multisig (confirmed by tracing the owner contract creator to the Safe Proxy Factory 1.3.0). This removes the single EOA mint risk but the multisig can still call `mint` (currently inert at the cap) and can configure the treasury distribution whitelist.
- Arbitrum deployment is a verified but upgradeable proxy (GoPlus is_proxy = 1). Proxy admin was not independently resolved; upgradeability is a standing risk on that chain.
- Independent liquidity and sellability were confirmed: GoPlus and honeypot.is both show 0 tax and no honeypot, and honeypot.is simulated real sells for 1,805 holders with zero failures, so the token is freely tradeable in practice.
- GitHub review of the official `AethirCloud` organization: 6 public repos. `checker-client` and `HostAgent` contain only versioned installers and compiled binaries (for example `HostAgent` folders V1.7.6.4 to V1.7.6.6; `checker-client` folders v1.0.2.5 to v1.0.3.1) with no buildable source. `client-sdk-js` and `metamask_demo` are thin JS/TS samples, `axelar-configs` is a fork, and `AethirCloud` is a profile config repo. The core GPU orchestration, Checker validation, Container runtime, and Indexer matching are closed source.
- Docs read on chain of reasoning: the Checker page states Checkers are "Initially deployed by the Aethir, with plans for gradual decentralization," confirming the validation layer is presently operator run, not permissionless.

### Claim vs reality
- "Fixed 42B ATH supply, no uncapped inflation": CONFIRMED IN CODE. Hard cap at 42B, mint already exhausted, no burn to reopen headroom. Supply is permanently fixed.
- "Clean token with no tax, no pause, no blacklist, no honeypot": CONFIRMED IN CODE and on chain. Source shows none of these; GoPlus and honeypot.is agree; real sells succeed.
- "Decentralized GPU cloud / DePIN": OVERSTATED. The token and reward accounting touch chain, but the compute orchestration, Checker validation, Container runtime, and Indexer are off chain closed source binaries, and the docs admit Checkers were initially deployed by Aethir with decentralization only planned. Decentralization of the compute layer cannot be verified in code and is presently aspirational.
- "Checker nodes secure and validate the network": OVERSTATED as a trustless claim. `checker-client` is a downloadable closed source binary and validation logic is not open source or on chain, so it cannot be independently verified.
- "Aethir Edge hardware": informational and out of code scope. It is a physical device described in docs; nothing in the public repos verifies its behavior. Not labeled FALSE, just unverifiable from code.
- "ATH staking, rewards, and burn": OVERSTATED at the token contract level. The ERC20 itself contains no staking, no reward emission, and no burn logic; rewards come from pre minted allocations and any staking or burn happens in separate, non public contracts or off chain. The token cannot inflate, but the described mechanics are not in the audited contract.
- "Open and transparent": OVERSTATED. The token is verified and open source, but the product core that the value proposition depends on is closed source, so overall transparency is partial.

### Severity
- Critical: 0.
- High: 0. No single EOA mint (owner is a Safe multisig), no active honeypot, pause, or blacklist.
- Medium (3): (1) Core product stack (Checker, HostAgent, orchestration, Indexer) is closed source, so DePIN and decentralization claims are unverifiable and transparency is capped. (2) The owner controllable whitelist is a treasury distribution control, not a restriction on ordinary holders, so it is a minor centralization note rather than a transfer risk. (3) Arbitrum ATH is an upgradeable proxy with an unverified admin.
- Low (3): (1) `mint(onlyOwner)` function still present though capped and exhausted. (2) Ownership not renounced; the multisig retains the capped mint and treasury distribution powers. (3) LP lock status of the deepest pools not independently verified.
- Informational (2): Aethir Edge hardware is not verifiable from public code. ATH exists across Ethereum, Arbitrum, and Solana with bridged representations, adding cross chain surface.
