# Mefai Security Audits

Independent smart contract and token security audit reports by **Mefai Security Research**.

Submit any contract address for audit - we review tokens across all major chains including BNB Smart Chain, Solana, Ethereum, and more.

---

## How It Works

Our audit process combines two layers:

1. **Automated Scan** - Our platform at [mefai.io](https://mefai.io) provides rapid initial analysis using proprietary scanning tools. This identifies common vulnerability patterns, authority configurations, and on-chain risk indicators within seconds.

2. **Manual Review** - Every report published in this repository has been **manually reviewed and verified** by the Mefai security team. Automated scans are a starting point - they miss context, produce false positives, and cannot reason about business logic. Manual verification ensures every finding is accurate, every on-chain claim is confirmed via direct RPC calls, and the final risk rating reflects reality.

> Automated tools flag risks. Humans confirm them. Every audit here has passed both.

---

## Audit Reports

Search by token name or contract address:

| Token | Chain | Contract Address | Score | Risk | Date | Report |
|-------|-------|-----------------|-------|------|------|--------|
| **MEFAI** | BSC | `0x45E57907058c707a068100De358BA4535b18E2F3` | **94/100** | **LOW** | 2026-03-25 | [View](audits/MEFAI-BSC-0x45E57907058c707a068100De358BA4535b18E2F3.md) |
| **MEFAI** | Solana | `7gcoey4EXJcZ8u3iGYhgTBrh3JuhLWzV4Gs1zNaPtu3U` | **96/100** | **LOW** | 2026-03-25 | [View](audits/MEFAI-SOL-7gcoey4EXJcZ8u3iGYhgTBrh3JuhLWzV4Gs1zNaPtu3U.md) |
| **MEFAI** | Solana (Deep) | `7gcoey4EXJcZ8u3iGYhgTBrh3JuhLWzV4Gs1zNaPtu3U` | **96/100** | **LOW** | 2026-03-25 | [View](audits/MEFAI-SOL-DEEP-7gcoey4EXJcZ8u3iGYhgTBrh3JuhLWzV4Gs1zNaPtu3U.md) |
| **ClipX** | BSC | `0xc269d59a0d608ea0bd672f2f4616c372d8554444` | **91/100** | **LOW** | 2026-03-25 | [View](audits/CLIPX-BSC-0xc269d59a0d608ea0bd672f2f4616c372d8554444.md) |
| **Ensoul** | BSC | `0xf671f96f7763e88ea92ff7db79d57c0ee3c7ffff` | **88/100** | **LOW-MEDIUM** | 2026-03-25 | [View](audits/ENSOUL-BSC-0xf671f96f7763e88ea92ff7db79d57c0ee3c7ffff.md) |
| **FLAP** | BSC | `0xCD96a472b448d6C2c99F714737E7F9F4fCa67777` | **89/100** | **LOW-MEDIUM** | 2026-03-28 | [View](audits/FLAP-BSC-0xCD96a472b448d6C2c99F714737E7F9F4fCa67777.md) |
| **WLFI** | Ethereum | `0xda5e1988097297dcdc1f90d4dfe7909e847cbef6` | **0/100** | **CRITICAL** | 2026-03-28 | [View](audits/WLFI-ETH-0xda5e1988097297dcdc1f90d4dfe7909e847cbef6.md) |

> How is the score calculated? See [SCORING.md](SCORING.md) for the full methodology.

---

## Key Findings Summary

### MEFAI - BNB Smart Chain

| Check | Status |
|-------|--------|
| Ownership | **RENOUNCED** - verified via BSC RPC (`owner()` = zero address) |
| Minting | **DISABLED** - no mint function, fixed 1B supply |
| Fee | **IMMUTABLE** - 1% constant, cannot be changed |
| Team Wallet | **IMMUTABLE** - cannot be redirected |
| Proxy/Upgrade | **NONE** - contract is permanent |
| Vulnerabilities | **0 Critical, 0 High, 0 Medium** |

### MEFAI - Solana

| Check | Status |
|-------|--------|
| Mint Authority | **REVOKED** - no new tokens can be created |
| Freeze Authority | **REVOKED** - no wallets can be frozen |
| Metadata | **IMMUTABLE** - name/symbol/image cannot be changed |
| Transfer Fee | **0%** - no hidden tax |
| Token Program | **Standard SPL Token** - no hidden extensions |
| LP Lock | **LOCKED** - PinkLock until 2026-09-25 ([verify on-chain](https://www.pinksale.finance/solana/pinklock/record/9vbrLMNnxnthojJYQZc1cCKvdPzSmzPe33uHmxocC2mF)) |
| Vulnerabilities | **0 Critical, 0 High, 0 Medium** |

### ClipX - BNB Smart Chain

| Check | Status |
|-------|--------|
| Ownership | **RENOUNCED** - verified via BSC RPC (`owner()` = zero address) |
| Minting | **DISABLED** - no mint function, fixed 1B supply |
| Source Code | **VERIFIED** - on BscScan |
| LP Security | **BURNED** - 100% of LP tokens sent to dead address |
| Proxy/Upgrade | **NONE** - contract is permanent |
| Deployment | **Four.Meme** - standard launchpad contract |
| Vulnerabilities | **0 Critical, 0 High, 0 Medium** |

### Ensoul - BNB Smart Chain

| Check | Status |
|-------|--------|
| Ownership | **RENOUNCED** - verified via BSC RPC (`owner()` = zero address) |
| Minting | **DISABLED** - no mint function, fixed 1B supply |
| Fee | **3% IMMUTABLE** - locked at 3%, 100% to founder wallet, cannot be changed |
| LP Security | **BURNED** - 100% of LP tokens sent to dead address |
| Proxy/Upgrade | **NONE** - contract is permanent |
| Blacklist | **LOCKED** - ownership renounced, cannot blacklist any address |
| Deployment | **Four.Meme** - Advanced Token template |
| Vulnerabilities | **0 Critical, 0 High, 0 Medium** |

### WLFI - Ethereum (World Liberty Financial)

| Check | Status |
|-------|--------|
| Ownership | **CENTRALIZED** - single multisig controls token + proxy upgrades, no timelock |
| Minting | **UNLIMITED** - `mint(uint256)` with no supply cap |
| Source Code | **MISMATCH** - on-chain bytecode (18,657 bytes) ≠ GitHub source (~7,385 bytes compiled) |
| Hidden Functions | **28 UNDISCLOSED** - including `ownerReallocateFrom` (token seizure), `isBlacklisted`, guardian role |
| Governance | **FACADE** - single multisig holds 100% of `MAX_VOTING_POWER` |
| Proxy/Upgrade | **UPGRADEABLE** - TransparentUpgradeableProxy, no timelock |
| Blacklist | **ACTIVE** - `isBlacklisted(JustinSun) = TRUE` (function not in GitHub code) |
| Vulnerabilities | **4 Critical, 5 High, 3 Medium** |
| Score | **0/100 - CRITICAL RISK** |

### FLAP - BNB Smart Chain

| Check | Status |
|-------|--------|
| Ownership | **RENOUNCED** - verified via BSC RPC (`owner()` = zero address) |
| Minting | **DISABLED** - `mint()` selector not found in implementation bytecode |
| Fee | **3% PERMANENT** - locked for 100 years, cannot be changed (owner renounced) |
| LP Security | **BURNED** - 100% of LP tokens sent to dead address |
| Proxy/Upgrade | **NONE** - EIP-1167 immutable clone (implementation hardcoded in bytecode) |
| Blacklist/Pause | **NOT FOUND** - selectors absent from implementation bytecode |
| Honeypot | **NO** - buy and sell both execute via PancakeSwap router |
| Source Verified | **NO** - not verified on BSCScan |
| Vulnerabilities | **0 Critical, 0 High, 1 Medium** |

---

## Verification Methodology

Every published audit follows this process:

### Step 1: Automated Analysis
- On-chain data collection via direct blockchain RPC
- Authority and configuration checks
- Known vulnerability pattern matching
- Holder distribution and liquidity analysis

### Step 2: Manual Verification
- Line-by-line source code review (EVM contracts)
- On-chain authority verification via direct RPC calls - not relying on third-party APIs
- LP lock verification by decoding locker program accounts directly from blockchain
- Business logic analysis and DeFi integration risk assessment
- False positive elimination - automated tools frequently misreport locked LP, renounced ownership, etc.

### Supported Chains
- BNB Smart Chain (BSC)
- Solana
- Ethereum
- Polygon
- Arbitrum
- Base
- Additional chains on request

---

## Scoring System

Every project receives a score from **0 to 100** across 6 categories:

| Category | Max Points |
|----------|-----------|
| Ownership & Access Control | 20 |
| Supply & Minting | 20 |
| Liquidity & LP Security | 20 |
| Code & Program Safety | 15 |
| Fee & Transfer Mechanics | 15 |
| Transparency & Metadata | 10 |

| Score | Rating |
|-------|--------|
| 90-100 | **LOW RISK** - Safe. Best practices followed. |
| 75-89 | **LOW-MEDIUM RISK** - Minor issues. Funds not threatened. |
| 60-74 | **MEDIUM RISK** - Attention needed. Conditional risks. |
| 40-59 | **MEDIUM-HIGH RISK** - Significant concerns. Review first. |
| 20-39 | **HIGH RISK** - Direct risk to funds. Extreme caution. |
| 0-19 | **CRITICAL RISK** - Immediate danger. Do not interact. |

Full scoring criteria with examples: **[SCORING.md](SCORING.md)**

---

## Submit a Contract for Audit

Want your project audited? Contact us:

- **Website:** [mefai.io](https://mefai.io)
- **GitHub:** [github.com/mefai-dev](https://github.com/mefai-dev)

---

## Report Template

See [TEMPLATE.md](TEMPLATE.md) for our standardized audit report format.
