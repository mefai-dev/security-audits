# Security Audit Report: Alchemist AI (ALCH) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Alchemist AI |
| **Token Symbol** | ALCH |
| **Contract (Solana SPL mint)** | `HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump` |
| **Chain** | Solana (SPL token, classic Token program) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **55/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Passed** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Alchemist AI is one of the more substantive AI product tokens MEFAI has reviewed in this batch, but it carries a set of real operational and provenance cautions that keep it out of the top tier. The token contract is clean and the product is genuine, yet the flagship site was down at review time, the traction story rests on a single unverified number, and the launch pedigree is a pump.fun memecoin rather than a funded product.

1. **The contract is clean and fixed.** MEFAI's live Solana read confirmed a classic SPL mint with six decimals, a fixed supply near one billion, mint authority renounced (null), freeze authority renounced (null), and no transfer fee or Token 2022 extensions. The common rug and honeypot vectors are absent at the contract level.
2. **The product is real and the token utility is genuine.** Alchemist AI is a documented no code app builder, and ALCH is an actual consumptive credit: project materials state a premium generation costs 200 ALCH, users recharge an in app wallet with ALCH for premium models, and ALCH is the currency of the Arcane Forge marketplace. This is real token utility, not an empty narrative, and MEFAI credits it.
3. **The official site was down at review time.** Every route on www.alchemistai.app returned HTTP 500 (Vercel Internal Error) during the review, so the builder itself could not be exercised. The documentation subdomain docs.alchemistai.app was reachable and confirms the product exists, but the live product was effectively offline.
4. **Traction and provenance are the soft spots.** The only adoption figure available is a self reported "over 100,000 app builds," repeated by marketing with no independent verification and no user, retention, or revenue metrics. The token launched on pump.fun (the mint address ends in `pump`), and a lookalike domain, alchemistai.org, is a parked GoDaddy lander that creates a user confusion and future phishing surface.

The contract is not a scam and the product is not vaporware. Alchemist AI is a legitimate, feature rich builder with a token that is actually used, which is why it passes. The MEDIUM rating and the 55 out of 100 reflect an offline flagship at review time, unproven traction, a speculative pump.fun profile, and a parked lookalike domain that a security minded user needs to be aware of.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Alchemist AI / ALCH |
| **Contract (Solana SPL mint)** | `HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump` |
| **Decimals** | 6 |
| **Total supply** | 999,953,125.101559 ALCH (fixed, near one billion; circulating roughly 850M per CoinGecko) |
| **Origin** | pump.fun bonding curve launch (mint address suffix `pump`), live since November 2024 |
| **Contract controls** | Mint authority renounced (null) and freeze authority renounced (null); classic SPL, no Token 2022 extensions |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct read of the ALCH mint on Solana via public RPC returned:

| Check | Result |
|-------|--------|
| Token identity | ALCH, 6 decimals, classic SPL mint owned by the Token program |
| Total supply | 999,953,125.101559 ALCH (raw 999953125101559), fixed |
| Mint authority | Null (renounced): supply cannot be inflated |
| Freeze authority | Null (renounced): no holder balance can be frozen |
| Upgradeable / extensions | None: 82 byte account, no Token 2022 extensions |
| Transfer fee | None (no fee extension present) |

**Interpretation.** At the contract level ALCH is strong. It is a classic SPL token owned by the standard Token program (`TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA`) with both mint and freeze authorities renounced, so the team cannot print new supply or freeze wallets, and there is no hidden transfer fee or freeze extension because the mint carries no Token 2022 extensions. This removes the usual rug and honeypot vectors. The residual token level caution is provenance rather than mechanics: the address suffix `pump` confirms a pump.fun origin, and the asset trades as a speculative memecoin, down roughly 88 percent from its December 2025 all time high near 0.24 USD. The material concerns for this project sit at the product, traction, and domain level below, not in the token contract.

---

## 3. Claim vs Reality: "No code app builder that turns a description into an app"

> Site: Alchemist AI is presented as a no code development platform where a plain language description is turned into a working software application, from simple utilities to games, generated on the fly.

**Reality: the product is genuinely real, but the live site was down at review time.** MEFAI confirms the builder is real and substantive. The documentation at docs.alchemistai.app describes a multi agent pipeline that generates code, builds a UI, tests it in a sandboxed environment, and deploys a playable web app, with shipped features including image generation, multiplayer, sixteen third party API integrations, personal API keys, version rollback (Forge Rewind), and integration of current frontier models. This is a delivered product, not an empty narrative. The caution is operational: at review time every route on www.alchemistai.app returned HTTP 500 (Vercel Internal Error, body `{"message":"Internal Error"}`) on the apex, www, and /sandbox paths, so the builder itself could not be exercised. The documentation subdomain returned HTTP 200 and confirms the product exists, but the flagship experience was effectively offline during the review. A real product whose official site remained down through the review, still returning HTTP 500, is a caution, not a red flag, and MEFAI notes it as such.

---

## 4. Claim vs Reality: "ALCH powers premium generation and the marketplace"

> Site: ALCH is described as the utility credit of the platform, consumed for premium app generation and used as the marketplace currency.

**Reality: the token utility is genuine and MEFAI credits it.** Unlike many AI token narratives, ALCH has documented, consumptive in product use. The reachable official documentation confirms that users recharge an in app Alchemist wallet with ALCH to access premium generation models, and project materials describe a premium generation cost of 200 ALCH and an Arcane Forge marketplace denominated in ALCH, with staking and governance also cited. This is real utility that ties the token to actual product usage. Two fair caveats keep this from being a clean positive: a free normal generation mode exists, so casual users can use the builder without ever touching the token, and with the large majority of supply having entered a public liquidity pool at launch, the low tens of millions market cap is driven mainly by trading rather than proven product spend. The utility is real; it is simply optional for basic use and not yet corroborated by published revenue.

---

## 5. Claim vs Reality: "Over 100,000 app builds"

> Site: The project points to more than one hundred thousand application builds since launch as proof of traction.

**Reality: a single self reported figure with no independent verification.** The "over 100,000 builds" number is the only adoption metric in circulation, and it is echoed across Solana Compass and secondary crypto blogs rather than independently audited. No monthly active users, retention, wallet, or marketplace transaction volume figures are published, and several deep dive articles that otherwise praise the product explicitly note the absence of adoption metrics beyond that single count. The number is plausible given the product is real, but a security minded reader should treat it as unverified marketing rather than proven traction until MEFAI can corroborate it with onchain or independent data.

---

## 6. Claim vs Reality: Official domain and the parked lookalike

> Site: The project's official presence is at alchemistai.app, corroborated by the X handle alchemistAIapp and the docs host docs.alchemistai.app.

**Reality: the official domain is legitimate, but a lookalike domain is a live user confusion and phishing surface.** MEFAI confirms that www.alchemistai.app is the genuine property: a SvelteKit single page app on Vercel that loads only same origin assets, embeds no token address, uses ordinary read only Solana wallet login, and shows no drainer style approve, transfer, or blind signature calls and no unbacked audit badge. By contrast, alchemistai.org is not the real product. It is a parked GoDaddy lander that JavaScript redirects to a /lander parking page carrying `lander_type parkweb` and `traffic_target gd` cookies and pulls monetization scripts from `img1.wsimg.com`, with no app, no wallet connect, and no token present. It is a parked lookalike today rather than an active drainer, but it is a real user confusion risk and a future phishing surface: a squatted domain one character away from the official site can be weaponized later. Users should reach the token and product only through alchemistai.app or a reputable explorer such as Solscan, and treat alchemistai.org as untrusted.

---

## 7. Positive Findings (Credited)

- The mint is clean at the contract level: classic SPL, mint authority renounced (null), freeze authority renounced (null), no transfer fee, and no Token 2022 extensions, verified live via public RPC.
- The token utility is genuine: 200 ALCH per premium generation, ALCH recharge for premium models, and ALCH as the Arcane Forge marketplace currency, with the recharge mechanism documented in the official GitBook.
- The product is real and feature rich, with many shipped capabilities (image generation, multiplayer, sixteen API integrations, personal API keys, version rollback, frontier model access) that match the pitch.
- The official website is clean: no remote drainer scripts, no embedded token address, read only wallet login, and no unbacked audit badge on any reachable surface.

---

## 8. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| ALCH 001 | **MEDIUM** | Official product site down at review time: every route on www.alchemistai.app returned HTTP 500 (Vercel Internal Error), so the live builder could not be exercised. |
| ALCH 002 | **MEDIUM** | Parked lookalike domain alchemistai.org (GoDaddy parking lander) creates a user confusion and future phishing surface one character from the official domain. |
| ALCH 003 | **MEDIUM** | Traction rests on a single self reported figure (over 100,000 builds) with no independent verification and no user, retention, or revenue metrics. |
| ALCH 004 | **LOW** | pump.fun origin and speculative memecoin profile; the asset fell roughly 88 percent from its December 2025 all time high, so high volatility. |
| ALCH 005 | **LOW** | No public third party smart contract or security audit of the project beyond MEFAI's onchain read. |
| ALCH 006 | **INFO** | Contract clean and token utility real: mint and freeze renounced, no transfer fee, classic SPL, backed by a genuine documented product (positive). |

---

## 9. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified classic SPL, 6 decimals, no transfer fee, no extensions |
| Supply / minting | Low risk | Mint authority renounced (null); supply fixed near one billion |
| Token utility | Low risk | Genuine consumptive credit (200 ALCH per premium generation) plus marketplace use |
| Product reality | Medium risk | Real feature rich builder, but the official site returned HTTP 500 at review time |
| Traction | Medium risk | Single unverified 100,000 builds figure; no independent adoption metrics |
| Provenance / domain | Medium risk | pump.fun origin; parked lookalike alchemistai.org as a phishing surface |

---

## 10. Technical Specifications

| Item | Value |
|------|-------|
| Contract (SPL mint) | `HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump` |
| Chain / program | Solana, classic Token program `TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA` |
| Decimals | 6 |
| Total supply | 999,953,125.101559 ALCH (fixed) |
| Mint authority | Null (renounced) |
| Freeze authority | Null (renounced) |
| Extensions / upgradeable | None (82 byte account, no Token 2022 extensions) |
| Transfer fee | None |
| Origin | pump.fun bonding curve (address suffix `pump`) |
| Official site | https://www.alchemistai.app (HTTP 500 at review time); docs at https://docs.alchemistai.app |

---

## 11. Conclusion

Alchemist AI passes. The token contract is clean at the mechanical level, with renounced mint and freeze authorities, a fixed supply, no transfer fee, and no hidden extensions, and the project is backed by a genuine, feature rich no code app builder whose token has real consumptive utility (200 ALCH per premium generation and marketplace currency). That combination of a clean contract and real token utility is why Alchemist AI earns a Passed verdict rather than a Flag. The score of 55 out of 100 and the MEDIUM overall risk reflect the cautions that sit around, not inside, the token: the official site was returning HTTP 500 and was effectively offline at review time, the traction story rests on a single unverified 100,000 builds figure, the asset carries a speculative pump.fun profile that is well off its peak, and a parked lookalike domain, alchemistai.org, poses a user confusion and future phishing risk. The caution here is not a contract exploit; it is an otherwise legitimate product whose live surface and provenance need to firm up.

---

## 12. Recommendations

**For the Alchemist AI team:**
- Restore full availability of www.alchemistai.app; a flagship product should not return HTTP 500 across every route.
- Publish independent or onchain backed traction metrics (active users, generations, marketplace volume) instead of a single self reported build count.
- Seek to control or clearly disavow alchemistai.org, and prominently direct users to the .app domain to reduce the lookalike phishing surface.
- Commission a public third party review of the offchain application and signing paths to complement the clean onchain contract.

**For users:**
- Reach the token and product only through alchemistai.app or a reputable explorer such as Solscan; treat alchemistai.org as an untrusted parked lookalike.
- Recognize that ALCH is a genuine utility credit but also a speculative pump.fun asset that is highly volatile and well off its all time high.
- Treat the "over 100,000 builds" figure as unverified marketing until independently corroborated, and note the official site was offline at review time.

---

## 13. Verification

- MEFAI onchain analysis: a direct Solana RPC read of the ALCH mint `HNg5PYJmtqcmzXrv6S9zP1CDKk5BgDuyFBxbvNApump` (identity, 6 decimals, fixed supply of 999,953,125.101559, mint authority null, freeze authority null, classic SPL owned by the Token program, 82 byte account with no Token 2022 extensions, no transfer fee), cross checked against Solscan and CoinGecko.
- Product and site checks: live fetches of www.alchemistai.app (HTTP 500, Vercel Internal Error across apex, www, and /sandbox) and docs.alchemistai.app (HTTP 200), a frontend integrity read of the official SvelteKit app (same origin assets, no drainer calls, read only wallet login, no unbacked audit badge), and inspection of alchemistai.org (GoDaddy parking lander, /lander redirect, wsimg.com monetization scripts, no token).
- Project statements: the project's documentation and marketing (the no code builder positioning, the 200 ALCH per premium generation cost, the Arcane Forge marketplace, and the "over 100,000 builds" traction figure), and confirmation of the pump.fun origin from the mint address suffix.
