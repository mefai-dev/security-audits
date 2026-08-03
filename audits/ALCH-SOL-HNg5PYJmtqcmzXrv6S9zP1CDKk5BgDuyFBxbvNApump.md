# Security Audit Report: Alchemist AI (ALCH) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 3, 2026 |
| **Project** | Alchemist AI |
| **Website** | https://www.alchemistai.app |
| **Audit Type** | Whole Project (Claim versus Reality) |
| **Methodology** | Product and Traction Review + Website Frontend Review + Onchain Analysis |
| **Mefai Security Score** | **55/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Passed** |
| **Token / Contract** | ALCH `HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump` (Solana) |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Alchemist AI is a genuine Solana SPL token whose live mint was confirmed at HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump via public RPC. The account is a classic SPL mint owned by the Token program with six decimals and a supply near one billion. Both mint authority and freeze authority read null, meaning issuance is renounced and no account can be frozen, and there is no transfer fee because there are no Token 2022 extensions. The address ends in the pump suffix, confirming a pump.fun origin, and the project markets a real documented no code app builder. The one material gap is the website supplied for review: alchemistai.org is a parked lander page, while the true official site is alchemistai.app.

**Product reality:** PARTIAL genuine app builder but the official site is currently DOWN, returning HTTP 500 (Vercel, body {"message":"Internal Error"}) at review time  |  **Traction:** MIXED only one self reported figure (over 100,000 app builds) repeated by marketing, no independent verification and no user or retention metrics  |  **Token utility:** REAL_USED ALCH is a documented consumptive credit (200 ALCH per premium generation) plus premium model access and marketplace currency, though a free tier exists and speculation dominates market cap  |  **Delivery:** MATCHES many shipped features (image generation, multiplayer, 16 API integrations, GPT5 and Grok4 models, folders, version rollback, marketplace) match the pitch

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 1 |
| Low | 1 |
| Informational | 4 |
| **Total** | **6** |

MEFAI whole project analysis places Alchemist AI at 55 out of 100 (MEDIUM risk, Passed).

---

## 1. Project and Product Reality

Alchemist AI is a real no code app builder (Sacred Laboratory / Sandbox) where a user describes an app in plain language and a multi agent pipeline generates code, builds the UI, tests it in a sandboxed environment and deploys a playable web app. The product is genuinely feature rich: image generation, multiplayer games, sixteen third party API integrations, personal API keys, version rollback (Forge Rewind) and integration of GPT5 and Grok4, all documented in a live GitBook. However at review time the official product at www.alchemistai.app is DOWN, returning HTTP 500 on the apex, www and /sandbox paths (Vercel server error, JSON body {"message":"Internal Error"}), so the app itself could not be exercised. The documentation subdomain docs.alchemistai.app is up (HTTP 200), which confirms the product exists but does not prove the builder is currently functional. Net read: a real, substantive builder that is presently non operational due to a server side outage.

**Project level flags:** main product site down with HTTP 500 at review; pump.fun origin (mint ends in "pump"); alchemistai.org parked lookalike domain not official; no public third party smart contract or security audit; large speculative overhang (market cap around 67M USD versus thin proof of product revenue); free normal generation mode means the token is optional for basic use; single self reported traction number; very early launch (Nov 2024)

---

## 2. Website and Frontend Integrity

VERDICT: MINOR ISSUES | CONFIDENCE: med
The official domain www.alchemistai.app is a SvelteKit single page app on Vercel whose shell loads only same origin /_app/immutable assets with no remote scripts, no eval or atob or base64 obfuscation, and no drainer style approve or transfer or signature calls, it uses ordinary Solana wallet public key login for a read only session, it embeds no token address at all, and its manifest plus the GitBook docs at docs.alchemistai.app confirm genuine Alchemist AI and ALCH branding while the official mint HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump is corroborated by Solscan and exchanges with no wrong or lookalike mint seen anywhere. By contrast alchemistai.org is a parked GoDaddy lander and not the real product, since it JavaScript redirects to a /lander page carrying lander_type parkweb and traffic_target gd cookies and pulls monetization scripts from img1.wsimg.com, and it shows no app, no wallet connect, and crucially no token, so it is a parked lookalike and a user confusion or future phishing surface rather than a live drainer today. Two caveats keep this from a clean rating: at audit time the official .app returned HTTP 500 on every route so the site was effectively down, and I could not decode the current wallet transaction bundle because live asset hashes had rotated to 404 and the archived July 2025 bundles were brotli encoded with no decoder available, leaving the live signing path uninspected. Nothing observed points to a wallet drainer, an unbacked audit badge, or a substituted token, and no audit badge was even present on any reachable surface, but users should reach the token only through the .app or Solscan and treat alchemistai.org as untrusted. Overall the official property looks clean while the parked .org and the current .app outage are the only real flags.

---

## 3. Traction and Claims

The only traction figure available is "over 100,000 application builds" since the November 2024 launch, which is self reported and echoed across Solana Compass and secondary crypto blogs rather than independently audited. No monthly active users, retention, wallet, or marketplace transaction volume figures are published. Several deep dive articles that praise the product explicitly note zero adoption metrics beyond that single build count. The build number is plausible given the product is real, but it should be treated as unverified marketing rather than proven traction.

---

## 4. Token Utility and Economics

ALCH has genuine in product utility: official docs state generating an app in paid/premium mode costs 200 ALCH, users must recharge an in app Alchemist wallet with ALCH to access premium generation models, and ALCH is the medium of exchange in the Arcane Forge marketplace, with staking and governance also cited. This is real consumptive use rather than speculation only. Caveats: a free normal generation mode exists so casual users can skip the token entirely, and with 85 percent of the 1 billion supply dumped straight into a public liquidity pool the roughly 67M USD market cap is driven mainly by trading, not proven product spend. The mint HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump is confirmed in the official docs and its "pump" suffix marks a pump.fun origin.

The website provided for review, alchemistai.org, is not the official Alchemist AI site. Fetching it returns a tiny stub that redirects on load to a /lander parking page, a classic hallmark of a parked or lookalike domain. The genuine official presence is alchemistai.app, corroborated by the project X account alchemistAIapp and the documentation host docs.alchemistai.app. The documentation clearly describes the no code builder, iframe sandboxing, image generation, and the Arcane Forge marketplace. The directory should record alchemistai.app and treat alchemistai.org as an unofficial or squatted domain.

---

## 5. Claim versus Reality

- "No code development platform that turns natural language prompts into apps" / Reality: the product is real and documented at docs.alchemistai.app, and the ALCH token is a utility credit costing about 200 ALCH per app generation; the token itself is a plain SPL token and does no building on chain.
- "Secure Sandbox environment and image generation and marketplace" / Reality: docs describe iframe based sandboxing and an Arcane Forge marketplace, consistent marketing, but these are off chain app features with no on chain enforcement.
- Website supplied as alchemistai.org / Reality: that domain is a parked page that redirects to a /lander parking screen; the official domain is alchemistai.app, matching the X handle alchemistAIapp and docs.alchemistai.app.

---

## 6. Contract Security Check (supporting)

- Supply 999,953,125.101559 ALCH (raw 999953125101559), decimals 6, circulating roughly 850M per CoinGecko.
- Mint authority is null, so minting is renounced and supply is fixed.
- Freeze authority is null, so no holder balance can be frozen.
- Classic SPL token owned by TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA, mint account space 82 with no extensions, therefore no transfer fee; pump.fun origin confirmed by the pump address suffix; market cap roughly 24M to 67M USD across sources.

On chain reality is strong and matches a fixed supply utility token. The mint HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump is owned by the standard SPL Token program with an 82 byte account and no extensions, so there is no transfer fee or freeze extension hiding in the token. Mint authority and freeze authority both return null, meaning the team cannot inflate supply or freeze wallets. Total supply reads 999,953,125.101559 tokens at six decimals, and the pump suffix confirms the pump.fun bonding curve origin. Market cap sits in the low tens of millions of dollars after an 88 percent decline from the December 2025 peak, marking it a speculative but structurally clean asset.

### Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 10 |
| Supply and minting | 11 |
| Liquidity and market | 8 |
| Code safety | 11 |
| Transfer neutrality | 13 |
| Transparency | 2 |
| **Total** | **55/100** |

---

## 7. Findings by Severity

- HIGH: none. MEDIUM: the reviewed website alchemistai.org is a parked lookalike, not the official site; listing it risks sending users to a domain that could later be weaponized, so the directory must publish alchemistai.app. LOW: pump.fun origin speculative asset that fell roughly 88 percent from its December 2025 all time high near 0.24 USD, so high volatility. INFO: mint renounced, freeze renounced, no transfer fee, classic SPL with no hidden extensions, and a real documented product.

---

## 8. Conclusion

Alchemist AI is a genuine Solana SPL token whose live mint was confirmed at HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump via public RPC. The account is a classic SPL mint owned by the Token program with six decimals and a supply near one billion. Both mint authority and freeze authority read null, meaning issuance is renounced and no account can be frozen, and there is no transfer fee because there are no Token 2022 extensions. The address ends in the pump suffix, confirming a pump.fun origin, and the project markets a real documented no code app builder. The one material gap is the website supplied for review: alchemistai.org is a parked lander page, while the true official site is alchemistai.app. On the MEFAI whole project scale this token scores 55 out of 100 and is classified Passed.

---

## 9. Verification

- Methodology: product and traction review of the live project, a review of the project website frontend against its stated claims, and onchain analysis using read only public RPC on Solana.
- Sources:
  - `https://api.mainnet-beta.solana.com (getAccountInfo and getTokenSupply for HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump)`
  - `https://www.coingecko.com/en/coins/alchemist-ai`
  - `https://docs.alchemistai.app/docs/get-started`
  - `https://www.alchemistai.app/`
  - `https://x.com/alchemistAIapp`
  - `https://www.alchemistai.app`
  - `https://docs.alchemistai.app/docs/token-usdalch`
  - `https://docs.alchemistai.app/docs/get-started/ai-laboratory/features`
  - `https://solanacompass.com/projects/alchemist-ai`
  - `https://blog.millionero.com/blog/alchemist-ai-alch-a-deep-look-at-an-ai-powered-no-code-builder-on-solana/`
  - `https://www.geckoterminal.com/solana/pools/FyDF3vKQFbcvNTsBi7L7LremrFPmXKbQqgAgnPg1hXXd`
  - `https://alchemistai.org`

---

*Mefai Security Research. Independent whole project claim versus reality assessment.*
