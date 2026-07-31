# Security Audit Report: Polygon (POL) on Ethereum / Polygon PoS

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Polygon |
| **Token Symbol** | POL (formerly MATIC) |
| **Contract (Ethereum)** | `0x455e53CBB86018Ac2B8092FdCd39d8444aFFC3F6` |
| **Chain** | Ethereum (canonical POL) / Polygon PoS (native gas) |
| **Audit Type** | Network Native Token (Claim vs Reality) |
| **Mefai Security Score** | **73/100** |
| **Overall Risk** | **LOW to MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Polygon is a real, deeply adopted scaling ecosystem with a genuine token, deep liquidity and one of the largest user bases in the industry. It is a blue chip network, not a scam. The LOW to MEDIUM rating reflects two structural realities that sit in tension with the "secures the network / broad decentralization" branding:

1. **Core control rests with a small governance council, not permissionless governance.** MEFAI's review confirms that upgrade authority over the core proof of stake staking contract and the bridge/checkpoint contract on Ethereum is held by the **Polygon Protocol Council**, a 13 member multisig: a 7 of 13 route with a 10 day timelock for standard upgrades, and a 10 of 13 emergency route with no timelock. This governance layer has shipped (it replaced the legacy 5 of 9 multisig, which some un updated documentation pages still list), but it is still a bounded multisig rather than permissionless on chain governance, so the centralization caution stands.
2. **The validator set is small and slot limited.** Active validators are capped at roughly 105 slots, orders of magnitude fewer than Ethereum's base layer.
3. **POL is uncapped and perpetually inflationary** (~2 percent per year: 1 percent to validators, 1 percent to a community treasury), and the emission model is itself under active community debate.

The MATIC to POL migration is genuine and roughly 99 percent complete, and real usage is high. The caution is control centralization and uncapped inflation, not legitimacy.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name / symbol** | Polygon Ecosystem Token / POL |
| **Contract (Ethereum)** | `0x455e53CBB86018Ac2B8092FdCd39d8444aFFC3F6` |
| **Decimals** | 18 |
| **Total supply (verified)** | ~10,687,978,414 POL |
| **Supply model** | Uncapped, perpetual ~2 percent annual emission (1 percent validators + 1 percent treasury) |
| **Predecessor** | MATIC (migrated 1:1 to POL, ~99 percent complete) |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's on chain read of the canonical POL contract on Ethereum confirmed:

| Check | Result |
|-------|--------|
| Token identity | "Polygon Ecosystem Token", symbol POL, verified |
| Decimals | 18 |
| Total supply | ~10,687,978,414 POL (above the original 10 billion) |
| Supply model | Uncapped, ~2 percent annual emission |
| Core contract control | Polygon Protocol Council, 13 member multisig (7 of 13 with a 10 day timelock; 10 of 13 emergency, no timelock) |
| Validator slots | Capped at ~105 |

**Interpretation.** The token is genuine and deeply liquid. The two structural facts a holder must weigh are that supply is **not fixed** (perpetual emission) and that upgrade authority over the core Ethereum staking and bridge contracts rests with a **bounded governance council**, not yet permissionless governance.

---

## 3. Claim vs Reality: "Secures the Network" / Decentralization

> Site: "$POL powers Polygon as the native gas and staking token that secures the network... A token with real utility." A Polygon blog states "Polygon has a broad group of validators that helps avoid collusion."

**Reality: real staking, bounded and council gated control.** POL is genuinely the gas and staking token, and there is a real validator set. But:
- Active validators are **capped at roughly 105 slots**, a small, limited slot set (Ethereum's base layer has hundreds of thousands of validators).
- The **Polygon Protocol Council** (a 13 member multisig) holds upgrade authority over the core staking and bridge contracts. Standard upgrades take a 7 of 13 vote with a 10 day timelock; a 10 of 13 emergency vote can act with no timelock. This shipped governance is an improvement over the legacy 5 of 9 multisig, but it is still a bounded set, not permissionless on chain governance.

"Broad, collusion resistant decentralization" overstates a small, slot capped validator set sitting under a bounded council.

---

## 4. Claim vs Reality: Speed and the AggLayer

> Site: "The fastest settlement layer to move money globally"; "5,000 Transactions per second"; "$0.002 Average transaction cost"; "settles instantly."

**Reality: real performance, partly forward looking.** Polygon PoS is genuinely fast and cheap in everyday use. But the headline throughput and "fastest settlement layer" framing map to **peak or target capabilities and the Polygon 2.0 / AggLayer architecture** rather than sustained mainnet load. The AggLayer's documented guarantee is primarily **cryptographic containment** (a compromised connected chain "cannot drain more than its own deposits"), and the interoperability documentation does not itself substantiate how POL secures the AggLayer. The engineering is real and shipping; the unified cross chain vision is still maturing.

---

## 5. Claim vs Reality: POL "Real Utility" vs Uncapped Inflation

POL is marketed as a utility token, but it is **uncapped and perpetually inflationary** at roughly 2 percent per year (1 percent to validator rewards, 1 percent to a self sustaining community treasury), scheduled for at least ten years. During activity spikes, fee burns can episodically exceed emissions, but this is not structural deflation. Notably, the emission model is **actively contested**: a community proposal to eliminate the 2 percent inflation in favor of treasury buyback or burn is under discussion. The token is presented as settled utility while its monetary policy is under live governance debate.

---

## 6. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| POL 001 | **MEDIUM** | Core staking and bridge contracts are controlled by the 13 member Polygon Protocol Council (7 of 13 with a 10 day timelock; 10 of 13 emergency, no timelock), not permissionless governance. |
| POL 002 | **MEDIUM** | Uncapped, perpetual ~2 percent inflation; emission model under active community debate. |
| POL 003 | **LOW** | Validator set capped at ~105 slots. |
| POL 004 | **LOW** | "Fastest settlement layer / 5,000 TPS" maps to peak or target, not sustained load; AggLayer vision partly forward looking. |
| POL 005 | **INFO** | MATIC to POL 1:1 migration genuine and ~99 percent complete (positive). |
| POL 006 | **INFO** | Deep liquidity, huge real usage, verified token (positive; blue chip). |

---

## 7. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token legitimacy | Low risk | Verified, deeply liquid, real usage |
| Supply / minting | Medium risk | Uncapped, perpetual inflation |
| Governance / upgrade control | Medium risk | 13 member Protocol Council over core contracts |
| Decentralization | Medium risk | ~105 validator slots |
| Roadmap delivery | Low to medium risk | AggLayer partly forward looking |
| Transparency | Low risk | Council and tokenomics documented |

---

## 8. Technical Specifications

| Item | Value |
|------|-------|
| Contract (Ethereum) | `0x455e53CBB86018Ac2B8092FdCd39d8444aFFC3F6` |
| Decimals | 18 |
| Total supply | ~10,687,978,414 POL |
| Supply model | Uncapped, ~2 percent/year (1 percent validators + 1 percent treasury) |
| Core contract control | Polygon Protocol Council (13 members; 7 of 13 + 10 day timelock; 10 of 13 emergency) |
| Validator slots | ~105 (capped) |

---

## 9. Conclusion

Polygon is a legitimate blue chip scaling ecosystem with a verified, deeply liquid token and one of the largest real user bases in crypto. It scores 73/100 (LOW to MEDIUM) because upgrade authority over the core Ethereum contracts rests with a bounded 13 member governance council (not permissionless governance), the validator set is a small slot capped set, and POL is uncapped and perpetually inflationary with a contested emission model. The MATIC to POL migration is genuine and the network is real; the caution is control centralization and uncapped inflation, not legitimacy.

---

## 10. Recommendations

**For the Polygon team:**
- Continue decentralizing beyond the Protocol Council toward permissionless on chain governance, and keep the council membership and powers clearly documented (retiring stale pages that still list the legacy 5 of 9 multisig).
- Present POL's uncapped inflation and the live emission reform debate transparently on consumer pages.
- Clearly separate shipped throughput from peak or target and AggLayer forward looking claims.

**For users:**
- Understand POL is uncapped and inflationary, and that a bounded council can upgrade core contracts today.
- Treat Polygon as a real, high usage network whose control layer is still decentralizing.

---

## 11. Verification

- MEFAI on chain analysis: a direct read of the canonical POL contract on Ethereum (identity, 18 decimals, total supply ~10.69 billion, uncapped emission model), cross checked against the project's documented Polygon Protocol Council control (13 members; 7 of 13 with a 10 day timelock; 10 of 13 emergency) and ~105 validator slot cap.
- The contract address, supply, council and validator set are publicly verifiable on the Ethereum and Polygon explorers and the project's own governance documentation.
- Project statements: the project's homepage ("fastest settlement layer", "5,000 TPS", "$POL... secures the network... real utility"), its interoperability/AggLayer documentation, its Polygon 2.0 tokenomics blog, and the community forum debate on POL emissions.
