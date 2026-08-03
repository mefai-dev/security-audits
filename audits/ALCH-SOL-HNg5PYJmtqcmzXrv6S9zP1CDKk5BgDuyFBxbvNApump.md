# Security Audit Report: Alchemist AI (ALCH) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 3, 2026 |
| **Project** | Alchemist AI |
| **Token Symbol** | ALCH |
| **Contract / Program** | `HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump` |
| **Chain** | Solana |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **66/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Alchemist AI is a genuine Solana SPL token whose live mint was confirmed at HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump via public RPC. The account is a classic SPL mint owned by the Token program with six decimals and a supply near one billion. Both mint authority and freeze authority read null, meaning issuance is renounced and no account can be frozen, and there is no transfer fee because there are no Token 2022 extensions. The address ends in the pump suffix, confirming a pump.fun origin, and the project markets a real documented no code app builder. The one material gap is the website supplied for review: alchemistai.org is a parked lander page, while the true official site is alchemistai.app.

The token is legitimate at the contract level and the core safety checks pass cleanly. Live RPC confirmed a fixed supply near one billion with six decimals, renounced mint authority, and renounced freeze authority, which removes the common rug and honeypot vectors. There is no transfer fee because the mint is a classic SPL account without Token 2022 extensions. Alchemist AI backs the token with an actual no code application builder rather than an empty narrative. The chief caution is the domain mismatch and the speculative memecoin style profile inherited from its pump.fun launch.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 1 |
| Low | 1 |
| Informational | 4 |
| **Total** | **6** |

### Overall Risk Assessment: MEDIUM

MEFAI onchain analysis places Alchemist AI at 66 out of 100 (MEDIUM risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Alchemist AI / ALCH |
| **Contract or program** | `HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump` |
| **Chain** | Solana |
| **Tags** | SPL, AI App Builder, Solana, Passed |

On chain reality is strong and matches a fixed supply utility token. The mint HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump is owned by the standard SPL Token program with an 82 byte account and no extensions, so there is no transfer fee or freeze extension hiding in the token. Mint authority and freeze authority both return null, meaning the team cannot inflate supply or freeze wallets. Total supply reads 999,953,125.101559 tokens at six decimals, and the pump suffix confirms the pump.fun bonding curve origin. Market cap sits in the low tens of millions of dollars after an 88 percent decline from the December 2025 peak, marking it a speculative but structurally clean asset.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- Supply 999,953,125.101559 ALCH (raw 999953125101559), decimals 6, circulating roughly 850M per CoinGecko.
- Mint authority is null, so minting is renounced and supply is fixed.
- Freeze authority is null, so no holder balance can be frozen.
- Classic SPL token owned by TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA, mint account space 82 with no extensions, therefore no transfer fee; pump.fun origin confirmed by the pump address suffix; market cap roughly 24M to 67M USD across sources.

---

## 3. Claim versus Reality

- "No code development platform that turns natural language prompts into apps" / Reality: the product is real and documented at docs.alchemistai.app, and the ALCH token is a utility credit costing about 200 ALCH per app generation; the token itself is a plain SPL token and does no building on chain.
- "Secure Sandbox environment and image generation and marketplace" / Reality: docs describe iframe based sandboxing and an Arcane Forge marketplace, consistent marketing, but these are off chain app features with no on chain enforcement.
- Website supplied as alchemistai.org / Reality: that domain is a parked page that redirects to a /lander parking screen; the official domain is alchemistai.app, matching the X handle alchemistAIapp and docs.alchemistai.app.

The website provided for review, alchemistai.org, is not the official Alchemist AI site. Fetching it returns a tiny stub that redirects on load to a /lander parking page, a classic hallmark of a parked or lookalike domain. The genuine official presence is alchemistai.app, corroborated by the project X account alchemistAIapp and the documentation host docs.alchemistai.app. The documentation clearly describes the no code builder, iframe sandboxing, image generation, and the Arcane Forge marketplace. The directory should record alchemistai.app and treat alchemistai.org as an unofficial or squatted domain.

---

## 4. Website and Frontend Integrity

VERDICT: MINOR ISSUES | CONFIDENCE: med
The official domain www.alchemistai.app is a SvelteKit single page app on Vercel whose shell loads only same origin /_app/immutable assets with no remote scripts, no eval or atob or base64 obfuscation, and no drainer style approve or transfer or signature calls, it uses ordinary Solana wallet public key login for a read only session, it embeds no token address at all, and its manifest plus the GitBook docs at docs.alchemistai.app confirm genuine Alchemist AI and ALCH branding while the official mint HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump is corroborated by Solscan and exchanges with no wrong or lookalike mint seen anywhere. By contrast alchemistai.org is a parked GoDaddy lander and not the real product, since it JavaScript redirects to a /lander page carrying lander_type parkweb and traffic_target gd cookies and pulls monetization scripts from img1.wsimg.com, and it shows no app, no wallet connect, and crucially no token, so it is a parked lookalike and a user confusion or future phishing surface rather than a live drainer today. Two caveats keep this from a clean rating: at audit time the official .app returned HTTP 500 on every route so the site was effectively down, and I could not decode the current wallet transaction bundle because live asset hashes had rotated to 404 and the archived July 2025 bundles were brotli encoded with no decoder available, leaving the live signing path uninspected. Nothing observed points to a wallet drainer, an unbacked audit badge, or a substituted token, and no audit badge was even present on any reachable surface, but users should reach the token only through the .app or Solscan and treat alchemistai.org as untrusted. Overall the official property looks clean while the parked .org and the current .app outage are the only real flags.


---

## 5. Findings by Severity

- HIGH: none. MEDIUM: the reviewed website alchemistai.org is a parked lookalike, not the official site; listing it risks sending users to a domain that could later be weaponized, so the directory must publish alchemistai.app. LOW: pump.fun origin speculative asset that fell roughly 88 percent from its December 2025 all time high near 0.24 USD, so high volatility. INFO: mint renounced, freeze renounced, no transfer fee, classic SPL with no hidden extensions, and a real documented product.

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 12 |
| Supply and minting | 14 |
| Liquidity and market | 9 |
| Code safety | 12 |
| Transfer neutrality | 15 |
| Transparency | 4 |
| **Total** | **66/100** |

---

## 7. Conclusion

Alchemist AI is a genuine Solana SPL token whose live mint was confirmed at HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump via public RPC. The account is a classic SPL mint owned by the Token program with six decimals and a supply near one billion. Both mint authority and freeze authority read null, meaning issuance is renounced and no account can be frozen, and there is no transfer fee because there are no Token 2022 extensions. The address ends in the pump suffix, confirming a pump.fun origin, and the project markets a real documented no code app builder. The one material gap is the website supplied for review: alchemistai.org is a parked lander page, while the true official site is alchemistai.app. On the MEFAI scale this token scores 66 out of 100 and is classified Passed.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Solana, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `https://api.mainnet-beta.solana.com (getAccountInfo and getTokenSupply for HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump)`
  - `https://www.coingecko.com/en/coins/alchemist-ai`
  - `https://docs.alchemistai.app/docs/get-started`
  - `https://www.alchemistai.app/`
  - `https://x.com/alchemistAIapp`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
