# Security Audit Report: Griffain (GRIFFAIN) on Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | July 31, 2026 |
| **Project** | Griffain |
| **Token Symbol** | GRIFFAIN |
| **Mint (Solana)** | `KENJSUYLASHUMfHyy5o4Hp2FdNqZg1AsUPhfH2kYvEP` |
| **Chain** | Solana |
| **Audit Type** | Project + Token (Claim vs Reality) |
| **Mefai Security Score** | **56/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's on chain analysis. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Griffain is a real, working Solana AI agent chat product with a clean mechanics token. It is not a rug at the contract level. The MEDIUM rating reflects a large gap between the "autonomous agent" branding and a human gated tool, plus pseudonymity, false pedigree claims and severe price collapse:

1. **The token mechanics are clean:** MEFAI's on chain read confirms mint and freeze authorities are both revoked, supply is fully circulating, and holder distribution is broad.
2. **But the "autonomous on chain agents" claim is overstated.** In reality Griffain is a hosted chat product whose agents use an embedded wallet managed by a third party provider (Privy). The wallet is locked by default; the user performs a one time unlock that grants the agents ongoing session authority to sign and send on their behalf until the user re locks it. It is user delegated session signing behind an opt in gate, not an independent key holding swarm.
3. **Pseudonymous team, no disclosed backers, no public audit**, and third party claims of prestigious backing (a major venture firm, or a chain's core team) are unsubstantiated.
4. **Severe drawdown:** the token is down roughly 99 percent from its peak, with thin on chain liquidity.

The product is real and the contract is clean; the caution is overstated autonomy, pseudonymity, false pedigree and extreme drawdown.

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Mint (Solana)** | `KENJSUYLASHUMfHyy5o4Hp2FdNqZg1AsUPhfH2kYvEP` |
| **Decimals** | 6 |
| **Supply** | ~999.85 million (of a fixed ~1 billion), effectively fully circulating |
| **Mint authority** | Revoked |
| **Freeze authority** | Revoked |
| **Liquidity** | Main pool on the order of ~1.1 million dollars (thin) |

---

## 2. On chain Security Assessment (MEFAI analysis)

MEFAI's direct Solana read of the GRIFFAIN mint returned a clean profile:

| Check | Result |
|-------|--------|
| Mint identity | GRIFFAIN (Solana SPL); on chain metadata name is a leftover "test" label |
| Decimals | 6 |
| Supply | ~999.85 million, effectively fully circulating (no locked pre mine) |
| Mint authority | **Revoked** (no dilution possible) |
| Freeze authority | **Revoked** (accounts cannot be frozen) |
| Holders | Broad distribution (tens of thousands), no single dominant whale in the reads |
| Liquidity | Thin main pool (~1.1 million dollars) relative to peak valuation |

**Interpretation.** On pure token mechanics, GRIFFAIN is clean: revoked mint and freeze authorities, no locked pre mine, broad distribution. The risks are not in the contract; they are in the product claims, the team and the market.

---

## 3. Claim vs Reality: "Autonomous On chain Agents"

> Site: "Griffain powers the Agent Engine, a network of agents to help you take action on the blockchain"; "Wallets give agents the ability to turn intent into action"; "the agent of agents architecture allows a network of specialized agents to work together"; third party framing cites "over 1 million automated transactions."

**Reality: user delegated session signing, not independent autonomy.** Griffain is a hosted chat product. Each account gets an **embedded wallet managed by a third party wallet provider (Privy)**. The wallet is **locked by default**; the user performs a **one time unlock** (accepting Griffain's request to sign and send transactions on the user's behalf), which grants the agents **ongoing session authority** to sign and send without approving each individual transaction, until the user re locks the wallet to revoke access. So once unlocked the agents act on the user's behalf within that session, but the user must opt in first and can revoke at any time. Practically it is **conversational middleware that translates natural language into Solana transactions** behind a user granted session gate, not an autonomous swarm holding its own independent keys. The "agent of agents" swarm is a fixed set of pre built agents routing prompts to canned action sets, and the "1 million plus transactions" figure is self reported and unaudited.

---

## 4. Claim vs Reality: Pedigree and Value

- **Pseudonymous team, no disclosed backers, no public security audit.** Griffain drew attention in the Solana AI agent era, but third party claims that it is "developed by a chain's core team" or "backed by a major venture firm" are **unsubstantiated** by primary sources.
- **Severe drawdown:** the token peaked near a few hundred million dollar valuation and is now roughly **99 percent below its all time high**, with thin main pool liquidity, token value tracked the AI agent hype, not proven product economics.

---

## 5. Findings by Severity

| ID | Severity | Finding |
|----|----------|---------|
| GRIFFAIN 001 | **MEDIUM** | "Autonomous on chain agents" overstated: hosted product, third party embedded wallet (Privy), default locked with a one time user unlock granting ongoing session signing (user delegated, not independent autonomy). |
| GRIFFAIN 002 | **MEDIUM** | Pseudonymous team, no disclosed backers, no public audit; third party claims of prestigious backing are unsubstantiated. |
| GRIFFAIN 003 | **LOW** | ~99 percent drawdown from peak; thin main pool liquidity (~1.1 million dollars); self reported usage figures. |
| GRIFFAIN 004 | **INFO** | Clean token mechanics: mint and freeze authorities revoked, fully circulating, broad distribution (positive). |
| GRIFFAIN 005 | **INFO** | Real, working chat product exists (context). |

---

## 6. Risk Matrix

| Dimension | Rating | Basis |
|-----------|--------|-------|
| Token mechanics | Low risk | Mint and freeze revoked, broad distribution |
| Autonomy claim | Medium risk | User delegated session signing (opt in unlock), not independent autonomy |
| Team / pedigree | Medium risk | Pseudonymous, no backers, false third party pedigree |
| Liquidity / volatility | Medium to high risk | Thin liquidity, ~99 percent drawdown |
| Product reality | Low to medium risk | Real hosted tool, self reported usage |
| Transparency | Medium risk | Autonomy and pedigree overstated |

---

## 7. Technical Specifications

| Item | Value |
|------|-------|
| Mint | `KENJSUYLASHUMfHyy5o4Hp2FdNqZg1AsUPhfH2kYvEP` |
| Decimals | 6 |
| Supply | ~999.85 million (fully circulating) |
| Mint / freeze authority | Revoked / Revoked |
| Liquidity | ~1.1 million dollars (main pool) |

---

## 8. Conclusion

Griffain is a real, working Solana AI agent chat product with genuinely clean token mechanics (revoked mint and freeze authorities, fully circulating, broad distribution), which keeps it in the MEDIUM band at 56/100. It is held back because its "autonomous on chain agents / agent of agents" branding overstates a hosted, user delegated tool (a hosted product with a third party embedded wallet (Privy) that is locked by default and, once unlocked, grants agents ongoing session signing), because the team is pseudonymous with no disclosed backers or audit and third party pedigree claims are unsubstantiated, and because the token is down roughly 99 percent from peak with thin liquidity. The contract is clean; the caution is overstated autonomy, pseudonymity and extreme drawdown.

---

## 9. Recommendations

**For the Griffain team:**
- Describe the wallet model accurately: a hosted embedded wallet that is locked by default and, once unlocked, grants agents ongoing session signing, and name the embedded wallet provider.
- Disclose the team and any backers, and correct third party claims of prestigious backing; publish a security audit.
- Publish verifiable usage data rather than self reported transaction counts.

**For users:**
- Understand agents cannot act until you unlock the wallet; a single unlock then grants ongoing session signing until you re lock it, so review permissions before unlocking.
- Note the thin liquidity and ~99 percent drawdown; the clean contract does not offset market and disclosure risk.

---

## 10. Verification

- MEFAI on chain analysis: a direct Solana read of the GRIFFAIN mint (identity, 6 decimals, ~999.85 million fully circulating, mint and freeze authorities revoked, holder distribution) and the main pool liquidity.
- The mint address, supply and authorities are publicly verifiable by anyone on the Solana explorers.
- Project statements: the project's own pages (the "Agent Engine", "turn intent into action", and "agent of agents" wording, and the embedded wallet unlock and approve documentation), and the public record of drawdown and the unsubstantiated pedigree claims.
