# Security Audit Report: Heurist (HEU) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Heurist |
| **Token Symbol** | HEU |
| **Contract (Ethereum)** | `0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea` |
| **Chain** | Ethereum ERC 20 (canonical origin; primary trading on a Base bridged mirror `0xEF22cb48B8483dF6152e1423b19dF5553BbD818b`) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **70/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Passed** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Heurist markets itself as full stack AI infrastructure for the onchain economy, spanning decentralized inference, a GPU mining DePIN, image generation, an LLM gateway, and an emerging agent economy. Unlike many projects that sell a story ahead of a product, the audit finds that Heurist's core products are genuinely live and its token contract is clean and conservative. The reservations are about traction and token economics rather than about broken software or a dangerous contract.

1. **The core products are real and usable.** The Imagine image and video generator, the OpenAI SDK compatible LLM gateway, and the open source GPU miner software are all live and actively maintained, not slideware. The marketing describes products that a visitor can actually reach and use.
2. **The token is a clean, hard capped ERC 20 with its mint already exhausted.** MEFAI's onchain read confirms a verified OpenZeppelin ERC 20 with a MAXIMUM_SUPPLY constant of 1,000,000,000 HEU, a live total supply already equal to that cap, no proxy, no pause, no blacklist, and no transfer fee. No new tokens can be minted.
3. **Headline traction numbers are not independently verifiable.** Claims of over 13,000 GPU miners, over 1 billion inference requests, and over 30 hosted models recur across the marketing and blog but have no live public stats endpoint to confirm them, so they read as marketing snapshots rather than auditable metrics.
4. **Token utility is designed but economically thin.** Staking and payment mechanics are genuinely wired in, yet the market cap sits in the low hundreds of thousands of dollars with daily volume in the low thousands, so real demand is modest. Ownership also rests with a single external account rather than a multisig, though its mint power is already spent.

The contract is not a scam and the products are not vaporware. As a project, Heurist ships more than it claims to on the product side, while its traction narrative runs ahead of what is publicly provable and its token has a small real footprint. This lands Heurist at 70 out of 100, Passed, at MEDIUM overall risk.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Heurist / HEU |
| **Contract (Ethereum)** | `0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea` |
| **Decimals** | 18 |
| **Max supply** | 1,000,000,000 HEU (hard cap, equal to the onchain MAXIMUM_SUPPLY constant and to the live total supply) |
| **Circulating** | About 199.5 million HEU per CoinGecko; the Base bridged mirror reads about 193.1 million |
| **Structure** | Canonical origin on Ethereum; official bridged mirrors on Base (the main trading and protocol home) and ZKsync Era |
| **Contract controls** | Ownable ERC 20; a single external account holds the owner role; mint is capped and already exhausted; non upgradeable |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the HEU contract on Ethereum returned:

| Check | Result |
|-------|--------|
| Token identity | Heurist, HEU, 18 decimals, verified source |
| Max supply | 1,000,000,000 HEU, equal to the MAXIMUM_SUPPLY constant |
| Live total supply | Exactly 1,000,000,000 HEU, already at the cap |
| Mint authority | `mint(address,uint256)` is onlyOwner but reverts once totalSupply plus amount exceeds MAXIMUM_SUPPLY; supply is at the cap, so no further mint is possible |
| Owner | A single external account `0xfb93bee230a72a241534f70d85b76e07f35cd33f` with no bytecode (a single key, not a multisig); not renounced |
| Upgradeable | No, the contract is not a proxy; standard proxy and admin storage slots are empty |
| Pause / blacklist / fee | None; plain OpenZeppelin ERC 20 |

**Interpretation.** At the contract level HEU is strong. It is a verified, hard capped, non upgradeable ERC 20 with no pause, no blacklist, and no transfer fee, and its mint function can no longer produce tokens because the live supply already equals the hard cap. Staking and mining rewards are paid from the pre minted allocation, not from new inflation, and mining has ended. The one residual contract concern is that ownership sits with a single external account that has not renounced, rather than a multisig. Because the mint is exhausted and there is no pause, blacklist, or upgrade path, the practical power of that key is limited, so it is a caution rather than a red flag. The Base mirror is a standard OptimismMintableERC20 whose `l1Token()` points back to the Ethereum HEU and whose `bridge()` is the canonical Base Standard Bridge `0x4200000000000000000000000000000000000010`, minting only against collateral locked on Ethereum. The substantive questions for this project sit at the traction and token economics level below, not in the token contract.

---

## 3. Claim vs Reality: "Full stack AI infrastructure for onchain economy"

> Site: Heurist presents itself as full stack AI infrastructure for the onchain economy, decentralized, composable, compliant, and permissionless, with decentralized computing, image and video generation, an LLM gateway, GPU mining, and an agent marketplace.

**Reality: the core products are genuinely live.** This is the strongest part of Heurist. MEFAI confirmed that Imagine at imagine.heurist.ai is a working image and video generator exposing FLUX, Stable Diffusion, Veo 3, and Hunyuan models with a connect wallet flow and pay as you go credits. The LLM gateway is operational, is OpenAI SDK compatible, and is documented with an API token and a model list. The GPU miner is a real, actively maintained open source repository with Docker support and more than 170 commits, allowing contributors to add NVIDIA GPUs. Newer surfaces such as Heurist Mesh, the agent framework, Ask Heurist, and the x402 facilitator are documented and reflect a shift toward an agent economy. A visitor following the marketing reaches products that actually work, which is the opposite of the aspirational branding many projects rely on.

---

## 4. Claim vs Reality: "Fixed supply, 1 billion maximum"

> Site: The tokenomics describe a fixed 1 billion HEU supply split across mining and staking, treasury, team, and investors, with mining now ended and staking rewards paid from the pre minted allocation.

**Reality: confirmed onchain.** MEFAI's read shows the MAXIMUM_SUPPLY constant equals 1,000,000,000 HEU and the live total supply already equals it, so the mint function reverts and no new tokens can be created. Rewards are distributed from the already minted allocation rather than from inflation beyond the cap. This is a case where the marketing claim and the onchain reality match exactly, and it is a meaningful positive for a DePIN token whose value depends on a credible supply schedule.

---

## 5. Claim vs Reality: Traction numbers

> Site: Heurist and its blog repeat headline traction of over 13,000 GPU miners, over 1 billion AI inference requests, over 30 hosted models, and hundreds of developers.

**Reality: consistent in the marketing, but not independently verifiable.** These figures appear across the token announcement, Medium posts, and third party writeups, but MEFAI found no live public dashboard or stats endpoint that independently confirms the current miner count or request volume, so they read as marketing snapshots rather than auditable metrics. The homepage itself shows no live counters to confirm or fabricate. Coin listing descriptions also state that compute is aggregated from trusted DePIN partners, which suggests reliance on partner data centers alongside individual miners and slightly tempers the pure permissionless miner framing. The working products make the claims plausible, but the specific large numbers remain unproven, and a security minded reader should treat them as directional rather than settled.

---

## 6. Claim vs Reality: HEU utility and token economics

> Site: HEU is described as the asset that powers staking, mining rewards up to around 50 percent APR, and payment for API and compute access, with staking discounts.

**Reality: real mechanics, thin economics.** HEU has designed utility rather than pure decoration. Miner nodes must stake a minimum of 10,000 HEU or esHEU to earn, staking pays a base rate around 50 percent APR plus a share of protocol revenue and sequencer fees, and HEU or credits pay for API and compute access. These mechanics are live through the stake page and the credits system, and the emissions are paid from the already minted mining and staking bucket rather than from new inflation. The caveat is the size of the economy around them. The market cap sits roughly in the low hundreds of thousands of dollars, daily volume is in the low thousands, and the price has fallen about 55 percent over the reviewed week. The utility is real, but usage driven demand is modest and trading is thin and largely speculative, so the token's economic footprint is much smaller than the breadth of the product suite implies.

---

## 7. Claim vs Reality: "Decentralized" and multichain HEU

> Site: Heurist markets decentralized computing and a multichain HEU available across networks.

**Reality: broadly accurate, with normal bridge and aggregation caveats.** The canonical token lives on Ethereum, and the tradeable market and protocol home is Base, reached through the official Base Standard Bridge, with a further mirror on ZKsync Era. MEFAI verified that the Base mirror is a standard bridged representation backed by collateral locked on Ethereum, which is the expected and legitimate design, and MEFAI's frontend review found no lookalike or drainer addresses on the site. The two honest qualifications are that most tradeable supply lives on bridged mirrors, which adds the usual bridge trust assumptions even with the official bridge, and that compute is aggregated from trusted DePIN partners alongside independent miners, so the network is decentralized in design but partly partner served in practice.

---

## 8. Claim vs Reality: "Compliant" positioning versus "fully uncensored" Imagine

> Site: The homepage headline positions Heurist as decentralized, composable, compliant, and permissionless, while the Imagine product markets itself as fully uncensored with no limits.

**Reality: an internal tension worth noting.** A sitewide claim of compliance sits awkwardly next to a flagship image product advertised as fully uncensored. This is not a contract risk and not evidence of misconduct, but it is a transparency and positioning inconsistency that a reviewer should flag, because compliant and fully uncensored are not usually simultaneously true of the same content pipeline. The honest read is that Heurist is permissionless first, and the compliance language is aspirational framing rather than a demonstrated control.

---

## 9. Positive Findings (Credited)

- The core products are genuinely live and usable: Imagine image and video generation, an OpenAI SDK compatible LLM gateway, and an actively maintained open source GPU miner.
- The token contract is a verified, hard capped, non upgradeable OpenZeppelin ERC 20 with no pause, no blacklist, and no transfer fee.
- The mint is already exhausted because the live total supply equals the 1,000,000,000 HEU cap, so no inflation is possible and rewards come from the pre minted allocation.
- HEU has real, wired in utility (staking to mine, protocol revenue share, and payment for compute), not decorative tokenomics.
- The Base mirror uses the canonical Base Standard Bridge and is collateral backed, and MEFAI's frontend review found the marketing site clean, with no wallet or signature surface and no lookalike addresses.

---

## 10. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| HEU 001 | **MEDIUM** | Headline traction numbers (over 13,000 miners, over 1 billion inference requests, over 30 models) are repeated across marketing but have no live public stats endpoint to confirm them. |
| HEU 002 | **MEDIUM** | Token economics are thin: market cap in the low hundreds of thousands of dollars, daily volume in the low thousands, and about a 55 percent weekly price decline, despite genuine utility design. |
| HEU 003 | **LOW** | The Ownable ERC 20 is owned by a single external account that has not renounced, though mint is hard capped and supply is already at the cap, so no inflation is possible. |
| HEU 004 | **LOW** | Most tradeable supply lives on Base and ZKsync as bridged mirrors, adding bridge trust assumptions even though the official Base Standard Bridge is used. |
| HEU 005 | **LOW** | Compute is aggregated from trusted DePIN partners alongside individual miners, which softens the pure permissionless decentralized framing. |
| HEU 006 | **LOW** | The sitewide compliant positioning is in tension with Imagine marketing itself as fully uncensored with no limits. |
| HEU 007 | **INFO** | Verified OpenZeppelin ERC 20, immutable with no proxy, no pause, no blacklist, no fee on transfer, and a hard capped 1 billion supply with mint reverting at the cap (positive). |

---

## 11. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, hard capped, no transfer fee, standard OpenZeppelin ERC 20 |
| Supply / minting | Low risk | Live supply already equals the cap; mint reverts; no inflation possible |
| Product reality | Low risk | Imagine, LLM gateway, and GPU miner are genuinely live and usable |
| Traction | Medium risk | Headline metrics unverifiable via any public dashboard |
| Token economics | Medium risk | Very small market cap and thin volume despite real utility design |
| Ownership / bridges | Medium risk | Single external owner (mint exhausted) and tradeable supply on bridged mirrors |
| Transparency | Medium risk | Unverifiable numbers, partner aggregated compute, uncensored versus compliant tension |

---

## 12. Technical Specifications

| Item | Value |
|------|-------|
| Contract (Ethereum) | `0xec463D00aa4dA76fb112cD2e4AC1C6BeF02da6ea` |
| Base mirror | `0xEF22cb48B8483dF6152e1423b19dF5553BbD818b` (OptimismMintableERC20, collateral backed) |
| Decimals | 18 |
| Max supply | 1,000,000,000 HEU (hard cap, equal to the live total supply) |
| Upgradeable | No (not a proxy) |
| Mint | onlyOwner, reverts once supply reaches MAXIMUM_SUPPLY; already exhausted |
| Owner | Single external account `0xfb93bee230a72a241534f70d85b76e07f35cd33f`, not renounced |
| Pause / blacklist / fee | None |
| Bridges | Official Base Standard Bridge `0x4200000000000000000000000000000000000010`; ZKsync Era mirror |

---

## 13. Conclusion

Heurist is a project whose products are more real than its numbers are provable. The token contract is clean and conservative, a verified, hard capped, non upgradeable ERC 20 whose mint is already exhausted, which keeps contract level risk low. On the product side, Imagine, the LLM gateway, and the GPU miner are genuinely live and usable, so the full stack AI narrative is largely backed by shipped software rather than promises. The reservations that hold Heurist to 70 out of 100 are the traction story and the token economy: the headline figures of thousands of miners and billions of requests cannot be independently confirmed, compute is partly aggregated from trusted partners, the token has a small real footprint with thin volume and a steep recent decline, and ownership still rests with a single external key. None of these is a contract exploit or a broken product. On balance, Heurist earns a Passed verdict at MEDIUM overall risk, with the guidance that its capabilities are credible while its scale should be treated as unverified until backed by public data.

---

## 14. Recommendations

**For the Heurist team:**
- Publish a live public stats endpoint or dashboard for miner count, inference volume, and hosted models, so the headline traction numbers become auditable rather than marketing snapshots.
- Move the owner key to a multisig or renounce ownership, since the mint is already exhausted and the residual power of the key is small; formalizing this would close the last contract level caution.
- Reconcile the compliant positioning with the fully uncensored Imagine framing, and be explicit about which compute is permissionless miner served versus partner aggregated.
- Continue surfacing real onchain protocol revenue and staking metrics to connect the token's designed utility to demonstrable demand.

**For users:**
- Treat the products as real and usable, but treat the large traction figures as unverified until Heurist publishes live data.
- Understand that HEU has genuine staking and payment utility but a very small and thin market, so trading is largely speculative today.
- Note that most tradeable HEU lives on Base and ZKsync as bridged mirrors, and that ownership of the Ethereum contract still sits with a single key, even though its mint power is spent.

---

## 15. Verification

- MEFAI onchain analysis: a direct Ethereum read of the HEU contract (identity, 18 decimals, total supply of exactly 1,000,000,000 HEU equal to the MAXIMUM_SUPPLY constant, mint restricted to owner and reverting at the cap, no proxy with empty implementation and admin slots, no pause, no blacklist, no transfer fee, and a single external owner account that has not renounced), plus verification that the Base mirror is a collateral backed OptimismMintableERC20 whose l1Token points to the Ethereum HEU and whose bridge is the canonical Base Standard Bridge.
- Product checks: live confirmation of Imagine at imagine.heurist.ai (FLUX, Stable Diffusion, Veo 3, Hunyuan, connect wallet, pay as you go credits), the OpenAI SDK compatible LLM gateway, and the actively maintained open source GPU miner repository with more than 170 commits.
- Frontend review: MEFAI's inspection of the marketing site found no wallet or web3 signature surface, no lookalike or drainer addresses, and only legitimate outbound trading and analytics links.
- Market and token economics: CoinGecko and CoinMarketCap readings of circulating supply, market cap in the low hundreds of thousands of dollars, daily volume in the low thousands, and about a 55 percent weekly price decline.
- Project statements: the project's website, documentation, and blog (full stack AI infrastructure positioning, 1 billion fixed supply tokenomics, the over 13,000 miners and over 1 billion inference requests traction claims, staking and payment utility, and the multichain and compliant framing).
