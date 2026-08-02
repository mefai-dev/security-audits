# Security Audit Report: Covalent (CXT) on Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | August 2, 2026 |
| **Project** | Covalent |
| **Token Symbol** | CXT |
| **Contract / Program** | `0x7ABc8A5768E6bE61A6c693a6e4EAcb5B60602C4D` |
| **Chain** | Ethereum |
| **Audit Type** | Project + Token (Claim versus Reality) |
| **Methodology** | Manual Review + Onchain Analysis + Website Frontend Review |
| **Mefai Security Score** | **65/100** |
| **Overall Risk** | **MEDIUM** |
| **Verdict** | **Passed** |
| **Classification** | Public |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research, based on public information, the project's own published statements, and onchain data verified through MEFAI's onchain analysis with read only public RPC. The assessments are Mefai Security Research's analysis and opinion. Onchain data can change. This report is not investment advice. Mefai Security Research assumes no liability for losses arising from reliance on this report. The project is welcome to respond, and documented corrections will be published.

---

## Executive Summary

Claim vs reality audit of Covalent (CXT). An AI data availability token that migrated one to one from CQT to a source verified, non upgradeable, non pausable, fee free Ethereum contract with a fixed 1 billion supply today, but whose verified code contains a multisig controllable, rate limited emission function, so the exchange marketed no inflation framing is contradicted even though minting is currently dormant.

Covalent CXT is the migrated network token of the Covalent data availability and AI data project, replacing the older CQT token at a one to one ratio. The Ethereum contract at 0x7ABc8A5768E6bE61A6c693a6e4EAcb5B60602C4D is confirmed by live RPC reads, by the aggregator platform mapping, and by a Sourcify exact source match. It is a standard OpenZeppelin style ERC20 with permit support, holds a fixed one billion total supply with roughly 96.7 percent circulating, carries no transfer fee, is not pausable, and is not upgradeable.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 1 |
| Medium | 1 |
| Low | 1 |
| Informational | 2 |
| **Total** | **5** |

### Overall Risk Assessment: MEDIUM

MEFAI onchain analysis places Covalent at 65 out of 100 (MEDIUM risk, Passed).

---

## 1. Token Overview

| Field | Value |
|-------|-------|
| **Token name and symbol** | Covalent / CXT |
| **Contract or program** | `0x7ABc8A5768E6bE61A6c693a6e4EAcb5B60602C4D` |
| **Chain** | Ethereum |
| **Tags** | ERC 20, AI Data Availability, Dormant Emission, Ethereum, Passed |

Control is not renounced and not held by a single private key. Instead a three of five Gnosis Safe multisig at 0x381225fa2dffa29c01b52214656077f8550f819e holds the admin, cap manager, and permit revoker roles. There is no legacy owner function, no proxy implementation slot, and no pause function, which means the code itself is immutable while the token supply and emission cap remain under multisig authority. Overall this reads as a competent and transparent deployment whose main residual risks are the retained multisig control and the dormant but real minting capability with no hard cap.

---

## 2. Onchain Security Assessment (MEFAI analysis)

- CXT on Ethereum (0x7ABc8A5768E6bE61A6c693a6e4EAcb5B60602C4D, 18 decimals) verified live and source verified on Sourcify: total supply exactly 1 billion, a genuine one to one migration from CQT with no expansion. It is not a proxy, not pausable, and has no transfer fee. Admin, cap manager, and permit revoker roles sit with a three of five Gnosis Safe (0x381225fa2dffa29c01b52214656077f8550f819e).
- A rate limited emission mint exists, but the emission role is held by the zero address, so no live account can mint today, and there is no hard cap in code.

---

## 3. Claim versus Reality

- "A fixed 1 billion supply with no inflationary mechanics" / Reality: the fixed total and honest migration check out, but the verified contract contains a multisig activatable, rate limited emission function with no cap, so the no inflation framing is contradicted; minting is dormant only because the emission role is unset.

Exchange facing marketing presents CXT as a fixed one billion supply with no inflationary mechanics, and the fixed total and honest one to one migration both check out on chain. The contradiction is that the verified contract does contain a multisig controllable and rate limited emission function for treasury emission, so the token is inflation capable by design even though it is switched off right now. The only reason no new tokens can be minted today is that the emission role currently sits at the zero address, a condition the admin multisig can change.

---

## 4. Website and Frontend Integrity

VERDICT: CLEAN | CONFIDENCE: high
Covalent's live frontend is clean from a user protection standpoint. Its code references the correct official CXT contract 0x7ABc8A5768E6bE61A6c693a6e4EAcb5B60602C4D and only that one, with no trace anywhere of the deprecated CQT contract or any lookalike, so there is nothing steering visitors toward the old token. The page is a read only marketing and data site with no wallet connection, no signing, and no approval or transfer logic, which means there is no drainer surface at all, and there is no eval, no obfuscation, and no remote script beyond the analytics loader and Covalent's own domains. Live figures come from the official Covalent API endpoints rather than being faked, and the audit reference is a genuine link to Covalent's own documentation instead of an unbacked badge. The only thing worth a second look is that the site's canonical social handle is listed as a non standard account rather than the expected official Covalent account, a minor identity note that does not put user funds at risk.


---

## 5. Findings by Severity

- HIGH: a multisig activatable, uncapped emission function against a no inflation claim (a latent inflation vector). MEDIUM: admin, cap, and revoker roles held by a multisig and not renounced. LOW: Safe signer identities and intent to activate emission are unknown. INFO: a source verified, non upgradeable, non pausable, fee free contract with a fixed supply today.

---

## 6. Score Breakdown

| Dimension | Score |
|-----------|-------|
| Ownership control | 10 |
| Supply and minting | 8 |
| Liquidity and market | 11 |
| Code safety | 13 |
| Transfer neutrality | 15 |
| Transparency | 8 |
| **Total** | **65/100** |

---

## 7. Conclusion

Claim vs reality audit of Covalent (CXT). An AI data availability token that migrated one to one from CQT to a source verified, non upgradeable, non pausable, fee free Ethereum contract with a fixed 1 billion supply today, but whose verified code contains a multisig controllable, rate limited emission function, so the exchange marketed no inflation framing is contradicted even though minting is currently dormant. On the MEFAI scale this token scores 65 out of 100 and is classified Passed.

---

## 8. Verification

- Methodology: manual review, onchain analysis using read only public RPC on Ethereum, and a review of the project website frontend against its stated claims.
- Onchain facts (supply, ownership, mint authority, upgradeability, pause, transfer fee) were read live from public nodes and cross checked against the project's public statements.
- Sources:
  - `etherscan 0x7abc8a...`
  - `sourcify`
  - `coingecko covalent-x-token`
  - `covalenthq migration blogs`
  - `okx CXT`
  - `etherscan old CQT 0xd41714...`
  - `ethereum-rpc.publicnode.com.`

---

*Mefai Security Research. Independent claim versus reality assessment. Onchain data verified with read only public RPC.*
