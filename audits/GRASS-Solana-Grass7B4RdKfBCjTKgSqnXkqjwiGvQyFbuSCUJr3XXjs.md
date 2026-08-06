# Grass: Whitepaper Claims vs Code Reality
**Score: 50/100, HIGH RISK (Flagged)**
Date: 2026-08-06

Token (live, independently found and confirmed onchain):
- SPL mint: `Grass7B4RdKfBCjTKgSqnXkqjwiGvQyFbuSCUJr3XXjs` (confirmed on three independent sources: Solana mainnet RPC `getAccountInfo`, CoinGecko, and the DexScreener token API)
- Owner program: `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (classic SPL Token program, not `Token-2022`)
- Decimals: 9
- Freeze authority: `null` (renounced, so balances cannot be frozen)
- Mint authority: `31rYartQwHeBMjAe2MgGpffGV57fQY3kug4BDN8tLGqQ` (LIVE, not renounced)
- On chain supply: 999,993,128.405 GRASS (raw `999993128405014338`), effectively the full 1B cap already minted
- Live market (CoinGecko and DexScreener): price about $0.31, market cap about $204M, FDV about $311M, rank about #162, 24h volume about $15M, circulating about 654.55M of 1B
- Deepest on chain pool: Raydium CLMM GRASS/SOL (`GTJ2S27UL7yZ3TdTwpKjfNcxeEZRkRPHjpj5Fubwb8Mk`) at about $244K liquidity. Orca GRASS/USDC about $145K, Orca GRASS/SOL about $63K. Most real depth and volume live on centralized venues (Bybit GRASS/USDT, LBank), not on chain.

Websites:
- https://www.getgrass.io (redirects to https://www.grass.io)
- Docs and whitepaper: https://grass-foundation.gitbook.io/grass-docs (formerly `wynd-network.gitbook.io/grass-docs`)
- GitHub orgs checked: https://github.com/getgrass and https://github.com/Wynd-Network

### Verified in code and on chain (MEFAI deep source review)
- SPL mint state read directly from Solana mainnet RPC. Freeze authority is `null`, which is a genuine positive: no party can freeze user token accounts. Decimals 9, standard SPL Token program, no `Token-2022` transfer fee or transfer hook extensions, so transfers are permissionless with no on chain tax or owner settable fee.
- Mint authority is present and set to `31rYartQwHeBMjAe2MgGpffGV57fQY3kug4BDN8tLGqQ`. I read that authority account on chain: it is owned by the System Program (`11111111111111111111111111111111`), has zero data, is not executable, and holds about 0.74 SOL. It is therefore a discretionary externally controlled authority (a wallet or a multisig vault), not a program, timelock, or on chain minting rule. Minting is thus governed by whoever holds that key or keys, not by code.
- The whitepaper states supply is "fixed at 1,000,000,000 tokens." On chain that cap is NOT enforced, because the mint authority is still live. The fixed supply is a policy promise, not an immutable on chain guarantee. Practically about 99.999 percent is already minted, so near term dilution is small, yet the authority to mint more remains.
- Source code: the `Wynd-Network` org and the `getgrass` account both show zero public repositories. The website links no repositories, and the docs make no open source statement. A GitHub repository search for Grass and Wynd node code returned no official results. The node, router, validator, ZK processor, and the Sovereign Data Rollup are closed source. None of the ZK proof, data provenance, or rollup claims can be verified in code.
- Decentralization is self described as not yet achieved. The docs say "Initially, the validator operates as a singular, centralized entity managing all transactions," with a committee of validators planned for the future. So the network runs on a single centralized validator today.
- Tokenomics from docs: 1B fixed target. Community 30 percent (Airdrop One 100M, Future Incentives 170M, Router Rewards 30M), Early Investors 25.2 percent (252M), Contributors 22 percent (220M), Foundation and Ecosystem 22.8 percent (228M). Vesting: early investors "1-year cliff and a 1-year vesting period," contributors "1-year cliff and 3-year vesting." Rewards are distributions from these pre allocated pools rather than open ended inflation.
- Holder concentration could not be retrieved. `getTokenLargestAccounts` was rate limited or blocked on every free RPC endpoint I tried. Not fabricating a distribution.

### Claim vs reality
- "Residential bandwidth network of millions sharing unused internet" (getgrass.io). Plausible and consistent with a large real product, real CEX listings, and a top ~160 market cap, but the client and network code are closed source, so the mechanics cannot be verified in code. Status: OVERSTATED transparency, product likely real.
- "Sovereign Data Rollup" with validators that "generate ZK proofs to checkpoint session data on-chain" and a Grass Data Ledger that gives "data provenance" (architecture docs). No source code, no public program address for the ZK processor or rollup is disclosed, and the docs concede a single centralized validator. The claim is unverifiable in code and partly contradicted by the admitted centralization. Status: OVERSTATED.
- "Decentralization" and "own a part of the Grass network" (getgrass.io). The docs admit the validator is a "singular, centralized entity" with decentralization only planned. Status: OVERSTATED.
- "The total supply of GRASS tokens will remain fixed at 1,000,000,000" (tokenomics docs). On chain the mint authority is live, so the cap is not enforced by code. Status: OVERSTATED (fixed by policy, not on chain).
- Token safety of GRASS itself (no tax, cannot freeze you). CONFIRMED IN CODE: classic SPL Token, freeze authority `null`, no transfer fee extension, no owner settable tax. This part matches a clean, permissionless transfer token.
- Vesting for investors and contributors. Stated in docs only; cannot be independently confirmed on chain without the specific vesting or escrow accounts, which are not disclosed. Status: unverified claim.

### Severity
- HIGH: Live mint authority `31rYartQwHeBMjAe2MgGpffGV57fQY3kug4BDN8tLGqQ` is a discretionary System owned account with unrestricted mint power, contradicting the "fixed 1,000,000,000" supply claim. Per MEFAI rules a live discretionary mint authority caps the supply dimension and flags it regardless of product quality.
- HIGH: Entire protocol stack (node, router, validator, ZK processor, Sovereign Data Rollup) is closed source. Neither the Wynd-Network org nor the getgrass account has public repositories. The headline ZK proof, provenance, and decentralization claims are unverifiable in code.
- MEDIUM: Decentralization is overstated. Docs concede a single centralized validator today, so trust currently rests on one operator.
- LOW: On chain DEX depth is thin (deepest pool about $244K), which is expected for a centralized exchange primary token where real trading depth is on centralized exchanges, so this is a liquidity note rather than a core risk, with no evidence of locked LP.
- LOW: Mint authority is a plain System owned account rather than a transparent program, timelock, or disclosed multisig, so there is no on chain constraint or public governance visible over minting.
- INFORMATIONAL: Freeze authority is renounced and the token uses the audited classic SPL Token program with no tax or transfer hook, which are genuine positives. Token first pooled on chain around late October 2024.
