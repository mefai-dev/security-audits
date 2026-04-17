# Mefai Security Score - Methodology

Every audited project receives a **Mefai Security Score** from 0 to 100. This document explains exactly how that score is calculated so anyone can verify it independently.

---

## Scoring Categories

The score is divided into 6 categories. Each category has a maximum point value and clear criteria for how points are awarded or deducted.

| Category | Max Points | What It Measures |
|----------|-----------|------------------|
| 1. Ownership & Access Control | 20 | Who can modify the contract/token after deployment |
| 2. Supply & Minting | 20 | Whether new tokens can be created |
| 3. Liquidity & LP Security | 20 | Whether liquidity can be removed |
| 4. Code & Program Safety | 15 | Contract quality, upgrade risks, known vulnerabilities |
| 5. Fee & Transfer Mechanics | 15 | Hidden taxes, transfer restrictions, fee manipulation |
| 6. Transparency & Metadata | 10 | Metadata integrity, verified source code, documentation |

**Total: 100 points**

---

## Category 1: Ownership & Access Control (20 points)

| Condition | Points |
|-----------|--------|
| Ownership renounced (zero address / no authority) | 20 |
| Ownership behind multisig with timelock | 16 |
| Ownership behind multisig without timelock | 12 |
| Single owner with timelock on critical functions | 8 |
| Single owner, no timelock | 4 |
| Owner can call destructive functions (pause, blacklist, selfdestruct) | 0 |

**EVM:** Checked via `owner()` call. Verified the return value is zero address.
**Solana:** Checked via update authority and program upgrade authority.

---

## Category 2: Supply & Minting (20 points)

| Condition | Points |
|-----------|--------|
| No mint function / mint authority revoked | 20 |
| Mint capped at fixed max supply enforced in code | 16 |
| Mint behind multisig or governance | 12 |
| Mint behind single owner with rate limit | 8 |
| Unlimited minting by single owner | 0 |

**EVM:** Source code reviewed for `mint()`, `_mint()` functions and access modifiers.
**Solana:** Mint authority field checked via `getAccountInfo`. Null = revoked.

---

## Category 3: Liquidity & LP Security (20 points)

| Condition | Points |
|-----------|--------|
| LP tokens burned (permanently locked) | 20 |
| LP locked in audited locker for 12+ months | 18 |
| LP locked in audited locker for 6-12 months | 16 |
| LP locked in audited locker for 3-6 months | 12 |
| LP locked for less than 3 months | 8 |
| LP partially locked (less than 80%) | 4 |
| LP unlocked / no LP lock | 0 |

**Verification:** LP lock is verified on-chain by reading the locker contract/program account directly via RPC. We decode the binary lock record to extract lock amount, lock date, and unlock date. We do not rely on third-party APIs for LP lock status.

---

## Category 4: Code & Program Safety (15 points)

| Condition | Points |
|-----------|--------|
| No vulnerabilities found, standard program/library | 15 |
| Informational findings only | 13 |
| Low severity findings only | 10 |
| Medium severity findings | 6 |
| High severity findings | 3 |
| Critical severity findings | 0 |

**EVM:** Line-by-line source code review. Checked for reentrancy, overflow, access control, proxy risks, front-running, flash loan vectors.
**Solana:** Token program type verified. Standard SPL Token = maximum safety (audited by Solana Labs). Token-2022 extensions analyzed individually. Custom programs require separate code audit.

**Deductions:**
- Contract not verified on block explorer: -3
- Uses proxy/upgrade pattern: -3
- Compiler version below 0.8.0 (no overflow protection): -5

---

## Category 5: Fee & Transfer Mechanics (15 points)

| Condition | Points |
|-----------|--------|
| No transfer fee, no restrictions | 15 |
| Fixed fee (immutable in code, cannot be changed) | 13 |
| Fee adjustable but capped in code (e.g. max 5%) | 10 |
| Fee adjustable by owner, no cap | 4 |
| Hidden fee mechanism or reflection tax | 2 |
| Freeze/blacklist/pause authority active | 0 |

**EVM:** Fee variables checked for `constant` or `immutable` modifiers. `onlyOwner` fee setters identified.
**Solana:** Transfer fee extension checked. Freeze authority checked via `getAccountInfo`.

---

## Category 6: Transparency & Metadata (10 points)

| Condition | Points |
|-----------|--------|
| Source code verified + metadata immutable + project website active | 10 |
| Source code verified + metadata immutable | 8 |
| Source code verified but metadata mutable | 6 |
| Source code not verified but metadata immutable | 4 |
| Source code not verified and metadata mutable | 2 |
| No verifiable information | 0 |

**EVM:** BscScan/Etherscan verification status.
**Solana:** Metadata mutability flag. URI availability.

---

## Score to Rating Conversion

| Score | Rating | Meaning |
|-------|--------|---------|
| 90 - 100 | **LOW RISK** | Safe. Best practices followed. No critical concerns. |
| 75 - 89 | **LOW-MEDIUM RISK** | Minor issues. Does not threaten funds. |
| 60 - 74 | **MEDIUM RISK** | Attention needed. Conditional risks exist. |
| 40 - 59 | **MEDIUM-HIGH RISK** | Significant concerns. Review before interacting. |
| 20 - 39 | **HIGH RISK** | Direct risk to funds. Proceed with extreme caution. |
| 0 - 19 | **CRITICAL RISK** | Immediate danger. Do not interact. |

---

## Example: MEFAI BSC Score Breakdown

| Category | Check | Result | Points |
|----------|-------|--------|--------|
| 1. Ownership | `owner()` returns zero address | Renounced | **20/20** |
| 2. Supply | No `mint()` function, fixed 1B supply | No minting possible | **20/20** |
| 3. Liquidity | LP status on PancakeSwap | Verified | **16/20** |
| 4. Code Safety | Solidity 0.8.23, OpenZeppelin v5, 0 medium+ findings | Clean | **15/15** |
| 5. Fee Mechanics | `FEE_PERCENT` is `constant` (1%), cannot be changed | Fixed fee | **13/15** |
| 6. Transparency | Verified on BscScan, active website | Full transparency | **10/10** |
| **TOTAL** | | | **94/100** |

**Rating: LOW RISK (94/100)**

---

## Example: MEFAI Solana Score Breakdown

| Category | Check | Result | Points |
|----------|-------|--------|--------|
| 1. Ownership | Mint authority = null, freeze authority = null | Revoked | **20/20** |
| 2. Supply | Mint authority revoked, ~100M fixed supply | No minting possible | **20/20** |
| 3. Liquidity | PinkLock until 2026-09-25 (6 months) | Locked 6-12 months | **16/20** |
| 4. Code Safety | Standard SPL Token, no extensions, no custom program | Maximum safety | **15/15** |
| 5. Fee Mechanics | 0% transfer fee, no freeze authority | No fees, no restrictions | **15/15** |
| 6. Transparency | Metadata immutable, mefai.io active | Full transparency | **10/10** |
| **TOTAL** | | | **96/100** |

**Rating: LOW RISK (96/100)**

---

## Principles

1. **Reproducible** - Anyone can verify every score by running the same on-chain checks
2. **Objective** - Criteria are binary or clearly defined, not subjective
3. **On-chain first** - Every claim is backed by a blockchain query, not a third-party API
4. **Transparent** - Full breakdown shown for every audited project
5. **Conservative** - When in doubt, the lower score is applied

---

*Mefai Security Research - Scoring Methodology v1.0*
