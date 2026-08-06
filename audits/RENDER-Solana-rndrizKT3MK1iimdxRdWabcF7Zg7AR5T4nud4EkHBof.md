# Render Network: Whitepaper Claims vs Code Reality
**Score: 47/100, HIGH (Flagged)**
Date: 2026-08-06

Token (live, independently found and confirmed onchain):
- Canonical current token is the Solana SPL mint `rndrizKT3MK1iimdxRdWabcF7Zg7AR5T4nud4EkHBof`, symbol RENDER, decimals 8, owned by the classic Token program `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (mint account space 82, so no token-2022 extensions such as transfer fee or permanent delegate).
- On chain supply 484,367,040.41839333 RENDER (`getTokenSupply` and mint account agree).
- Mint authority is LIVE, not null: `CFyeujXVymxgP2YR9kLbPsaCv2rKrtXMWtJ3EbAN2pdc`. That account is a PDA owned by upgradeable program `circiqFCstNzaFBji1udQ6txgQBrn29pVSYHNJQo3wZ` (the Burn and Mint Equilibrium emissions program).
- Freeze authority is LIVE, not null: `3LNxAhNnQpbCPcvgiamZhUbBugZTzxbjhcMwJ5jE65r5`. That account is a PDA owned by upgradeable program `distZXJ5FYrPhjBhB5P2BQ9B2AsPzJ4TcUSz6hKssP1`.
- Both of those programs share the SAME upgrade authority `7CVt936gVDXfKeXdRs5xcWVkrEaYGMTV3HA2K7j4Bqa7`, and that account is System Program owned (`11111111111111111111111111111111`), space 0, roughly 0.46 SOL balance. That is a plain single key wallet, not a Squads vault and not a program multisig. One key can upgrade both the minting program and the freeze program.
- Legacy Ethereum ERC20 RNDR `0x6De037ef9aD2725EB40118Bb1702EBb27e4Aeb24` still deployed: name Render Token, 0 percent buy, 0 percent sell, 0 percent transfer tax, not a honeypot (GoPlus `honeypot_with_same_creator` 0, `cannot_sell_all` 0), verified open source, 92,912 holders, listed on Binance and Coinbase. It is an upgradeable proxy (`is_proxy` 1) and still reports total supply about 533,532,274.
- Live market (CoinGecko and CoinPaprika): price about USD 1.33, market cap about USD 688.8M, FDV about USD 708.4M, rank about 84, 24h volume roughly USD 8.8M to 22.6M, circulating about 518.8M, total about 533.5M, reported max about 644.2M, ATH USD 13.53 (March 2024). Deepest on chain pool is Raydium RENDER/SOL at about USD 391K liquidity; on chain DEX liquidity is thin because volume is CEX dominated.

Websites:
- https://rendernetwork.com
- https://renderfoundation.com
- Docs: https://know.rendernetwork.com
- Code org: https://github.com/rendernetwork

### Verified in code and on chain (MEFAI deep source review)
- GitHub org `github.com/rendernetwork` holds only four public repositories: `RNPs` (governance proposals), `c4d-plugin` (a Cinema 4D client side plugin), `advent-tos`, and `.github`. There is NO public node client, NO job dispatch service, NO proof of render verifier, and NO source for the on chain BME minting program. The rendering orchestration core is closed source.
- The BME emissions logic lives in the on chain program `circiqFCstNzaFBji1udQ6txgQBrn29pVSYHNJQo3wZ` as bytecode only. Its source is not published in the org, and the program is upgradeable.
- Mint authority and freeze authority are both PDAs of upgradeable programs, and both programs answer to one System owned key `7CVt936gVDXfKeXdRs5xcWVkrEaYGMTV3HA2K7j4Bqa7`. This is the decisive control finding: a single key is one program upgrade away from minting unlimited RENDER or freezing any holder. There is no immutable on chain supply cap.
- The RENDER SPL token itself is a standard classic SPL mint with no token-2022 traps; buy and sell work and there is no transfer tax at the token level. The risk is authority centralization, not a transfer trap.
- Governance is real but off the critical path: RENDER SPL voting now runs on Nation.io with voting power equal to self custodial token balance, and legacy ERC20 voting used Snapshot. The Render Network Foundation runs proposal review and implementation, so binding execution and protocol control remain centralized.

### Claim vs reality
- Claim: decentralized GPU rendering marketplace. Verdict OVERSTATED. The product and marketplace are real and at scale, but the orchestration that would prove decentralization (node client, job dispatch, proof of render) is entirely closed source, so decentralization of the compute layer cannot be verified from code.
- Claim: proof of render. Verdict OVERSTATED. Marketed as a concept, but there is no public verifier code and no dedicated docs page; the `llms.txt` index has no proof of render or job dispatch page. Unverifiable from public source.
- Claim: Burn and Mint Equilibrium tokenomics with predictable declining emissions (year 1 about 9,126,804 and year 2 about 5,905,580 RENDER) and burn on job payment. Verdict CONFIRMED IN CODE with a caveat. A live minting program and a live mint authority exist on chain, consistent with BME, but the schedule is not enforced by an immutable contract; the emissions program is upgradeable by one key, so the cap is policy, not code.
- Claim: RNP governance is community driven and transparent. Verdict CONFIRMED for signaling, OVERSTATED for control. Proposals and token weighted voting are public and real, but the Foundation reviews and implements, and it holds the single upgrade key over mint and freeze, so voters do not hold the keys.
- Claim: RNDR upgraded to RENDER SPL. Verdict CONFIRMED. The Solana SPL mint is live and is the canonical token; the ERC20 remains deployed as an upgradeable proxy and still reports a large nominal supply, which is a dual supply and legacy proxy risk.

### Severity
- CRITICAL: one System owned single key `7CVt936gVDXfKeXdRs5xcWVkrEaYGMTV3HA2K7j4Bqa7` is the upgrade authority of BOTH the mint controlling program and the freeze controlling program. That key can upgrade to mint unlimited RENDER or freeze any account.
- HIGH: mint authority is live with no immutable on chain supply cap (inflation risk beyond the stated schedule).
- HIGH: freeze authority is live on the SPL mint, so holder accounts can be frozen.
- MEDIUM: rendering orchestration core and the BME minting program are closed source; only governance text and a client plugin are public.
- MEDIUM: legacy ERC20 is an upgradeable proxy still deployed with about 533M nominal supply, creating dual chain supply accounting and proxy upgrade risk.
- LOW: on chain DEX liquidity is thin (deepest pool about USD 391K) relative to a roughly USD 688M market cap; exit depth on chain is limited and CEX dependent.
- INFORMATIONAL: docs do not disclose to users that the SPL mint and freeze authorities are live and controlled by one key.
- INFORMATIONAL: RENDER is a genuine top 100 asset with zero transfer tax, no honeypot behavior, deep CEX liquidity, and years of operation; the flag is about key centralization, not a scam.
