# Security Audit Report: Heurist (HEU) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 3, 2026 |
| **Project** | Heurist |
| **Token Symbol** | HEU |
| **Contract / Program** | `0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea` |
| **Chain** | Ethereum |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **74/100** |
| **Overall Risk** | **LOW** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

HEU is the token of Heurist, an AI and GPU DePIN that markets decentralized inference, agent tooling, and image generation. The canonical token is a verified OpenZeppelin ERC20 with Ownable on Ethereum at 0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea, while the active trading market and protocol contracts live on Base through an official bridged mirror at 0xEF22cb48B8483dF6152e1423b19dF5553BbD818b. Live reads confirm name Heurist, symbol HEU, 18 decimals, and a total supply of exactly 1,000,000,000 HEU that already equals the MAXIMUM_SUPPLY constant, so no new tokens can be minted. The contract has no proxy, no pause, no blacklist, and no fee on transfer, which makes behavior simple and predictable. The only residual concern is that ownership rests with a single external account rather than a multisig, though its mint power is already exhausted by the cap.

HEU is the token of Heurist, an AI and GPU DePIN that sells decentralized inference, agent tooling, and image generation. The canonical token is a verified OpenZeppelin ERC20 with Ownable on Ethereum at 0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea, and the tradeable market and protocol live on Base through an official bridged mirror at 0xEF22cb48B8483dF6152e1423b19dF5553BbD818b. Total supply is hard capped at 1,000,000,000 HEU and the live total supply already equals that cap, so no new tokens can be minted. The contract has no proxy, no pause, no blacklist, and no fee on transfer, which makes the token behavior simple and predictable. The only residual concern is that ownership sits with a single external account rather than a multisig, though its mint power is already exhausted by the cap.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 2 |
| Informational | 6 |
| **Total** | **8** |

### Overall Risk Assessment: LOW

MEFAI onchain analysis places Heurist at 74 out of 100 (LOW risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Heurist / HEU |
| **Contract or program** | `0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea` |
| **Chain** | Ethereum |
| **Tags** | ERC 20, AI Inference DePIN, Ethereum, Passed |

Live reads on Ethereum confirm name Heurist, symbol HEU, 18 decimals, and a total supply of exactly 1,000,000,000 HEU, which equals the MAXIMUM_SUPPLY constant hard coded in the verified source. The mint function is restricted to the owner and reverts once the cap is reached, so the supply is effectively fixed. The contract is a plain OpenZeppelin ERC20 with no proxy pattern, no pause switch, no blacklist, and no transfer fee, and the standard proxy implementation and admin storage slots are empty. Ownership is held by the external account 0xfb93bee230a72a241534f70d85b76e07f35cd33f, which carries no code and is therefore a single key rather than a multisig. The Base token is a standard bridged representation whose l1Token points back to the Ethereum HEU and whose bridge is the canonical Base Standard Bridge, minting only against collateral locked on Ethereum.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- Total supply on Ethereum is exactly 1,000,000,000 HEU (0x0000...033b2e3c9fd0803ce8000000 = 1e27 at 18 decimals), equal to the MAXIMUM_SUPPLY constant 1_000_000_000e18; supply is hard capped and CoinGecko circulating is about 199.5M. Base bridged supply reads about 193.1M.
- Ownable; owner is the account 0xfb93bee230a72a241534f70d85b76e07f35cd33f which returns no bytecode, so it is a single key rather than a multisig. No AccessControl roles. The Base mirror has no owner (owner() reverts).
- mint(address,uint256) is onlyOwner but reverts when totalSupply() + amount > MAXIMUM_SUPPLY; supply already equals the cap so no further tokens can be minted. Staking and mining rewards are paid from the pre minted 1B allocation, not from new inflation. Mining has ended.
- Not a proxy (Blockscout proxy_type null, standard proxy storage slots empty); no pause; no blacklist; no fee on transfer (plain OZ ERC20); decimals 18 on all chains. The Base token is an OptimismMintableERC20 whose l1Token() returns the Ethereum HEU and whose bridge() is the canonical Base Standard Bridge 0x4200000000000000000000000000000000000010, minting only against collateral locked on Ethereum.

---

## 3. Claim versus Reality

- "Decentralized AI inference, GPU mining DePIN, Imagine image generation, LLM gateway" / Reality: these are off chain compute and software services; the token contract is a plain ERC20 and performs none of them, which is expected for a DePIN payment and staking token and is not misrepresented on chain.
- "Fixed supply, 1B maximum" / Reality: confirmed on chain, the MAXIMUM_SUPPLY constant equals 1B and the live total supply already equals it, and mint is capped.
- "Multichain HEU" / Reality: confirmed; the origin is Ethereum and bridged mirrors run on Base (the main trading and protocol home) and ZKsync Era via official bridges.
- "Staking rewards up to 50% APR from emissions" / Reality: emissions are distributed from the already minted 500M mining and staking bucket, not from inflation beyond the 1B cap.

The Heurist website markets a full stack AI infrastructure for the onchain economy, including decentralized inference, an agent marketplace called Heurist Mesh, an onchain trading copilot, and historically GPU mining, an image product named Imagine, and an LLM gateway. These are claims about off chain compute and software, and they cannot be proven or disproven from the token contract alone. The documentation describes a 1B token allocation split across mining and staking, treasury, team, and investors, with mining now ended and staking rewards paid from the pre minted allocation. None of these marketing claims contradict what the token contract actually does. The token functions as a payment and staking instrument for the ecosystem, which matches the stated purpose.

---

## 4. Website and Frontend Integrity

VERDICT: CLEAN | CONFIDENCE: high
The only three Ethereum style addresses present in both the homepage HTML and every JavaScript bundle sit inside outbound trade and analytics links, and none of them impersonate the canonical asset. The Base value 0xef22cb48b8483df6152e1423b19df5553bbd818b matches the official Base HEU mirror exactly on an app.uniswap.org chain equals base swap link, 0xAbEc5eCBe08b6c02F5c9A2fF82696e1E7dB6f9bf is the expected ZKsync mirror on a PancakeSwap link, and 0xb655dc66ecead581d1f1a5759c2c37c2dbef2275 is a Base liquidity pair on DexScreener, so there is no lookalike and no drainer contract. The reviewed pages expose zero wallet or web3 surface whatsoever (no WalletConnect, wagmi, window.ethereum, eth_sendTransaction, personal_sign, approve, permit, transferFrom, or setApprovalForAll), meaning the marketing site never requests a signature; the imagine, LLM gateway, and GPU mining apps live on separate first party subdomains such as mesh, ask, and tokenize dot heurist dot ai. All fifteen plus scripts are locally hosted Next.js chunks with no remote or obfuscated payloads (the scattered atob and fromCharCode tokens are ordinary bundled library code and the single new function occurrence is benign browser detection, not dynamic evaluation), and every external host referenced is legitimate (Spline, Vercel, Google Analytics, the GEODNET DePIN partner, GSAP, Dune, socials, and reputable exchanges). The homepage claims no security audit badges and shows no live metric counters to fabricate, so nothing is unbacked.


---

## 5. Findings by Severity

- HIGH: none. MEDIUM: none. LOW: owner is a single external account that has not renounced ownership, though mint is hard capped and supply is already at the cap so no inflation is possible; the tradeable supply also lives on Base and ZKsync as bridged mirrors, adding bridge trust assumptions even though they use the official Base Standard Bridge. INFO: verified source, immutable logic with no proxy, no pause, no blacklist, no fee on transfer, hard capped 1B supply with mint reverting once the cap is reached, standard OpenZeppelin ERC20.

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 10 |
| Supply and minting | 14 |
| Liquidity and market | 11 |
| Code safety | 13 |
| Transfer neutrality | 15 |
| Transparency | 11 |
| **Total** | **74/100** |

---

## 7. Conclusion

HEU is the token of Heurist, an AI and GPU DePIN that markets decentralized inference, agent tooling, and image generation. The canonical token is a verified OpenZeppelin ERC20 with Ownable on Ethereum at 0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea, while the active trading market and protocol contracts live on Base through an official bridged mirror at 0xEF22cb48B8483dF6152e1423b19dF5553BbD818b. Live reads confirm name Heurist, symbol HEU, 18 decimals, and a total supply of exactly 1,000,000,000 HEU that already equals the MAXIMUM_SUPPLY constant, so no new tokens can be minted. The contract has no proxy, no pause, no blacklist, and no fee on transfer, which makes behavior simple and predictable. The only residual concern is that ownership rests with a single external account rather than a multisig, though its mint power is already exhausted by the cap. On the MEFAI scale this token scores 74 out of 100 and is classified Passed.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Ethereum, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `https://www.coingecko.com/en/coins/heurist`
  - `https://docs.heurist.ai/protocol-overview/contract-addresses`
  - `https://docs.heurist.ai/protocol-overview/tokenomics`
  - `https://etherscan.io/token/0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea`
  - `https://basescan.org/token/0xef22cb48b8483df6152e1423b19df5553bbd818b`
  - `https://eth.blockscout.com/address/0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea`
  - `https://www.heurist.ai/`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
