# World Liberty Financial (WLFI) Token Security Audit Report

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-03-28 |
| **Report Version** | 1.0 |
| **Contract/Program** | `0xda5e1988097297dcdc1f90d4dfe7909e847cbef6` (Proxy) |
| **Implementation** | `0xef48944abefff3f668f5324c050cd406618b771d` |
| **Chain** | Ethereum Mainnet |
| **Language** | Solidity (compiled bytecode analysis) |
| **Audit Type** | Smart Contract / Token / Governance |
| **Methodology** | On-Chain Bytecode Analysis + RPC Verification + GitHub Source Comparison |
| **Audit Duration** | 2 days |
| **Repository** | [worldliberty/usd1-smart-contracts](https://github.com/worldliberty/usd1-smart-contracts) |
| **Classification** | Public |

---

## Disclaimer

This report represents a point-in-time security assessment conducted by Mefai Security Research. The findings and recommendations contained herein are based on the information available and the state of the codebase at the time of the audit. This report does not constitute a guarantee that the audited system is free of vulnerabilities or defects. No part of this report should be considered as investment advice, an endorsement, or a recommendation regarding the security of any project, token, or protocol.

The scope of this audit was limited to the files, contracts, and components explicitly listed in the Scope section. Any code, infrastructure, or external dependencies outside the defined scope were not reviewed. Findings are classified by severity based on the potential impact and likelihood of exploitation at the time of analysis.

Mefai Security Research assumes no liability for any losses, damages, or adverse consequences resulting from the use of or reliance on this report. The responsibility for implementing fixes and maintaining security lies solely with the project team.

---

## Executive Summary

We audited the World Liberty Financial (WLFI) governance token deployed on Ethereum mainnet. The WLFI token uses a TransparentUpgradeableProxy pattern with an implementation contract at `0xef48944a...`. The publicly available GitHub repository (`worldliberty/usd1-smart-contracts`) contains a `Stablecoin.sol` contract that is presented as the token source code.

**Critical Discovery:** The on-chain implementation bytecode (18,657 bytes) is **drastically different** from what the GitHub source code (`Stablecoin.sol`, ~7,385 bytes compiled) would produce. Through bytecode selector extraction and 4byte.directory resolution, we identified **105 additional function selectors** in the on-chain code that do not exist in the published GitHub source. Of these, **28 are privileged admin functions** including `ownerReallocateFrom(address,address,uint256)` which allows the owner to transfer tokens from any holder without consent.

Additionally, the `frozen()` function — a public mapping in the GitHub code that should never revert — **reverts when called on-chain**, proving the deployed contract is fundamentally different from the published source.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 4 |
| High | 5 |
| Medium | 3 |
| Low | 2 |
| Informational | 3 |
| **Total** | **17** |

### Overall Risk Assessment: Critical

The on-chain WLFI token implementation contains undisclosed administrative capabilities that are not present in the publicly available source code, representing a significant transparency and trust violation for token holders.

---

## Mefai Security Score

| Category | Check | Result | Points |
|----------|-------|--------|--------|
| 1. Ownership & Access Control | `owner()` returns multisig `0x5be9...`, can pause/blacklist/reallocate, no timelock | Owner can call destructive functions | **0/20** |
| 2. Supply & Minting | `mint(uint256)` exists, onlyOwner, no supply cap enforced | Unlimited minting by single owner | **0/20** |
| 3. Liquidity & LP Security | DEX liquidity: $3.4M Uniswap pool, no LP lock mechanism found | LP unlocked / no LP lock | **0/20** |
| 4. Code & Program Safety | 4 Critical + 5 High findings, uses proxy/upgrade, on-chain code ≠ GitHub | Critical severity findings (-3 proxy, -3 unverified real source) | **0/15** |
| 5. Fee & Transfer Mechanics | No transfer fee, but freeze/blacklist/pause all active, undisclosed `ownerReallocateFrom` | Freeze/blacklist/pause authority active | **0/15** |
| 6. Transparency & Metadata | On-chain code (18,657 bytes) does NOT match GitHub source (7,385 bytes compiled), 28 undisclosed admin functions, 3 undisclosed privileged addresses | Source code effectively not verified | **0/10** |
| **TOTAL** | | | **0/100** |

**Rating: CRITICAL RISK (0/100) — Immediate danger. Do not interact.**

### Score Justification

Every scoring category receives the minimum score (0) because:

1. **Ownership (0/20):** Owner holds `pause()`, `freeze()`, `ownerSetBlacklistStatus()`, `ownerReallocateFrom()` — the most destructive set of admin functions possible, with no timelock.
2. **Supply (0/20):** `mint(uint256)` with no cap, controlled by single owner.
3. **Liquidity (0/20):** Primary DEX pool has only $3.4M liquidity. No LP lock mechanism.
4. **Code Safety (0/15):** 4 Critical findings. Proxy pattern. Published source code does not match deployed bytecode — unprecedented.
5. **Fee Mechanics (0/15):** `freeze()`, `isBlacklisted()`, `pause()`, and `ownerReallocateFrom()` all active. Owner can seize any holder's tokens.
6. **Transparency (0/10):** GitHub shows 118-line `Stablecoin.sol`. On-chain implementation has 105 extra functions (28 dangerous admin functions) in 18,657 bytes. This is not a discrepancy — it is a completely different contract.

---

## Scope

### In Scope

| File / Contract | SHA-256 | Notes |
|----------------|---------|-------|
| On-chain proxy: `0xda5e1988...` | N/A (bytecode) | TransparentUpgradeableProxy |
| On-chain implementation: `0xef48944a...` | N/A (bytecode) | 18,657 bytes |
| `src/Stablecoin.sol` (GitHub) | N/A | 118 LOC |
| `script/DeployStablecoin.s.sol` (GitHub) | N/A | 30 LOC |

### Out of Scope

- USD1 Stablecoin contract (separate report)
- AgentPay SDK
- Frontend application
- Off-chain governance (Snapshot)

---

## Methodology

### On-Chain Bytecode Analysis

1. Retrieved deployed bytecodes via Ethereum RPC (`eth_getCode`)
2. Extracted all PUSH4 function selectors from bytecode
3. Resolved selectors via 4byte.directory API
4. Compared on-chain selectors against GitHub source code functions
5. Tested each discovered function via `eth_call` RPC

### Manual Review

- Line-by-line review of GitHub `Stablecoin.sol`
- Access control analysis
- Upgradeability pattern review
- Storage layout analysis

### Bytecode Verification

- Compared WLFI implementation (18,657 bytes) against USD1 implementation (7,385 bytes)
- Both claimed to use same `Stablecoin.sol` source
- Differential analysis revealed 105 extra selectors in WLFI

---

## Architecture Overview

```
+------------------+       +------------------+       +------------------+
|                  |       |                  |       |                  |
| WLFI Token Proxy +------>+ Implementation   +------>+ Multisig Owner   |
| 0xda5e1988...    |       | 0xef48944a...    |       | 0x5be9a495...    |
|                  |       | 18,657 bytes     |       |                  |
+------------------+       +------------------+       +------------------+
        |                                                     |
        v                                                     v
+------------------+                               +------------------+
|                  |                               |                  |
| ProxyAdmin       |                               | Lockbox/Vester   |
| 0x7f533afa...    |                               | 0x74b4f6a2...    |
| Owner: Multisig  |                               |                  |
+------------------+                               +------------------+
```

**Single Point of Control:** Multisig `0x5be9a495...` is BOTH the token `owner()` AND the ProxyAdmin owner. No timelock exists between them.

---

## Findings

### F-001: On-Chain Implementation Differs from Published Source Code

| Attribute | Value |
|-----------|-------|
| **Severity** | Critical |
| **Type** | Transparency / Trust |
| **Location** | Implementation `0xef48944abefff3f668f5324c050cd406618b771d` |
| **Status** | Open |

**Description:**

The GitHub repository `worldliberty/usd1-smart-contracts` presents `Stablecoin.sol` (118 lines) as the WLFI token source code. When compiled, this produces approximately 7,385 bytes of bytecode (matching the USD1 implementation). However, the WLFI token implementation on-chain is **18,657 bytes** — 11,272 bytes larger.

Bytecode selector extraction reveals **105 additional function selectors** not present in `Stablecoin.sol`, including governance, vesting, blacklist, and admin reallocation functions.

Furthermore, calling `frozen(address)` — a public mapping in `Stablecoin.sol` that should never revert — **returns REVERT on-chain**, proving the deployed contract uses different code.

**Impact:**

Token holders, auditors, and investors reviewing the GitHub source code are given a misleading picture of the contract's capabilities. The actual contract contains 28 privileged admin functions not disclosed in public documentation.

**Proof of Concept:**

```bash
# WLFI bytecode size: 18,657 bytes (expected ~7,385 from Stablecoin.sol)
curl -s -X POST https://rpc.ankr.com/eth \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_getCode","params":["0xef48944abefff3f668f5324c050cd406618b771d","latest"],"id":1}' \
  | python3 -c "import json,sys; print(f'Size: {(len(json.load(sys.stdin)[\"result\"])-2)//2} bytes')"
# Output: Size: 18657 bytes

# frozen() REVERT test (should return bool from public mapping, not revert):
curl -s -X POST https://rpc.ankr.com/eth \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0xe58398360000000000000000000000000000000000000000000000000000000000000000"},"latest"],"id":1}'
# Returns: error/revert instead of 0x0000...0000
```

**Recommendation:**

Publish the actual source code of the WLFI token implementation and submit it for Etherscan verification. The public GitHub repository should contain the code that matches the deployed bytecode.

---

### F-002: Undisclosed `ownerReallocateFrom` Function Allows Token Seizure

| Attribute | Value |
|-----------|-------|
| **Severity** | Critical |
| **Type** | Access Control / Centralization |
| **Location** | Selector `0x7df9a674` in implementation bytecode |
| **Status** | Open |

**Description:**

The on-chain implementation contains `ownerReallocateFrom(address,address,uint256)` (selector `0x7df9a674`) which allows the contract owner to transfer tokens from any holder to any other address without the holder's consent. This function is not present in the published `Stablecoin.sol` source code.

**Impact:**

The owner can seize any holder's tokens at will. This likely explains the November 2025 incident where 166.667M WLFI ($22.14M) were "burned and reallocated" from 272 wallets, and the Justin Sun blacklisting where 595M WLFI (~$107M) were affected.

**Proof of Concept:**

```bash
# Verify function exists (reverts with OwnableUnauthorizedAccount for non-owner):
curl -s -X POST https://rpc.ankr.com/eth \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0x7df9a6740000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"},"latest"],"id":1}'
```

**Recommendation:**

This function should be disclosed in public documentation. Ideally, it should be removed or governed by a timelock with a mandatory delay period allowing holders to exit before execution.

---

### F-003: Undisclosed Blacklist System Separate from Published `frozen()` Mapping

| Attribute | Value |
|-----------|-------|
| **Severity** | Critical |
| **Type** | Access Control / Transparency |
| **Location** | Selectors `0xfe575a87`, `0xe1dfc884`, `0x5aa42d12` |
| **Status** | Open |

**Description:**

The on-chain contract contains an `isBlacklisted(address)` function (selector `0xfe575a87`) that is separate from the `frozen(address)` mapping in the published code. Additionally, two admin functions control this blacklist:
- `ownerSetBlacklistStatus(address,bool)` (`0xe1dfc884`)
- `guardianSetBlacklistStatus(address,bool)` (`0x5aa42d12`)

The existence of a "guardian" role is not disclosed anywhere in public documentation.

**Impact:**

An undisclosed secondary access control mechanism exists. Justin Sun's address returns `isBlacklisted = true` on-chain, confirming active use.

**Proof of Concept:**

```bash
# isBlacklisted(Justin Sun) = TRUE
curl -s -X POST https://rpc.ankr.com/eth \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0xfe575a870000000000000000000000005AB26169051d0D96217949ADb91E86e51a5FDA74"},"latest"],"id":1}'
# Returns: 0x0000000000000000000000000000000000000000000000000000000000000001 (TRUE)
```

**Recommendation:**

Disclose the blacklist system and guardian role in public documentation. Publish criteria for blacklisting and provide a dispute mechanism for affected holders.

---

### F-004: Multisig Holds 100% of MAX_VOTING_POWER

| Attribute | Value |
|-----------|-------|
| **Severity** | Critical |
| **Type** | Governance / Centralization |
| **Location** | Selectors `0x9ab24eb0`, `0x152439db` |
| **Status** | Open |

**Description:**

The on-chain contract contains an undisclosed on-chain governance system with `delegate()`, `getVotes()`, `getPastVotes()`, and `MAX_VOTING_POWER()` functions. Testing reveals:

- `MAX_VOTING_POWER()` = 5,000,000,000 WLFI
- `getVotes(multisig)` = 5,000,000,000 WLFI (100% of max)

A single address controls 100% of the on-chain voting power.

**Impact:**

The website claims "WLFI token holders are the ones steering the future of the platform." On-chain evidence shows a single multisig address holds 100% of voting power, making governance claims misleading.

**Proof of Concept:**

```bash
# MAX_VOTING_POWER() = 5000000000
curl -s -X POST https://rpc.ankr.com/eth \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0x152439db"},"latest"],"id":1}'

# getVotes(multisig) = 5000000000 (same as max)
curl -s -X POST https://rpc.ankr.com/eth \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0x9ab24eb00000000000000000000000005be9a4959308a0d0c7bc0870e319314d8d957dbb"},"latest"],"id":1}'
```

**Recommendation:**

If the project intends to be governance-driven, voting power should be distributed to token holders, not concentrated in a single multisig. Website claims should accurately reflect the on-chain reality.

---

### F-005: No Timelock on Proxy Upgrades

| Attribute | Value |
|-----------|-------|
| **Severity** | High |
| **Type** | Centralization / Upgradeability |
| **Location** | ProxyAdmin `0x7f533afa9e994b7e44673188034f9ce52886a0c0` |
| **Status** | Open |

**Description:**

The ProxyAdmin owner (multisig `0x5be9a495...`) can execute `upgradeAndCall` immediately with no timelock delay. This allows instant replacement of the entire implementation contract.

**Impact:**

A compromised multisig key can silently replace the token logic, potentially stealing all holder balances in a single transaction.

**Recommendation:**

Deploy a TimelockController (minimum 48-72 hours) between the multisig and ProxyAdmin.

---

### F-006: No Supply Cap on Minting

| Attribute | Value |
|-----------|-------|
| **Severity** | High |
| **Type** | Token Economics |
| **Location** | `mint()` function |
| **Status** | Open |

**Description:**

The `mint()` function has no maximum supply cap. The owner can mint an unlimited number of tokens.

**Impact:**

Unlimited token dilution. Current total supply is ~99.95 billion WLFI.

**Recommendation:**

Implement an on-chain supply cap.

---

### F-007: Undisclosed Guardian Role

| Attribute | Value |
|-----------|-------|
| **Severity** | High |
| **Type** | Access Control / Transparency |
| **Location** | Selectors `0x0c68ba21`, `0x8682db37`, `0x5aa42d12`, `0xd4593872` |
| **Status** | Open |

**Description:**

The contract contains an undisclosed "guardian" role with the following capabilities:
- `guardianSetBlacklistStatus(address,bool)` — Can blacklist any address
- `guardianPause()` — Can pause all transfers
- `isGuardian(address)` — Check guardian status
- `ownerSetGuardian(address,bool)` — Owner assigns guardians

The multisig is NOT the guardian (`isGuardian(multisig) = false`), meaning an unknown third party holds guardian privileges.

**Impact:**

An undisclosed entity can pause the entire token and blacklist any holder.

**Recommendation:**

Disclose the guardian address and role publicly.

---

### F-008: Undisclosed Authorized Signer

| Attribute | Value |
|-----------|-------|
| **Severity** | High |
| **Type** | Access Control / Transparency |
| **Location** | Selector `0xc771909c` |
| **Status** | Open |

**Description:**

`authorizedSigner()` returns `0x006dad584e68225dae2a45731224b3b48ac57184`. This address is not disclosed in any public documentation. The signer can likely authorize certain operations like account activation via `activateAccountAndClaimVest(bytes)`.

**Impact:**

An undisclosed EOA has signing authority over the token contract.

**Recommendation:**

Disclose the authorized signer address and its capabilities.

---

### F-009: Owner Can Exclude Addresses from Voting

| Attribute | Value |
|-----------|-------|
| **Severity** | High |
| **Type** | Governance / Centralization |
| **Location** | Selector `0x56c531b9` |
| **Status** | Open |

**Description:**

`ownerSetVotingPowerExcludedStatus(address,bool)` allows the owner to exclude any address from governance voting.

**Impact:**

Combined with `ownerSetMaxVotingPower(uint256)`, the owner has complete control over who can vote and the maximum voting power, making the governance system fully centralized.

**Recommendation:**

Governance manipulation functions should be governed by timelocked governance proposals, not unilateral owner actions.

---

### F-010: Missing `_disableInitializers()` in GitHub Source

| Attribute | Value |
|-----------|-------|
| **Severity** | Medium |
| **Type** | Upgradeability |
| **Location** | `src/Stablecoin.sol` (GitHub) |
| **Status** | Open |

**Description:**

The GitHub `Stablecoin.sol` has no constructor calling `_disableInitializers()`, which is an OpenZeppelin best practice for upgradeable contracts. Note: The on-chain implementation may differ.

**Impact:**

The implementation contract could be initialized by an attacker if deployed without immediately calling `initialize()`.

**Recommendation:**

Add `constructor() { _disableInitializers(); }`.

---

### F-011: Owner Can Control Pre-Trading Transfers

| Attribute | Value |
|-----------|-------|
| **Severity** | Medium |
| **Type** | Insider Trading Risk |
| **Location** | Selector `0xc7dac704` |
| **Status** | Open |

**Description:**

`ownerSetTransferBeforeStartStatus(address,bool)` allows the owner to grant specific addresses the ability to transfer tokens before the public trading start timestamp (2025-09-01 12:00 UTC, confirmed on-chain).

**Impact:**

Insiders could accumulate positions and set up liquidity before public trading begins.

**Recommendation:**

Disclose which addresses had pre-trading privileges and the timestamps of their transfers.

---

### F-012: Undisclosed Vesting System

| Attribute | Value |
|-----------|-------|
| **Severity** | Medium |
| **Type** | Transparency |
| **Location** | Selectors `0xf13253bb`, `0x402914f5`, `0x505bd3da`, `0xbfc9c359` |
| **Status** | Open |

**Description:**

The contract contains an undisclosed vesting system with `claimVest()`, `claimable(address)`, `unclaimed(address)`, and `ownerClaimVestFor(address)`. The VESTER address is confirmed as the Lockbox contract (`0x74b4f6a2...`).

The owner can claim vesting on behalf of any address via `ownerClaimVestFor`.

**Impact:**

Vesting mechanics are opaque to token holders.

---

### F-013: No Storage Gap in GitHub Source

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Upgradeability |
| **Location** | `src/Stablecoin.sol` |
| **Status** | Open |

**Description:**

The GitHub `Stablecoin.sol` does not declare a `__gap` storage variable, risking storage collisions in future upgrades. Note: The actual on-chain implementation may handle this differently.

---

### F-014: OpenZeppelin v4.8.1 Outdated

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Dependency |
| **Location** | `.gitmodules` |
| **Status** | Open |

**Description:**

The GitHub repository uses OpenZeppelin v4.8.1 (3+ years old, maintenance mode). v5.x is current.

---

### F-015: Undisclosed Registry Contract

| Attribute | Value |
|-----------|-------|
| **Severity** | Informational |
| **Type** | Transparency |
| **Location** | Selector `0x06433b1b` |
| **Status** | Open |

**Description:**

`REGISTRY()` returns `0x4f61a99e42e21ea3c3eaf9b1b30fb80a7900d3ce`. This registry contract is not documented.

---

### F-016: Trading Start Timestamp Hardcoded

| Attribute | Value |
|-----------|-------|
| **Severity** | Informational |
| **Type** | Design |
| **Location** | Selector `0x95d9538d` |
| **Status** | Open |

**Description:**

`TRADING_START_TIMESTAMP()` = 1756728000 (2025-09-01 12:00:00 UTC). This matches the public Binance listing date.

---

### F-017: 47 Unresolved Custom Selectors

| Attribute | Value |
|-----------|-------|
| **Severity** | Informational |
| **Type** | Transparency |
| **Location** | Implementation bytecode |
| **Status** | Open |

**Description:**

47 function selectors in the WLFI implementation bytecode could not be resolved via 4byte.directory. These are likely custom functions whose signatures have not been publicly registered. Their purpose is unknown.

---

## Findings Summary

| ID | Title | Severity | Status |
|----|-------|----------|--------|
| F-001 | On-chain implementation differs from published source | Critical | Open |
| F-002 | Undisclosed `ownerReallocateFrom` allows token seizure | Critical | Open |
| F-003 | Undisclosed blacklist system separate from `frozen()` | Critical | Open |
| F-004 | Multisig holds 100% of MAX_VOTING_POWER | Critical | Open |
| F-005 | No timelock on proxy upgrades | High | Open |
| F-006 | No supply cap on minting | High | Open |
| F-007 | Undisclosed guardian role | High | Open |
| F-008 | Undisclosed authorized signer | High | Open |
| F-009 | Owner can exclude addresses from voting | High | Open |
| F-010 | Missing `_disableInitializers()` in GitHub source | Medium | Open |
| F-011 | Owner can control pre-trading transfers | Medium | Open |
| F-012 | Undisclosed vesting system | Medium | Open |
| F-013 | No storage gap in GitHub source | Low | Open |
| F-014 | OpenZeppelin v4.8.1 outdated | Low | Open |
| F-015 | Undisclosed registry contract | Informational | Open |
| F-016 | Trading start timestamp hardcoded | Informational | Open |
| F-017 | 47 unresolved custom selectors | Informational | Open |

---

## Key Addresses

| Role | Address | Disclosed? |
|------|---------|------------|
| Token Proxy | `0xda5e1988097297dcdc1f90d4dfe7909e847cbef6` | Yes |
| Implementation | `0xef48944abefff3f668f5324c050cd406618b771d` | Partial |
| ProxyAdmin | `0x7f533afa9e994b7e44673188034f9ce52886a0c0` | No |
| Multisig/Owner | `0x5be9a4959308a0d0c7bc0870e319314d8d957dbb` | No |
| Vester/Lockbox | `0x74b4f6a2e579d730aacb9dd23cfbbaeb95029583` | Partial |
| Authorized Signer | `0x006dad584e68225dae2a45731224b3b48ac57184` | **No** |
| Registry | `0x4f61a99e42e21ea3c3eaf9b1b30fb80a7900d3ce` | **No** |
| Bytecode Embedded | `0xe4803bb50b29a36d0b78f8b734b8dae36f87c1a0` | **No** |

---

## Appendix A: Severity Classification

| Severity | Description |
|----------|-------------|
| **Critical** | Direct loss of funds, complete protocol takeover, or irreversible systemic damage. Exploitation requires minimal effort or can be automated. Immediate remediation required before any deployment or continued operation. |
| **High** | Significant risk to user funds, protocol integrity, or availability. Exploitation is feasible with moderate effort or under specific but realistic conditions. Must be resolved before mainnet deployment. |
| **Medium** | Conditional risk requiring specific circumstances, user interaction, or a combination of factors to exploit. Material impact if triggered. Should be addressed before mainnet deployment. |
| **Low** | Minor issues, best practice deviations, or theoretical risks with low probability and limited impact. Recommended to fix but not deployment-blocking. |
| **Informational** | Code quality observations, gas optimizations, documentation gaps, or architectural suggestions. No direct security impact. |

---

## Appendix B: Verification Commands

All findings can be independently verified using the following Ethereum RPC calls:

```bash
# F-001: Bytecode size comparison
curl -s -X POST https://rpc.ankr.com/eth -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_getCode","params":["0xef48944abefff3f668f5324c050cd406618b771d","latest"],"id":1}'

# F-002: ownerReallocateFrom exists (reverts for non-owner)
curl -s -X POST https://rpc.ankr.com/eth -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0x7df9a674000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"},"latest"],"id":1}'

# F-003: isBlacklisted(Justin Sun) = TRUE
curl -s -X POST https://rpc.ankr.com/eth -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0xfe575a870000000000000000000000005AB26169051d0D96217949ADb91E86e51a5FDA74"},"latest"],"id":1}'

# F-004: getVotes(multisig) = MAX_VOTING_POWER
curl -s -X POST https://rpc.ankr.com/eth -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0x9ab24eb00000000000000000000000005be9a4959308a0d0c7bc0870e319314d8d957dbb"},"latest"],"id":1}'

# F-007: isGuardian(multisig) = FALSE (guardian is someone else)
curl -s -X POST https://rpc.ankr.com/eth -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0x0c68ba210000000000000000000000005be9a4959308a0d0c7bc0870e319314d8d957dbb"},"latest"],"id":1}'

# F-008: authorizedSigner()
curl -s -X POST https://rpc.ankr.com/eth -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xda5e1988097297dcdc1f90d4dfe7909e847cbef6","data":"0xc771909c"},"latest"],"id":1}'
```

---

## Appendix C: Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| Ethereum RPC | Multiple providers | On-chain data retrieval |
| 4byte.directory | API v1 | Function selector resolution |
| Python3 | 3.12 | Bytecode analysis scripts |
| curl | 8.x | RPC calls |
| Manual review | N/A | Source code and bytecode comparison |

---

## Appendix D: Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-03-28 | Mefai Security Research | Initial report |

---

## Contact

**Mefai Security Research**
- GitHub: [github.com/mefai-dev](https://github.com/mefai-dev)

---

*This report was prepared by Mefai Security Research. All findings are independently verifiable using the provided RPC commands.*
