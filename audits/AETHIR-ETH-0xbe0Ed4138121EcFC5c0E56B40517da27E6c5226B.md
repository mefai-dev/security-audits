# Security Audit Report: Aethir (ATH) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Aethir |
| **Token Symbol** | ATH |
| **Contract (Ethereum, canonical)** | `0xbe0Ed4138121EcFC5c0E56B40517da27E6c5226B` |
| **Also on** | Arbitrum One `0xc87B37a581ec3257B734886d9d3a581F5A9d056c`; Solana `Dm5BxyMetG3Aq5PaG1BrG7rBYqEMtnkjvPNMExfacVk7` |
| **Chain** | Ethereum (and Arbitrum, Solana) |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **60/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Aethir is a real, funded GPU cloud compute network with a verified, fixed cap token and reputable backers. It is not a scam. The MEDIUM rating reflects a gap between the "enterprise grade decentralized GPU cloud" branding and a more curated, self reported, dilution heavy reality:

1. **The token cap is real and on chain:** MEFAI confirms the Ethereum contract holds exactly 42 billion ATH, the full fixed supply, already pre minted, so there is no protocol level inflation.
2. **But the "decentralized" network is a curated, permissioned set.** GPU providers must apply, submit a hardware inventory, pass identity checks, meet approved location and specification requirements and stake ATH, so the supply side is a vetted business to business set closer to a broker than a permissionless network.
3. **Headline scale and revenue are self reported.** The advertised container count conflates virtualized slices with physical enterprise GPUs, and the flagship high end GPU count is a tiny fraction of it; revenue figures come from the project's own reporting, not an independent audit.
4. **Heavy emissions and unlocks:** roughly half the supply is still locked, and circulating supply is expanding steeply, diluting holders even at a fixed cap; the token is down roughly 97 percent from peak.

The product and backers are real; the caution is curated permissioned supply, self reported metrics and dilution from emissions.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | Aethir Token / ATH |
| **Contract (Ethereum, canonical)** | `0xbe0Ed4138121EcFC5c0E56B40517da27E6c5226B` |
| **Decimals** | 18 |
| **Max supply (verified)** | 42,000,000,000 ATH (fixed, fully pre minted on Ethereum) |
| **Circulating** | ~20 billion ATH (~48 percent), expanding on a 2024 to 2028 emission curve |
| **Provider access** | Permissioned: identity checks, approved location and specification, ATH stake, plus a license NFT for checker nodes |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI verified the ATH token:

| Check | Result |
|-------|--------|
| Token identity | "Aethir Token", symbol ATH, 18 decimals, verified |
| Ethereum total supply | Exactly 42,000,000,000 ATH (the full fixed cap, pre minted on L1) |
| Protocol inflation | None: supply is fixed and already at cap on Ethereum |
| Multichain | Arbitrum One (compute and checker rewards, a bridged mint and burn asset) and Solana |
| Circulating | ~20 billion (~48 percent); roughly 52 percent still locked |
| Emissions | Provider and ecosystem allocations vest 2024 to 2028, so circulating supply is rising steeply |

**Interpretation.** ATH has a genuine, on chain verified fixed cap of 42 billion, a real positive versus uncapped peers. The catch is that circulating supply roughly quintupled between 2024 and 2026 as allocations vest, so effective dilution is steep even though the contract mints no new supply, and a large 2024 to 2028 emission overhang remains.

---

## 3. Claim vs Reality: "Enterprise Grade Decentralized GPU Cloud"

> Site: an "enterprise grade" decentralized GPU cloud for AI and gaming, hundreds of thousands of GPUs across dozens of countries, with checker nodes providing decentralized verification.

**Reality: a curated, permissioned aggregator with distributed hardware.** The headline container count measures virtualized GPU slices, not physical enterprise GPUs, and the project's own materials put the flagship high end data center GPU count in the low thousands, a small fraction of the container headline. GPU providers are not permissionless: they apply, submit a hardware inventory, pass identity checks, meet approved location and specification requirements and stake ATH before onboarding, and checker admission has approval and ban gating. This is closer to a vetted business to business broker than a permissionless decentralized network.

---

## 4. Claim vs Reality: Revenue, Emissions and Value

- **Self reported revenue:** the advertised annual recurring revenue and compute hour figures come from the project's own reporting and partner write ups, with no independent audit or on chain settlement proof, so they should be treated as directional and unverified.
- **Insider allocation and unlocks:** insider aligned buckets (team, investors, advisors) total on the order of 29 percent, and continuing large monthly unlocks create persistent sell pressure.
- **Effective dilution:** with roughly 52 percent still locked and circulating supply expanding on the emission curve, holders are diluted even though the cap is fixed.
- **Value:** the token is down roughly 97 percent from its 2024 peak.
- **Backers (positive):** genuinely funded by reputable venture backers, with a real product and major exchange listings, which partially offsets the concerns above.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| ATH 001 | **MEDIUM** | "Decentralized" GPU network is a permissioned, identity checked, curated provider set with staking and approval gating. |
| ATH 002 | **MEDIUM** | Headline container and revenue metrics are self reported and conflate virtualized slices with physical enterprise GPUs. |
| ATH 003 | **MEDIUM** | Roughly 52 percent of supply locked with heavy 2024 to 2028 emissions and monthly unlocks; steep effective dilution. |
| ATH 004 | **LOW** | ~97 percent drawdown from peak. |
| ATH 005 | **INFO** | On chain verified fixed cap of 42 billion (no protocol inflation); real product and reputable backers (positives). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, fixed cap, reputable backers |
| Supply / minting | Medium risk | Fixed cap but steep emission driven dilution |
| Decentralization | Medium risk | Permissioned, identity checked provider set |
| Claim accuracy | Medium risk | Container and revenue metrics self reported |
| Product reality | Low to medium risk | Real GPU cloud, real listings |
| Value / volatility | High risk | ~97 percent drawdown, unlock overhang |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Contract (Ethereum) | `0xbe0Ed4138121EcFC5c0E56B40517da27E6c5226B` |
| Decimals | 18 |
| Max supply | 42,000,000,000 ATH (fixed, pre minted) |
| Circulating | ~20 billion (~48 percent) |
| Arbitrum / Solana | `0xc87B37a581ec3257B734886d9d3a581F5A9d056c` / `Dm5BxyMetG3Aq5PaG1BrG7rBYqEMtnkjvPNMExfacVk7` |

---

## 8. Conclusion

Aethir is a real, funded GPU cloud compute network with an on chain verified fixed cap of 42 billion ATH and reputable backers, which keeps it in the MEDIUM band at 60/100. It is held back because the "decentralized" network is a permissioned, identity checked, curated provider set, because headline container and revenue metrics are self reported and conflate virtualized slices with physical enterprise GPUs, and because roughly 52 percent of supply is locked with heavy 2024 to 2028 emissions diluting holders even at a fixed cap, against a roughly 97 percent drawdown. The product is real; the caution is curated permissioned supply, self reported metrics and dilution.

---

## 9. Recommendations

**For the Aethir team:**
- Report physical enterprise GPU counts separately from virtualized container counts, and publish independently verifiable revenue.
- Disclose the permissioned, identity checked provider onboarding clearly rather than framing the network as permissionless.
- Publish the forward emission and unlock schedule prominently so dilution is transparent.

**For users:**
- Understand the fixed 42 billion cap does not prevent steep dilution as locked allocations vest through 2028.
- Treat scale and revenue claims as self reported, and model the unlock overhang and drawdown.

---

## 10. Verification

- MEFAI on chain analysis: a direct read of the ATH token on Ethereum (identity, 18 decimals, total supply exactly 42 billion at cap) and confirmation of the Arbitrum and Solana deployments.
- The contract addresses, supply and cap are publicly verifiable on the Ethereum, Arbitrum and Solana explorers.
- Project statements: the project's website and documentation (the enterprise grade GPU cloud and container count claims, the checker node and provider onboarding requirements, the revenue reporting and the tokenomics and emission schedule).
