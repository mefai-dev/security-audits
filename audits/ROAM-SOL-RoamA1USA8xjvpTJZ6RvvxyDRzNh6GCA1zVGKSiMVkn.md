# Security Audit Report: Roam (ROAM) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 4, 2026 |
| **Project** | Roam (formerly MetaBlox) |
| **Token Symbol** | ROAM |
| **Contract (Solana)** | `RoamA1USA8xjvpTJZ6RvvxyDRzNh6GCA1zVGKSiMVkn` |
| **Chain** | Solana (classic SPL Token; also bridged to BNB Chain) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **50/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Flagged** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements and website, and onchain data verified through MEFAI's onchain analysis. The assessments are Mefai Security Research's analysis and opinion. Data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Roam is a genuine wireless DePIN project with a real, actively used mobile app, which sets it apart from most of the tokens MEFAI reviews. The gap here is narrower than in a broken project, but it is real: the marketing dresses up static numbers as live traction, the token utility lives entirely inside one app, and the Solana mint still carries active mint and freeze authorities that were never renounced.

1. **The product is real and used.** The Roam app, formerly MetaBlox, is live on both the Apple App Store and Google Play under developer MetaBlox Labs Inc, carries strong ratings (4.7 on iOS across roughly 167 ratings, 4.78 on Android across around ten thousand ratings), and shows real download volume on Android on the order of 1.3 million lifetime installs. There is a working WiFi and eSIM product tied to OpenRoaming and a live node explorer. This is credited.

2. **The headline traction is marketing, not a live feed.** MEFAI's frontend review found that the site's showcase numbers, more than 100,000 users, over 127,000 measurement devices, and more than 3.7 billion data records, are hardcoded static values baked into the page HTML rather than an audited real time data source. The 127,000 figure refers to devices in the connectivity measurement network per the project litepaper, a different metric from the roughly 1.3 million app installs, so these are static litepaper marketing figures rather than a live audited feed.

3. **The token utility is confined to the app.** ROAM is a reward token: users earn Roam Points through daily check ins, adding hotspots, and referrals, and those points burn into ROAM that can be staked and used for governance. That utility is real but it lives inside the Roam application. Beyond exchange trading, there is no independent onchain utility, so for most holders ROAM is a rewards and speculative asset rather than the medium of a broad ecosystem.

4. **The Solana mint is not locked down.** A direct RPC read shows both the mint authority and the freeze authority are still active and were not renounced. The issuer can therefore mint additional supply and can freeze individual holder accounts. The advertised one billion cap is a policy statement, not an enforced onchain limit, and about 99.6 percent of that cap has already been minted.

Roam is not a scam and the app is real, which keeps the overall risk at medium. As a project audited on claims versus reality, however, the static traction, the app only utility, and the unrenounced mint and freeze authorities weigh it down to 50 out of 100, Flagged.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Roam / ROAM |
| **Contract (Solana)** | `RoamA1USA8xjvpTJZ6RvvxyDRzNh6GCA1zVGKSiMVkn` |
| **Decimals** | 6 |
| **Max supply** | 1,000,000,000 ROAM (policy cap only, not an enforced onchain hard cap) |
| **Total minted** | About 995.63 million (raw 995632498015410), roughly 99.6 percent of the policy cap |
| **Circulating** | About 358 million, roughly 36 percent (varies across trackers) |
| **History** | Rebranded from MetaBlox to Roam and migrated onto Solana Mainnet; token also bridged to BNB Chain |
| **Token program** | Classic SPL Token (not Token 2022), standard mint layout, no extensions |
| **Contract controls** | Mint authority active, freeze authority active, both populated and not renounced |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana RPC read of the ROAM mint returned:

| Check | Result |
|-------|--------|
| Token identity | ROAM, 6 decimals, mint address confirmed live and matching CoinGecko, Solana Compass, CoinCarp, and multiple exchanges |
| Token program | Classic SPL Token, standard layout, no extensions |
| Supply | About 995.63 million minted against a stated 1 billion policy cap |
| Mint authority | `DqeBtBQ5Ue4Ms7kjJ9kienccL8piCbiLJRfY3Dp7dRzJ` (active, not null, not renounced) |
| Freeze authority | `6oSNcJSzSr7UcAcJDKdD3N2tiu2TL6KGVC2etFS3HaM1` (active, not null, not renounced) |
| Transfer fee | None |
| Transfer hook | None |

**Interpretation.** At the code level the mint is clean and conventional: a standard SPL token with no transfer fee, no transfer hook, and no exotic Token 2022 extensions, so there is no hidden tax or blocklist mechanism woven into the token program. The concern is control, not code. Both privileged authorities remain live. The active mint authority means the one billion cap is a promise rather than a cryptographic guarantee, and supply could in principle be pushed past the advertised ceiling. The active freeze authority means the issuer can freeze any individual holder account. Neither authority is disclosed as sitting behind a published multisig, so the residual risk here is centralization over both supply and holder funds. This is the main reason the token side of the score is held back.

---

## 3. Claim vs Reality: "A real WiFi DePIN app used around the world"

> Site and stores: Roam presents a working DePIN WiFi and eSIM mobile app that lets users connect to millions of OpenRoaming hotspots and earn rewards, positioned as formerly MetaBlox.

**Reality: this claim holds and is credited.** The app is genuinely live and maintained. It ships on the Apple App Store as Roam: Global eSIM & WiFi and on Google Play as com.dapp.metablox, both published by MetaBlox Labs Inc. Ratings are strong, about 4.7 on iOS and about 4.78 on Android, and Android install volume is substantial, on the order of 1.3 million lifetime downloads with a steady daily install rate. A live node explorer exists, and the product integrates real connectivity standards such as OpenRoaming and DID based authentication. Unlike many DePIN narratives, there is a real, downloadable, well rated product behind this one. Roam earns clear credit for shipping.

---

## 4. Claim vs Reality: "100,000 plus users, 127,000 plus measurement devices, 3.7 billion plus data records"

> Site: the Roam marketing page presents these headline metrics as evidence of live traction.

**Reality: static marketing numbers, not a live audited feed.** MEFAI's frontend review found that these figures are hardcoded into the page HTML rather than served from a live or audited data source. The 127,000 figure is the project litepaper count of devices in the connectivity measurement network, a different metric from the roughly 1.3 million app installs the Android store reports, so it should be read as a static marketing figure rather than a live download counter. Presenting fixed values styled as real time traction is normal for a brochure page but is a transparency concern when the numbers are read as a live dashboard. The underlying product does have real users, but these specific on page metrics should be treated as marketing rather than measured traction.

---

## 5. Claim vs Reality: "Fixed maximum supply of 1,000,000,000 ROAM"

> Site and documentation: ROAM has a fixed maximum supply of one billion, split roughly sixty percent to growth, mining, and community, twenty eight percent to investors, and twelve percent to the team under a six year vesting plan, with an emission curve starting near 0.6 percent monthly.

**Reality: the cap is policy, not enforced onchain.** The headline numbers line up with chain data, total minted reads about 995.63 million against the stated one billion and circulating supply is near 358 million, so the disclosed tokenomics are broadly consistent with what is live. The important caveat is that the one billion ceiling is not an onchain hard cap. Because the mint authority is still active, the issuer retains the technical ability to mint beyond the advertised limit. With roughly 99.6 percent of the cap already minted there is little headroom left in the plan itself, but the guarantee that supply stops at one billion rests on the team's discretion rather than on the token program.

---

## 6. Claim vs Reality: "ROAM powers the network"

> Site: ROAM is presented as the token that rewards participation and powers the Roam network.

**Reality: real utility, but confined to the app.** ROAM is a functioning reward token inside the Roam application. Users accumulate Roam Points through check ins, hosting or adding hotspots, and referrals, and those points burn into ROAM, which can then be staked and used for community governance. The token is broadly listed, including Bybit, Bitget, Gate, KuCoin, and MEXC, so it has market liquidity. What it does not have is independent onchain utility beyond the app and trading, so for a holder who is not actively running or using the app the token functions mainly as a rewards and speculative asset rather than as the settlement layer of a wider ecosystem.

---

## 7. Claim vs Reality: "Your tokens are yours"

> Implicit: as a listed SPL token, ROAM is presented as a standard freely transferable asset.

**Reality: the issuer can freeze holder accounts.** The freeze authority on the mint is active and was not renounced, which means the issuer can freeze any individual ROAM account and block its transfers. This is a standard SPL capability, and there is no evidence it has been abused, but for holders it is a live counterparty power over their own balances. Combined with the active mint authority, it means custody of ROAM is not fully trustless at the token level.

---

## 8. Positive Findings (Credited)

- The core product is real and actively used, with a maintained app on both major stores, strong ratings, and roughly 1.3 million Android installs.
- The token is a clean classic SPL mint with no transfer fee and no transfer hook, so there is no hidden tax or blocklist logic in the token program.
- Disclosed tokenomics broadly match chain data, with total minted near the stated cap and circulating supply consistent with the published figures.
- ROAM has genuine in app utility as a reward token, with points that burn into ROAM plus staking and governance, and it carries real exchange liquidity.

---

## 9. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| ROAM 001 | **HIGH** | Mint and freeze authorities are both active and not renounced. The issuer can mint additional supply and can freeze any individual holder account. |
| ROAM 002 | **MEDIUM** | The one billion max supply is a policy statement, not an enforced onchain hard cap; about 99.6 percent is already minted and the ceiling depends on issuer discretion. |
| ROAM 003 | **MEDIUM** | Headline traction (100,000 plus users, 127,000 plus measurement devices, 3.7 billion plus data records) is hardcoded static HTML from the project litepaper, not a live feed. |
| ROAM 004 | **LOW** | ROAM utility is confined to the app (points burn into ROAM, staking, governance); beyond trading there is no independent onchain utility, so value is largely rewards and speculative. |
| ROAM 005 | **LOW** | ROAM is also bridged to BNB Chain, adding a cross chain supply and bridge trust surface beyond the Solana mint. |
| ROAM 006 | **INFO** | Classic SPL token, no transfer fee, no transfer hook, no Token 2022 extensions (positive). |
| ROAM 007 | **INFO** | Real, maintained, well rated app on the Apple App Store and Google Play under MetaBlox Labs Inc (positive). |

---

## 10. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified mint, clean classic SPL, no fee, no hook |
| Supply / minting | High risk | Mint authority active, cap is policy only, about 99.6 percent already minted |
| Holder control | High risk | Freeze authority active; issuer can freeze individual accounts |
| Product reality | Low risk | Real, maintained, well rated app with substantial installs |
| Traction | Medium risk | Real usage exists, but on site headline metrics are static litepaper figures |
| Utility | Medium risk | Genuine in app reward utility, but no independent onchain utility |
| Transparency | Medium risk | Static numbers styled as live traction; unrenounced authorities not disclosed behind a published multisig |

---

## 11. Technical Specifications

| Item | Value |
|------|-------|
| Contract | `RoamA1USA8xjvpTJZ6RvvxyDRzNh6GCA1zVGKSiMVkn` |
| Chain | Solana Mainnet (classic SPL Token) |
| Decimals | 6 |
| Total minted | About 995.63 million (raw 995632498015410) |
| Policy cap | 1,000,000,000 ROAM (not an onchain hard cap) |
| Circulating | About 358 million, roughly 36 percent |
| Mint authority | `DqeBtBQ5Ue4Ms7kjJ9kienccL8piCbiLJRfY3Dp7dRzJ` (active) |
| Freeze authority | `6oSNcJSzSr7UcAcJDKdD3N2tiu2TL6KGVC2etFS3HaM1` (active) |
| Transfer fee | None |
| Transfer hook | None |
| Token 2022 extensions | None |
| Cross chain | Also bridged to BNB Chain |

---

## 12. Conclusion

Roam is one of the more legitimate DePIN projects MEFAI has reviewed, because the product is real. There is a working WiFi and eSIM app on both stores, it is maintained, it is well rated, and it has genuine users and installs. That earns real credit and keeps the overall risk at medium rather than high. As a claim versus reality audit, though, three things pull the score down. The traction the marketing site puts forward is hardcoded litepaper figures rather than a live feed, the token's utility is confined to the app so most holders are exposed to a rewards and speculative asset, and the Solana mint still carries active mint and freeze authorities that let the issuer expand supply past the advertised cap and freeze individual holders. None of these is a scam signal, but together they describe a real product wrapped in overstated on page metrics and a token that is not yet locked down or independently useful. This lands Roam at 50 out of 100, Flagged.

---

## 13. Recommendations

**For the Roam team:**
- Renounce the mint authority, or move it behind a disclosed and published multisig, so the one billion cap becomes a real onchain guarantee rather than a policy promise.
- Renounce or clearly justify and disclose the freeze authority, and publish who controls it, so holders understand the counterparty power over their balances.
- Replace the hardcoded headline metrics with a live and auditable feed, or clearly label them as static marketing figures, and reconcile the download number with the actual store installs.
- Broaden ROAM's utility beyond the app, or stop implying the token powers a wider ecosystem than the rewards program it currently serves.

**For users:**
- Treat the on site headline numbers as marketing; the underlying app is real, but those specific metrics are static litepaper figures rather than a live feed.
- Understand that ROAM supply is not cryptographically fixed and that the issuer can freeze individual accounts, since both authorities remain active.
- Recognize that ROAM's real utility today is inside the Roam app as a reward and governance token; outside the app and exchanges it is largely a speculative holding.

---

## 14. Verification

- MEFAI onchain analysis: a direct Solana RPC read of the ROAM mint confirming the mint address, 6 decimals, classic SPL Token program with no extensions, no transfer fee, no transfer hook, total minted about 995.63 million, and both mint authority `DqeBtBQ5Ue4Ms7kjJ9kienccL8piCbiLJRfY3Dp7dRzJ` and freeze authority `6oSNcJSzSr7UcAcJDKdD3N2tiu2TL6KGVC2etFS3HaM1` still active. Address cross checked against CoinGecko, Solana Compass, CoinCarp, and multiple exchange listings.
- Product checks: live confirmation of the Roam app on the Apple App Store (Roam: Global eSIM & WiFi, developer MetaBlox Labs Inc, about 4.7 rating) and Google Play (com.dapp.metablox, about 4.78 rating, roughly 1.3 million installs), plus the existence of a live node explorer and OpenRoaming based connectivity.
- Frontend review: MEFAI integrity review of www.roam.network finding a conventional marketing and download page with no web3 or wallet connect surface, and headline metrics (100,000 plus users, 127,000 plus measurement devices, 3.7 billion plus data records) hardcoded as static HTML rather than a live data source.
- Project statements: the project's website and documentation (formerly MetaBlox, one billion policy cap, allocation and vesting plan, emission curve, and Roam Points burning into ROAM with staking and governance).
