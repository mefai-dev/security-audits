# Security Audit Report: Toncoin (TON) on The Open Network

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | The Open Network (TON) |
| **Token Symbol** | TON (Toncoin) |
| **Native token** | Toncoin (The Open Network, native, non EVM) |
| **Chain** | The Open Network (TON) |
| **Audit Type** | Network Native Token (Claim vs Reality) |
| **Mefai Security Score** | **54/100** |
| **Overall Risk** | **MEDIUM to HIGH** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

TON is a real, technically capable network with a genuine mass distribution advantage through its Telegram integration. It is not a scam. But it is the lowest scoring network in this set (54/100, MEDIUM to HIGH) because two structural facts sit in sharp tension with its "decentralized" branding:

1. **Extreme early supply concentration.** A single, publicly reproducible independent on chain analysis indicates a related cluster of wallets mined roughly **85.8 percent of TON supply** during a brief ~51 day proof of work window in mid 2020 (about 96 percent of supply distributed via only ~248 Large Giver contracts), and reporting places **over 68 percent of supply in whale wallets**. For a proof of stake network where stake equals security and voting power, this is a material centralization of both economics and consensus.
2. **Deep, deepening Telegram dependency.** TON was built by Telegram, abandoned after a 2020 SEC settlement, and revived by a foundation; it is now Telegram's sole official crypto rail (the official wallet and payments rail), so its usage, price and even a major messenger's balance sheet are structurally tied together, the opposite of a "no single point of control" network.

The technology and reach are real; the caution is supply concentration, Telegram dependency and marketing that outruns on chain demand.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token** | Toncoin (TON) |
| **Total supply** | ~5.2 billion TON (uncapped, inflationary PoS) |
| **Circulating** | ~2.7 billion (roughly half) |
| **Initial distribution** | Proof of work "mining" (now ended) |
| **Validator stake minimum** | ~300,000 TON protocol minimum (~400,000 practical) |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI reviewed TON's supply, distribution history and consensus parameters:

| Check | Result |
|-------|--------|
| Total supply | ~5.2 billion TON (uncapped, low PoS issuance ~0.5 to 2 percent) |
| Circulating | ~2.7 billion (roughly half) |
| Early distribution | ~85.8 percent mined by a related wallet cluster in a ~51 day 2020 window; ~96 percent via ~248 Large Giver contracts |
| Whale concentration | Over 68 percent of supply in whale wallets (per independent analysis) |
| Consensus | Proof of stake; validators lock ~300,000+ TON |
| Burns | ~50 percent of fees plus slashed stake burned (partial offset) |

**Interpretation.** TON is a genuine, liquid asset with low ongoing issuance, but its **initial distribution was extremely concentrated**, and that concentration persists. In a proof of stake system this concentrates both economic upside and consensus/governance weight in a small early cohort.

---

## 3. Claim vs Reality: "Decentralized" vs Supply Concentration

> Site: "Blockchain changes are only possible when approved by the majority of validators through Proof of Stake consensus" (shown alongside "395 Validators / 977 Nodes"). The coin page notes Toncoin was "initially distributed via Proof of Work mining."

**Reality: fair sounding distribution, highly concentrated in practice.** "Distributed via proof of work mining" reads as open and fair, but the on chain record shows a **brief, tightly concentrated distribution**: a related cluster of wallets acquired the overwhelming majority of supply in weeks, to a few hundred addresses. Because voting power tracks stake, "changes require a majority of validators" does not neutralize the fact that a small early group holds outsized economic and voting weight. "Community run decentralization" overstates this reality.

---

## 4. Claim vs Reality: "Telegram Scale" and Independence

> Site: "Telegram integration gives blockchain technology unprecedented reach among its 1B+ active users"; "a blockchain ecosystem built into Telegram... for 1B+ users."

**Reality: real reach, real dependency.** The Telegram funnel is a genuine, unique distribution advantage. But TON is marketed as a standalone decentralized network merely "integrated" with Telegram, while in practice it is Telegram's **sole official crypto infrastructure** (the official wallet and payments rail), and its usage, price and a major messenger's finances move together. If Telegram faces regulatory action or changes course, TON is directly exposed, a **single point dependency**, not the decentralized resilience the branding implies. The "1B+ users" reach is a Telegram figure; independent active on chain wallets are a small fraction of it.

---

## 5. Claim vs Reality: Throughput and History

- **Throughput:** the site cites "100K+ TPS (public test)" and "process up to millions of transactions per second," presented next to real mainnet numbers; observed sustained mainnet load is far lower (on the order of tens of TPS on average). The peak capacity marketing far exceeds demonstrated demand.
- **History:** TON was built by Telegram and funded by a ~1.7 billion dollar 2018 token sale; the **SEC sued in 2019**, Telegram **abandoned the project in May 2020** and settled (an 18.5 million dollar penalty plus roughly 1.22 billion dollars returned to investors). A foundation revived it. This regulatory history is material context that the marketing omits.

---

## 6. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| TON 001 | **HIGH** | Extreme early supply concentration: ~85.8 percent mined by a related cluster in a ~51 day 2020 window; over 68 percent of supply in whale wallets. In PoS this concentrates security and voting power. |
| TON 002 | **MEDIUM** | Deep, deepening Telegram dependency (sole official crypto rail); usage/price/messenger finances structurally linked; single point risk. |
| TON 003 | **MEDIUM** | Throughput marketing ("millions of TPS") far exceeds observed sustained mainnet load. |
| TON 004 | **LOW** | Regulatory history (2019 SEC suit, 2020 abandonment/settlement) omitted from marketing. |
| TON 005 | **INFO** | Uncapped supply with low PoS issuance (~0.5 to 2 percent) and partial fee burns. |
| TON 006 | **INFO** | Genuine mass distribution reach via Telegram and real, capable technology (positive). |

---

## 7. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Real, liquid, capable network |
| Supply / concentration | High risk | ~85.8 percent early mined by a related cluster; >68 percent whale held |
| Decentralization | Medium to high risk | Stake concentration + Telegram dependency |
| Throughput claims | Medium risk | "Millions of TPS" vs tens of TPS observed |
| Dependency / regulatory | Medium risk | Sole Telegram rail; SEC history |
| Transparency | Medium risk | Distribution and history under disclosed in marketing |

---

## 8. Technical Specifications

| Item | Value |
|------|-------|
| Token | Toncoin (TON) |
| Total supply | ~5.2 billion (uncapped) |
| Circulating | ~2.7 billion |
| Early distribution | ~85.8 percent mined by a related cluster (~248 Large Giver contracts, 2020) |
| Consensus | Proof of stake; validator minimum ~300,000 TON |
| Issuance / burn | ~0.5 to 2 percent issuance; ~50 percent of fees burned |

---

## 9. Conclusion

TON is a real, capable network with a genuine and unique Telegram distribution advantage, which keeps it out of the critical band. But it is the lowest scoring network in this set at 54/100 (MEDIUM to HIGH) because its supply is extremely concentrated (a related cluster mined roughly 85.8 percent in a brief 2020 window, and over 68 percent sits in whale wallets), it is deeply and increasingly dependent on Telegram as its sole crypto rail, and its throughput marketing far outruns observed demand. The technology and reach are real; the caution is concentration, dependency and overstated claims, not fraud.

---

## 10. Recommendations

**For the TON community:**
- Publish transparent, on chain supply concentration data and address the early mining concentration directly rather than framing distribution simply as "proof of work mining."
- Present throughput as tested capacity versus sustained load, and disclose the Telegram dependency and regulatory history as material context.

**For users:**
- Understand that supply and, therefore, staking/voting power are concentrated in a small early cohort, and that TON's fortunes are tied to Telegram.
- Treat "millions of TPS" as tested capacity, not demonstrated on chain demand.

---

## 11. Verification

- MEFAI on chain analysis: review of TON's supply (~5.2 billion, uncapped), the documented early proof of work distribution (~85.8 percent to a related cluster; ~96 percent via ~248 Large Giver contracts in 2020), whale concentration analysis (over 68 percent), and consensus parameters (validator stake minimum, fee burn).
- The supply, distribution and validator set are publicly verifiable on the TON explorers.
- Project statements: the TON website and coin page (the "1B+ users", validator consensus and "proof of work mining" wording, and the throughput figures), and the public record of the 2019 to 2020 Telegram / SEC matter.
