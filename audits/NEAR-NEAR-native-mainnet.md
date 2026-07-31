# Security Audit Report: NEAR Protocol (NEAR) on NEAR Mainnet

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | NEAR Protocol |
| **Token Symbol** | NEAR |
| **Native token** | NEAR (mainnet, non EVM); bridged ERC 20 `0x85F17Cf997934a597031b2E18a9aB6ebD4B9f6a4` |
| **Chain** | NEAR Protocol (native) |
| **Audit Type** | Network Native Token (Claim vs Reality) |
| **Mefai Security Score** | **70/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

NEAR is a real, technically credible sharded proof of stake network with low fees, genuine usage, and a recently shipped sharding milestone. It is a blue chip network. The MEDIUM rating is driven by a large headline throughput gap and a decentralization/AI marketing gap, offset by real progress on inflation and sharding:

1. **The "1 million TPS" headline is a theoretical target, not observed reality.** NEAR's homepage states it "supports 1 million TPS," but sustained mainnet throughput is orders of magnitude lower. This is the single largest claim vs reality gap.
2. **The recent "user owned AI" / "currency of agents" positioning overstates the on chain role.** NEAR's AI compute runs in centralized, off chain hardware enclaves (a NEAR AI cloud); the blockchain provides identity, settlement and payments, not the intelligence itself.
3. **Governance and holdings are less decentralized than branded.** The initial distribution sent about 36 percent to core team and backers versus roughly 12 percent via the community sale, validator seats are concentrated, and the core team advanced a protocol upgrade after a community governance vote failed to reach supermajority (validators themselves reached the required stake supermajority).

To its credit, NEAR shipped genuine stateless validation sharding, and cut gross inflation from 5 percent to 2.5 percent in late 2025. MEFAI verified a native supply of roughly 1.302 billion NEAR.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token** | NEAR (gas, staking, governance) |
| **Total supply (verified)** | ~1,302,305,440 NEAR (current supply, uncapped, nearly fully unlocked) |
| **Inflation** | Gross cut from 5 percent to 2.5 percent (late 2025); max protocol inflation ~2.5 percent |
| **Burns** | Fees burned, historically negligible relative to issuance |
| **Bridged ERC 20** | `0x85F17Cf997934a597031b2E18a9aB6ebD4B9f6a4` (small bridged portion) |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI read the native NEAR supply and inflation parameters directly from the network:

| Check | Result |
|-------|--------|
| Native total supply | ~1,302,305,440 NEAR, verified |
| Max inflation rate | ~2.5 percent (protocol parameter), cut from 5 percent in late 2025 |
| Burns | Negligible relative to issuance, so net inflation ran near the full gross rate for years |
| Validator seat floor | ~25,500 NEAR minimum (300th largest staking proposal) |
| Bridged ERC 20 supply | ~3.4 million NEAR (small) |

**Interpretation.** The token is genuine and nearly fully unlocked. Inflation is now genuinely capped low (2.5 percent), a real positive, but the historical "usage makes NEAR deflationary" thesis did not materialize because fee burns were negligible; deflation only became plausible after the 2025 inflation cut and a 2026 buyback mechanism.

---

## 3. Claim vs Reality: "1 Million TPS" and "Fully Sharded"

> Homepage: "NEAR protocol supports 1 million TPS, with 600ms blocks and 1.2s finality"; "NEAR Protocol is fully sharded, quantum adaptive blockchain infrastructure."

**Reality: sharding real, TPS headline aspirational.** NEAR genuinely shipped **Nightshade 2.0 with stateless validation**, where no single validator must track all shards, so "fully sharded" is now broadly accurate, a real milestone. But **"supports 1 million TPS" is a theoretical / linear scaling target**, presented in the present tense; sustained mainnet throughput is orders of magnitude lower. "Quantum adaptive" has no substantiation in the documentation. Finality (~1 to 2 seconds) is genuine.

---

## 4. Claim vs Reality: "User Owned AI" / "Currency of Agents"

> Marketing: "NEAR empowers users to own their assets and intelligence"; "the open infrastructure powering the agent economy"; "the currency of agents."

**Reality: NEAR is the payment/identity rail, not the AI.** NEAR's AI compute runs in **centralized, hardware backed trusted execution environments (a NEAR AI cloud), off chain**. The blockchain supplies identity, settlement and payments; it does not run decentralized on chain inference. "User owned AI" rests on enclave attestation and signatures, not on chain decentralized compute. The marketing implies AI runs *on* NEAR; in reality NEAR is the rail *around* off chain confidential compute.

---

## 5. Claim vs Reality: Decentralization

- The initial distribution sent roughly **36 percent to core team and backers** and only about **12 percent via the community sale**.
- Validator participation is concentrated (a low hundreds validator set, with governance historically limited to a few dozen effective validator voters), and the **core team advanced a protocol upgrade after a community governance (token holder) vote failed to reach supermajority**, while the validator node vote itself reached the required 80 percent stake supermajority, a move publicly criticized by validators.
- "House of Stake / decentralized, AI augmented governance" is real as a structure but coexists with this concentration.

---

## 6. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| NEAR 001 | **MEDIUM** | "Supports 1 million TPS" is a theoretical target; sustained mainnet throughput is orders of magnitude lower. |
| NEAR 002 | **MEDIUM** | "User owned AI" runs in centralized off chain enclaves; NEAR is the payment/identity rail, not on chain inference. |
| NEAR 003 | **LOW** | Insider heavy initial allocation (~36 percent team/backers vs ~12 percent via the community sale); validator/governance concentration; core team advanced an upgrade after a community governance vote failed to reach supermajority (validators themselves approved it). |
| NEAR 004 | **LOW** | "Deflationary trajectory" only became plausible after the 2025 inflation cut and 2026 buyback; burns were negligible historically. |
| NEAR 005 | **INFO** | Stateless validation sharding (Nightshade 2.0) genuinely shipped; inflation capped at 2.5 percent (positives). |
| NEAR 006 | **INFO** | Verified ~1.302 billion supply, near fully unlocked; real usage and low fees (positive). |

---

## 7. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, liquid, near fully unlocked |
| Supply / minting | Low risk | Inflation capped at 2.5 percent |
| Throughput claims | Medium risk | "1 million TPS" is theoretical, present tense |
| AI positioning | Medium risk | Off chain enclaves marketed as "user owned AI" |
| Decentralization | Low to medium risk | Insider allocation, validator concentration, vote override |
| Transparency | Low to medium risk | Sharding real; AI and TPS framing overstated |

---

## 8. Technical Specifications

| Item | Value |
|------|-------|
| Native total supply | ~1,302,305,440 NEAR |
| Max inflation | ~2.5 percent (cut from 5 percent, late 2025) |
| Sharding | Nightshade 2.0, stateless validation (shipped) |
| Validator seat floor | ~25,500 NEAR |
| Bridged ERC 20 | `0x85F17Cf997934a597031b2E18a9aB6ebD4B9f6a4` (~3.4M) |

---

## 9. Conclusion

NEAR is a real, technically credible sharded network with genuine usage, low fees, a shipped stateless validation sharding milestone, and a recently tightened 2.5 percent inflation cap, which keeps it in the MEDIUM band at 70/100. It is held back by a headline "1 million TPS" figure that is a theoretical target rather than observed throughput, a "user owned AI" narrative that describes off chain enclave compute rather than on chain inference, and an insider heavy allocation with validator concentration. The technology is real; the caution is the throughput and AI marketing versus the on chain reality.

---

## 10. Recommendations

**For the NEAR team:**
- Present "1 million TPS" as a theoretical maximum and publish sustained mainnet throughput alongside it.
- Clearly distinguish off chain enclave AI compute from on chain decentralization in the "user owned AI" messaging.
- Continue broadening validator participation and governance beyond the current concentration.

**For users:**
- Treat the TPS headline as capacity, not demonstrated load, and "user owned AI" as an off chain compute plus on chain payment model.
- Note the inflation cap and shipped sharding as genuine positives.

---

## 11. Verification

- MEFAI on chain analysis: a direct read of the native NEAR total supply (~1.302 billion) and the protocol inflation parameter (~2.5 percent) from the network, plus a read of the bridged ERC 20 representation (~3.4 million).
- The supply, inflation and validator set are publicly verifiable on the NEAR explorers and on chain parameters.
- Project statements: the NEAR homepage ("supports 1 million TPS", "fully sharded, quantum adaptive"), the AI positioning ("user owned AI", "currency of agents"), and the documented Nightshade 2.0 sharding and 2025 inflation cut.
