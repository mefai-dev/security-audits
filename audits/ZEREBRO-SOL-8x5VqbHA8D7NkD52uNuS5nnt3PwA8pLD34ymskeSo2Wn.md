# Security Audit Report: Zerebro (ZEREBRO) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Zerebro |
| **Token Symbol** | ZEREBRO |
| **Mint (Solana)** | `8x5VqbHA8D7NkD52uNuS5nnt3PwA8pLD34ymskeSo2Wn` |
| **Chain** | Solana |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **44/100** |
| **Overall Risk** | **HIGH** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion, and references to reported events are described as reported or alleged, not as findings of fact about any individual. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Zerebro is a Solana AI agent memecoin with clean token contract mechanics but a large gap between its "autonomous AI" story and a human operated reality, compounded by serious market integrity and conduct flags. It is rated HIGH risk:

1. **The token contract is clean:** MEFAI's on chain read confirms mint and freeze authorities are revoked and supply is near fully circulating with no locked pre mine.
2. **But the "autonomous AI operating without human oversight" claim does not hold.** The project's own open source framework is a conventional large language model wrapper that requires human configuration and third party model keys, and the supposedly autonomous account has behaved in ways incoherent with genuine autonomy.
3. **Serious integrity flags:** a suspected market manipulation episode driven by exchange funded wallets, exchange delistings, thin liquidity, a large share of supply in a single likely exchange wallet (on chain reads range from ~11 percent to ~51 percent), and a reported, widely suspected staged death event around the founder that coincided with large wallet movements.
4. **The token is down roughly 95 to 96 percent from its peak.**

The contract is clean, but the autonomy claim is a fabrication and the surrounding conduct and market flags make this HIGH risk.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Mint (Solana)** | `8x5VqbHA8D7NkD52uNuS5nnt3PwA8pLD34ymskeSo2Wn` |
| **Decimals** | 6 |
| **Supply** | ~999.95 million (of a fixed ~1 billion), near fully circulating, no locked pre mine |
| **Mint authority** | Revoked |
| **Freeze authority** | Revoked |
| **Concentration** | Largest single account holds a large share (on chain reads diverge, ~11 percent to ~51 percent); almost certainly a centralized exchange custody wallet |
| **Liquidity** | Main pool ~2.6 million dollars (~99 percent LP locked) |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana read of the ZEREBRO mint returned a clean contract profile:

| Check | Result |
|-------|--------|
| Mint identity | ZEREBRO (Solana SPL), 6 decimals, verified |
| Supply | ~999.95 million, near fully circulating (no locked pre mine announced) |
| Mint authority | **Revoked** (no dilution possible) |
| Freeze authority | **Revoked** (accounts cannot be frozen) |
| Holders | Tens of thousands |
| Concentration | Largest single account a large share (~11 percent to ~51 percent across reads); likely a centralized exchange custody wallet |
| Liquidity | ~2.6 million dollars (main pool, ~99 percent LP locked) |

**Interpretation.** On pure token mechanics, ZEREBRO is clean: revoked authorities, no locked pre mine, broad holder count. The real fragility is elsewhere: a large single account concentration (a likely exchange custody wallet), thin liquidity, and the conduct and market integrity issues below.

---

## 3. Claim vs Reality: "Autonomous AI Without Human Oversight"

> Framing: "an autonomous AI system designed to independently create, distribute, and analyze content" that "operates without human oversight", an "AI native crypto protocol" using "autonomous agents to generate viral content", on a path toward advanced AI.

**Reality: human operated / human assisted, not a self directing AI.** The project's own open source framework is a **conventional large language model wrapper** (posting, liking and retweeting via third party model keys) that requires human setup and configuration, not an autonomous mind. The clearest tell: the supposedly autonomous account has behaved in ways incoherent with genuine autonomy (including publicly attacking its own creator). Independent commentary concluded the evidence points to direct human operation despite the autonomous claims. The "AI artist on the path to advanced AI" framing is marketing over a fine tuned model plus a posting bot.

---

## 4. Claim vs Reality: Market Integrity and Conduct

- **Suspected market manipulation:** a sharp price spike was reportedly driven by exchange funded wallets opening leveraged long positions on a perpetuals venue, then quickly reversed, a pattern consistent with a pump and reverse rather than organic demand.
- **Exchange delistings:** the token has been delisted from at least one derivatives venue and one spot venue, cited to low liquidity and waning interest.
- **Reported founder event:** there are widely reported, widely suspected claims of a staged death event involving the project's founder that coincided with large movements of funds from associated wallets. MEFAI describes this as reported and alleged, not as established fact; it is included because, if the suspicions are correct, it represents a severe market integrity and holder trust concern.
- **Concentration and drawdown:** a large share of supply sits in a single likely exchange custody wallet (on chain reads range widely), liquidity is thin, and the token is down roughly 95 to 96 percent from its peak.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| ZEREBRO 001 | **HIGH** | "Autonomous AI without human oversight" is contradicted: the framework is a human configured language model wrapper, and the account has behaved incoherently with autonomy. |
| ZEREBRO 002 | **HIGH** | Market integrity and conduct flags: suspected pump and reverse via exchange funded wallets; exchange delistings; a reported, suspected staged death event coinciding with large wallet movements. |
| ZEREBRO 003 | **MEDIUM** | Large share of supply in a single likely exchange custody wallet (reads range ~11 to ~51 percent); thin liquidity (~2.6 million dollars, ~99 percent LP locked); ~95 to 96 percent drawdown from peak. |
| ZEREBRO 004 | **INFO** | Clean token contract: mint and freeze authorities revoked, near fully circulating, no locked pre mine (positive on mechanics only). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token mechanics | Low risk | Mint and freeze revoked, near fully circulating |
| Autonomy claim | High risk | Human operated; autonomy is a fabrication |
| Market integrity | High risk | Suspected manipulation, delistings, reported staged death event |
| Concentration | Medium to high risk | Large share in one likely exchange wallet (reads vary), thin liquidity |
| Value / volatility | High risk | ~95 to 96 percent drawdown from peak |
| Transparency | High risk | Central claim (autonomy) is untrue |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `8x5VqbHA8D7NkD52uNuS5nnt3PwA8pLD34ymskeSo2Wn` |
| Decimals | 6 |
| Supply | ~999.95 million (near fully circulating) |
| Mint / freeze authority | Revoked / Revoked |
| Concentration | Large share in one account (~11 to ~51 percent across reads) |
| Liquidity | ~2.6 million dollars (main pool, ~99 percent LP locked) |

---

## 8. Conclusion

Zerebro has a genuinely clean token contract (revoked mint and freeze authorities, near fully circulating, no locked pre mine), which is the only reason it is not lower. It scores 44/100 (HIGH risk) because its central claim, an "autonomous AI operating without human oversight", is contradicted by a human configured language model wrapper and an account that has behaved incoherently with autonomy, and because of serious market integrity and conduct flags: a suspected pump and reverse via exchange funded wallets, exchange delistings, a large single account concentration (a likely exchange custody wallet, on chain reads range widely), thin liquidity, a reported and widely suspected staged death event coinciding with large wallet movements, and a roughly 95 to 96 percent drawdown. The contract is clean; the autonomy claim is untrue and the surrounding conduct makes this HIGH risk.

---

## 9. Recommendations

**For any Zerebro promoters:**
- Stop describing the project as an autonomous AI operating without human oversight; it is a human configured language model wrapper.
- Address the market integrity concerns (suspected manipulation, the reported founder event, concentration) transparently.

**For users:**
- Treat the "autonomous AI" narrative as false and the token as a high risk memecoin with serious conduct and market integrity flags.
- Note the single account concentration (a likely exchange custody wallet, reads vary), thin liquidity and ~95 to 96 percent drawdown; the clean contract does not offset these risks.

---

## 10. Verification

- MEFAI on chain analysis: a direct Solana read of the ZEREBRO mint (identity, 6 decimals, ~999.95 million near fully circulating, mint and freeze authorities revoked, holder concentration) and the main pool liquidity.
- The mint address, supply and authorities are publicly verifiable by anyone on the Solana explorers.
- Project statements and public record: the project's own "autonomous / operates without human oversight" framing, its open source language model framework, and the public reporting of the market manipulation suspicion, exchange delistings and the reported founder event (described here as reported and alleged).
