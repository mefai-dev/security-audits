# Nosana: Whitepaper Claims vs Code Reality
**Score: 66/100, MEDIUM (Passed)**
Date: 2026-08-06

Token (live, independently found and confirmed onchain):
- Canonical SPL mint: `nosXBVoaCTtYdLvKY6Csb4AC8JCdQKKAaWYtx2ZMoo7` (symbol NOS, name Nosana). Same address is hardcoded as `NOS_TOKEN` mainnet in the program source `common/src/id.rs`, so code and chain agree.
- Token program owner: `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` (classic SPL Token, not Token-2022, so no transfer hooks or transfer fee extension).
- Decimals: 6 (matches `NOS_DECIMALS` in `common/src/constants.rs`).
- Mint authority: null (minting is permanently disabled onchain).
- Freeze authority: null (no account can be frozen).
- Supply: 99,999,720.497386 NOS (raw 99,999,720,497,386). Code sets `NOS_TOTAL_SUPPLY` at 100,000,000, so supply is effectively fixed at roughly 100M with a tiny historical reduction of about 279,503 NOS.
- Live market (CoinGecko and DexScreener, 2026-08-06): price about $0.235, market cap about $23.5M, FDV about $23.5M (fully circulating), market cap rank about 731, 24h volume about $0.49M. Deepest pool is Raydium CLMM NOS/USDC pair `3GkFzURGWNWyErnjQvnkZpgcLnNocjnRwvMYXiVDiVQk` at about $418K liquidity; a NOS/SOL Raydium CLMM pair adds about $138K.

Websites:
- https://nosana.io
- https://docs.nosana.com (redirects to learn.nosana.com)
- https://github.com/nosana-ci/nosana-programs (source repo)

### Verified in code and on chain (MEFAI deep source review)
Nosana ships a real, open source Anchor program suite in the `nosana-programs` repo (Nosana Program Library, MIT, marked "Most code is unaudited. Use at your own risk."). Five programs are declared in `common/src/id.rs` and all five are live executable programs on Solana mainnet:
- Nosana Jobs `nosJhNRqr2bc9g1nfGDcXXTXvYUmxD4cVwy2pMWhrYM`
- Nosana Staking `nosScmHY2uR24Zh751PmGj9ww9QRNHewh9H59AfrTJE`
- Nosana Rewards `nosRB8DUV67oLNrL45bo2pFLrmsWPiewe2Lk2DRNYCp`
- Nosana Pools `nosPdZrfDzND1LAR28FLMDEATUPK53K8xbRBXAirevD`
- Nosana Nodes `nosNeZR64wiEhQc5j251bsP4WqDabT6hmz4PHyoHLGD`

Jobs program (`programs/nosana-jobs/src/lib.rs`): real onchain marketplace. Projects post work with `list` or `assign` (IPFS job reference plus timeout); nodes join a queue with `work`, take work with `claim`, and settle with `finish`, which pays out escrowed NOS. Funds sit in a program owned vault, with `recover`, `extend`, `end`, and `quit` covering the job lifecycle. Job coordination, escrow, and NOS settlement are genuinely on chain. Admin variants (`close_admin`, `quit_admin`, `clean_admin`) exist for dispute resolution.

Staking program (`programs/nosana-staking/src/lib.rs`): real onchain staking. `stake` locks NOS into a StakeAccount plus VaultAccount for a chosen duration, `unstake` starts a cooldown, `withdraw` releases after cooldown, and `topup`, `extend`, `restake`, `close` manage the position. Two privileged instructions exist: `slash` can reduce a StakeAccount balance, and `update_settings` can change the slashing authority and token account.

Rewards program (`programs/nosana-rewards/src/lib.rs`): reflection style fee distribution. `add_fee` sends NOS into a reward vault, `enter` and `sync` track reflection points, and `claim` pays stakers. This is fee redistribution to stakers, not token inflation, which is consistent with mint authority being null.

Node client, CLI, and SDK (`nosana-cli`, `nosana-kit`, `nosana-sdk`, `nosana-python-sdk`) are public TypeScript repos. The staking and voting UIs (`stake.nosana.com`, `vote.nosana.com`) are public Vue repos. An `indexer` repo and `audits` folder with `NOSANA_STAKING_REPORT_1.pdf` and `NOSANA_STAKING_REPORT_2.pdf` (Op Codes, 2022, staking only) are published.

Onchain governance check (the main risk): the programData accounts for all five programs (jobs, staking, rewards, pools, nodes) all report the same upgrade authority `GXs53JMXbgdMDhtmjE9iNgSmC1gu8f3adZhXuCEq1Bx9`. That account is owned by the System Program with zero data bytes and is not executable, so onchain it presents as a single signer keypair, not a multisig program account. One key can therefore upgrade the code that governs staked and escrowed NOS across all five core programs.

### Claim vs reality
- "Decentralized GPU marketplace where you earn NOS by hosting GPUs" => CONFIRMED IN CODE for the coordination layer. Jobs, queueing, escrow, and payout are real onchain Anchor instructions, and the node client is open source. The actual GPU inference runs off chain on operator machines, which is normal DePIN architecture and not a false claim, but worth stating plainly: only settlement and coordination are on chain, not compute.
- "On chain jobs" => CONFIRMED IN CODE. The jobs program escrows NOS and pays nodes on `finish` with results referenced by IPFS.
- "NOS staking and rewards on Solana smart contracts" => CONFIRMED IN CODE. Staking and reflection reward programs are deployed and open source.
- "Fixed 100M NOS supply, no inflation" => CONFIRMED IN CODE and on chain. Mint authority is null and supply is about 99.9997M, matching `NOS_TOTAL_SUPPLY`.
- "NOS burn" => OVERSTATED. The reward design is reflection style fee redistribution to stakers, not a burn. Supply is only about 279K below 100M, so there is no meaningful active burn engine in the reviewed code. Supply is effectively capped, which is the material point.
- "Audited and secure" style framing => OVERSTATED. Only the staking program has a published audit, from Op Codes in 2022. Jobs, rewards, pools, and nodes are unaudited, which the repo itself admits with "Most code is unaudited." The programs are also upgradeable by a single key, so an old audit does not bind the current or future deployed bytecode.
- Transparency claims => CONFIRMED IN CODE and on chain, with one caveat. Program IDs, the mint, decimals, and supply are all independently verifiable and consistent between source and chain. The caveat is that the shared upgrade authority is an unlabeled single account rather than a documented onchain multisig.

### Severity
- HIGH: single System owned (EOA style) key `GXs53JMXbgdMDhtmjE9iNgSmC1gu8f3adZhXuCEq1Bx9` holds upgrade authority over all five programs (jobs, staking, rewards, pools, nodes). The bytecode that escrows job funds and locks staked NOS can be changed by one signer, and the authority is not renounced and not a multisig. This caps ownershipControl and codeSafety.
- MEDIUM: most of the program suite (jobs, rewards, pools, nodes) is unaudited; only staking carries a 2022 Op Codes audit that predates current code.
- MEDIUM: the staking program exposes a `slash` instruction plus `update_settings` that can reduce staked NOS and reassign the slashing authority, a privileged role over user funds.
- LOW: DEX liquidity is thin relative to market cap (deepest pool about $418K against roughly $23.5M market cap) and sits in Raydium CLMM pools with no evidence of an LP lock.
- LOW: the upgrade authority is an unlabeled single wallet rather than a published multisig, reducing governance transparency.
- INFORMATIONAL: actual GPU inference executes off chain on node clients; only job coordination, escrow, and settlement are on chain. Standard DePIN design, noted for clarity.
- INFORMATIONAL: mint authority and freeze authority are both null, so no inflation and no freeze risk; supply is fixed at about 100M.
