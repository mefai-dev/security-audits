# Security Audit Report: Arweave (AR) on Arweave

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 1, 2026 |
| **Project** | Arweave |
| **Token Symbol** | AR |
| **Native token** | AR (Arweave Layer 1, the blockweave) |
| **Chain** | Arweave |
| **Audit Type** | Project + Network (Claim vs Reality) |
| **Mefai Security Score** | **74/100** |
| **Overall Risk** | **LOW** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Arweave is one of the more genuine decentralized storage networks: a real, long running Layer 1 with a near fully circulating, effectively fixed supply token and a doxxed team. It is not vaporware. The LOW rating (on security and scam risk) reflects a legitimate network, while the caution is economic and forward looking:

1. **The token and supply are clean:** MEFAI confirms a hard cap of 66 million AR with roughly 99.5 percent already circulating, so there is very little future emission and negligible unlock or dilution risk.
2. **But the "store forever" guarantee rests on an unproven economic assumption.** The endowment that funds perpetual storage depends on storage costs continuing to fall over very long horizons, which no live network has ever validated.
3. **Real usage is modest** relative to the "world's permanent hard drive" framing, and storage replication concentrates among larger miners.
4. **The AO compute and AI narrative is new and unproven,** is a separate token and economy, and rides the AI cycle ahead of demonstrated traction.

The team and network are real and the token mechanics are clean; the caution is the untested endowment economics, a deep drawdown and an early AI compute narrative.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token** | AR (native gas, storage payment and mining reward) |
| **Chain** | Arweave, its own Layer 1 (the blockweave) |
| **Decimals** | 12 (the base unit is the winston, 1 AR equals 10 to the 12 winston) |
| **Max supply** | 66,000,000 AR (hard cap; 55 million minted at genesis, 11 million released as block rewards) |
| **Circulating** | ~65.65 million AR (~99.5 percent); only a few hundred thousand AR left to emit |
| **Emission** | Effectively fixed with a small, shrinking tail emission asymptoting to the cap |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI reviewed AR's supply and network model:

| Check | Result |
|-------|--------|
| Token type | Native AR on the Arweave Layer 1; not an ERC 20 |
| Decimals | 12 (winston base unit) |
| Max supply | 66,000,000 AR (hard cap) |
| Circulating | ~65.65 million (~99.5 percent), near fully circulating |
| Emission | Small, shrinking block reward tail; effectively fixed going forward |
| Consensus | Succinct proofs of random access (an evolution of the original proof of access), with packing that rewards miners holding complete replicas of the weave |

**Interpretation.** AR is a genuine, near fully circulating, effectively fixed supply native token with a doxxed team, so rug, dilution and unlock risks are low. The real questions are economic sustainability and usage, not token mechanics.

---

## 3. Claim vs Reality: "Permanent Storage, Pay Once Store Forever"

> Site: permanent, decentralized data storage, "pay once, store forever" via a storage endowment; the permaweb of provably neutral web apps.

**Reality: a real model with an unproven long horizon assumption.** Uploads are a one time fee, and most of the fee goes into a storage endowment designed to pay miners' future storage costs (users are priced for roughly 200 years at current rates). The critical assumption is that storage costs keep falling over time so the endowment yield outpaces perpetual storage cost. If that decline slows or stalls, or if the endowment value stays depressed, the perpetual guarantee is unproven at multi century horizons. No live network has ever validated a multi century endowment, so "store forever" is a well designed but untested promise.

---

## 4. Claim vs Reality: Usage, Decentralization and the AO Layer

- **Modest usage:** total data stored is on the order of a few hundred TiB with daily uploads of tens of GiB, real and growing but small versus centralized cloud and versus the "world's permanent hard drive" framing.
- **Miner concentration:** nodes span many countries, but full weave storage economics favor larger operators, so replication concentrates among a limited set of large miners; decentralization is real but not as flat as the "Bitcoin for data" framing implies.
- **AO compute and AI narrative is new and separate:** the AO hyper parallel compute layer only reached mainnet in early 2025, is a separate token and economy, and its on chain AI and agent positioning is largely forward looking. AR holders capture AO only via a distribution stream, not as core protocol revenue, and AR's value driver remains storage.
- **Value:** the token is down roughly 98 percent from its 2021 peak.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| AR 001 | **MEDIUM** | The "store forever" endowment depends on storage costs continuing to fall over very long horizons, an unproven multi century assumption. |
| AR 002 | **LOW** | Modest real usage versus the permanent hard drive framing; storage replication concentrates among larger miners. |
| AR 003 | **LOW** | The AO compute and AI narrative is new, unproven and a separate token and economy; AR value remains storage driven. |
| AR 004 | **LOW** | ~98 percent drawdown from the 2021 peak. |
| AR 005 | **INFO** | Clean token mechanics: hard cap of 66 million, ~99.5 percent circulating, doxxed team, effectively fixed supply (positives). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Established, doxxed team, verified |
| Supply / minting | Low risk | Hard cap, ~99.5 percent circulating |
| Endowment economics | Medium risk | Store forever assumption unproven long term |
| Usage / demand | Medium risk | Modest usage; miner concentration |
| AI framing | Medium risk | AO compute new, separate token, unproven |
| Value / volatility | High risk | ~98 percent drawdown from peak |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Token | AR (native, Arweave Layer 1) |
| Decimals | 12 (winston) |
| Max supply | 66,000,000 AR (hard cap) |
| Circulating | ~65.65 million (~99.5 percent) |
| Consensus | Succinct proofs of random access (evolved from the original proof of access) |

---

## 8. Conclusion

Arweave is a real, long running decentralized storage Layer 1 with a doxxed team, a near fully circulating and effectively fixed 66 million supply, and genuine usage, which keeps rug and dilution risk low and places it at 74/100. It is held back because the "store forever" endowment depends on the unproven assumption that storage costs keep falling over very long horizons, because real usage is modest and replication concentrates among larger miners, and because the AO compute and AI narrative is new, unproven and a separate token, against a roughly 98 percent drawdown. The token mechanics are clean; the caution is the untested endowment economics and an early AI compute narrative.

---

## 9. Recommendations

**For the Arweave team:**
- Publish transparent endowment health metrics and the assumptions behind the perpetual storage guarantee.
- Report real stored data and paid usage clearly, and address miner concentration.
- Distinguish the mature storage business from the new and separate AO compute and AI layer in messaging.

**For users:**
- Treat AR as a clean, near fully circulating fixed supply token, and understand the "store forever" guarantee is a well designed but unproven long horizon model.
- Note the AO AI narrative is early and a separate token, and model the drawdown.

---

## 10. Verification

- MEFAI on chain analysis: review of AR's hard cap of 66 million, ~99.5 percent circulating supply, 12 decimal winston base unit, the effectively fixed tail emission and the succinct proofs of random access consensus model.
- The supply, cap and emission are publicly verifiable on the Arweave explorers.
- Project statements: the project's website and materials (the "pay once, store forever" and permaweb framing, the endowment model, and the AO compute and AI positioning).
