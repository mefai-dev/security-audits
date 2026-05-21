# LARP Token Security Audit Report

**Project:** LarpScan
**Token:** `LARP` (Larpscan)
**Contract address (CA):** `0xc67415c9b915f27f402ddbc7bb1be1ecc55effff`
**Chain:** BNB Smart Chain (chainId 56)
**Audit window:** 2026-05-21
**Auditor:** MEFAI v5.0 BNB BUILDERS request (internal tooling)
**Scope:** This report covers ONLY the LARP ERC20 token contract and its ownership / treasury / multisig chain. The NFA contract (`0x15b15df…`) and the LarpScan web app are covered in the separate companion report.

---

> **Disclosure scope.** This report is delivered privately to the LarpScan / BNB Builders project team and is not intended for public distribution. The auditor will not publish summarize or share this report in whole or in part with any party outside the project team without prior written consent.

---

## Executive Summary

| Layer | Rating | Headline |
|---|---|---|
| Token contract logic | ✅ **LOW RISK** | Standard ERC20 with Ownable. No mint. No burn. No tax. No blacklist. No pause. No trading toggle. Fixed supply 1 000 000 000 LARP at deploy. |
| Ownership / treasury | ⚠️ **MEDIUM** | Owner is an upgradeable ERC1967 proxy that holds **61.85 %** of total supply (618 493 609 LARP) plus 7 080 BNB. |
| Owner controller | ✅ **MEDIUM LOW** | Proxy is controlled by a **Gnosis Safe v1.3.0** with a **3 of 6** signing threshold. 76 transactions executed to date. |
| Signer operational hygiene | ⚠️ **MEDIUM** | Only 2 of the 6 Safe signers have any onchain activity. 4 signers have nonce = 0 and 0 BNB. If two active signers lose their keys the Safe is recoverable only via the other four cold wallets. |
| Liquidity | ⚠️ **N/A** | No PancakeSwap V2 LARP/WBNB pair exists. No PancakeSwap V3 LARP/WBNB pair exists at any standard fee tier. The token is effectively illiquid on public DEXes at scrape time. |
| Upgradeability of treasury logic | ⚠️ **MEDIUM** | The contract that holds the 618.5 M LARP and 7 080 BNB is itself an upgradeable proxy. The Safe can swap the implementation and change vesting / unlock / distribution behaviour. |
| Token contract upgradeability | ✅ **LOW** | The LARP token itself is **not** a proxy. EIP1967 implementation slot is empty. `proxiableUUID()` reverts. Bytecode is final. |

Overall the LARP token is a clean fixed supply ERC20 with no rug pull functions. The principal risk surface is treasury concentration plus upgradeability of the treasury holder contract behind a 3 of 6 Safe.

---

## 0. Scope & Methodology

| Item | Detail |
|---|---|
| RPC used | `https://bsc-dataseed.binance.org/` (Binance public node) |
| Verification method | Direct EVM JSON RPC (`eth_call` `eth_getCode` `eth_getStorageAt` `eth_getBalance` `eth_getTransactionCount`) |
| Selector resolution | 4byte.directory and api.openchain.xyz |
| Source quotes | When available from BscScan verified contract viewer. Behaviour claims in this report are derived from runtime bytecode and live RPC where not verified |
| Out of scope | NFA contract `0x15b15df…` LarpScan web app Vercel infra Supabase backend |

All claims in this report are independently reproducible. See Appendix A.

---

## 1. Token Contract Inventory

### 1.1 LARP token

| Field | Value | Source |
|---|---|---|
| Address | `0xc67415c9b915f27f402ddbc7bb1be1ecc55effff` | user supplied + verified onchain |
| `name()` | `Larpscan` | `eth_call 0x06fdde03` |
| `symbol()` | `LARP` | `eth_call 0x95d89b41` |
| `decimals()` | `18` | `eth_call 0x313ce567` |
| `totalSupply()` | **1 000 000 000** LARP (`0x033b2e3c9fd0803ce8000000`) | `eth_call 0x18160ddd` |
| `owner()` (Ownable) | `0x5c952063c7fc8610ffdb798152d69f0b9550762b` | `eth_call 0x8da5cb5b` |
| Runtime bytecode | 10 456 bytes | `eth_getCode` length |
| EIP1967 impl slot | `0x0` (not a proxy) | `eth_getStorageAt` |
| `proxiableUUID()` | reverts (not UUPS) | `eth_call 0x52d1902d` |

### 1.2 Function surface (selector probe of bytecode)

Probed against the runtime bytecode. ✓ means the selector is present in the deployed code. `.` means not present.

| Selector | Signature | Present |
|---|---|---|
| `0x18160ddd` | `totalSupply()` | ✓ |
| `0x313ce567` | `decimals()` | ✓ |
| `0x8da5cb5b` | `owner()` | ✓ |
| `0x715018a6` | `renounceOwnership()` | ✓ |
| `0xf2fde38b` | `transferOwnership(address)` | ✓ |
| `0x40c10f19` | `mint(address,uint256)` | **absent** |
| `0x42966c68` | `burn(uint256)` | **absent** |
| `0x5c975abb` | `paused()` | **absent** |
| `0x8456cb59` | `pause()` | **absent** |
| `0xf9f92be4` | `blacklist(address)` | **absent** |
| `0x14e25920` | `setBlacklist(address,bool)` | **absent** |
| `0x6bc87c3a` | `maxTxAmount()` | **absent** |
| `0xd2d7ad83` | `maxWalletAmount()` | **absent** |
| `0x06bc89db` | `buyTax()` | **absent** |
| `0x52fc7be1` | `sellTax()` | **absent** |
| `0x53a47bb7` | `swapEnabled()` | **absent** |
| `0x8a8c523c` | `tradingEnabled()` | **absent** |
| `0x4f7041a5` | `swapAndLiquify(uint256)` | **absent** |
| `0x437823ec` | `excludeFromFee(address)` | **absent** |

**Live mint probe.** Calling `mint(address,uint256)` from a random EOA (`0x…bEEF`) via `eth_estimateGas` returns `execution reverted: 0x` (no mint function exists). Supply cannot grow.

**Live storage map (verified slots).**

```
slot 0 = 0x0      (initializable / lock flag empty)
slot 1 = 0x0
slot 2 = 0x033b2e3c9fd0803ce8000000   ← _totalSupply = 1e27 wei
slot 3 = 0x4c6172707363616e...0010    ← _name = "Larpscan" (length 0x10 / 2 = 8)
slot 4 = 0x4c41525000...0008          ← _symbol = "LARP" (length 0x08 / 2 = 4)
slot 5 = 0x...5c952063c7fc8610ffdb798152d69f0b9550762b   ← _owner
slot 6 = 0x1
```

**Conclusion.** This is a vanilla OpenZeppelin style fixed supply ERC20 with Ownable. No fee logic. No mint or burn. No pause. No blacklist. No max tx or max wallet. Cannot be upgraded.

---

## 2. Ownership Chain

### 2.1 Layer 1 — Owner of LARP

Address: `0x5c952063c7fc8610ffdb798152d69f0b9550762b`

| Field | Value |
|---|---|
| Type | Contract (not EOA) |
| Pattern | ERC1967 proxy. Storage slot `0x360894…d12a0d5d` returns the implementation address |
| Implementation | `0x7f9411ea1a34b0b0d91a54b4776d00e78a329bbf` (24 520 bytes of bytecode) |
| EIP1967 admin slot | `0x0` (admin is the proxy owner via delegated logic not a separate ProxyAdmin) |
| Initialized flag (slot 0) | `0x1` (initialize executed) |
| BNB balance | **7 080.00 BNB** |
| LARP balance | **618 493 609 LARP** ≈ **61.85 %** of total supply |
| `owner()` delegated | `0x161dd0cfdb0aa25e2504f725655ef9b799375b71` (the Gnosis Safe) |

This contract is the project treasury / vesting / distribution vault. It is upgradeable.

### 2.2 Layer 2 — Gnosis Safe controlling the treasury proxy

Address: `0x161dd0cfdb0aa25e2504f725655ef9b799375b71`

| Field | Value | Selector |
|---|---|---|
| `VERSION()` | `1.3.0` | `0xffa1ad74` |
| `getThreshold()` | **3** | `0xe75235b8` |
| `getOwners()` count | **6** | `0xa0e67e2b` |
| `nonce()` (executed tx count) | **76** | `0xaffed0e0` |

Safe signers (6 EOAs all distinct):

| # | Signer address | BNB | Tx nonce | LARP held directly |
|---|---|---|---|---|
| 1 | `0xa9b9e257ddae54e587b8271d98f7d305cc5c2346` | 0.0000 | 0 | 0 |
| 2 | `0x4682a6915f3c9723251cc514768baf6947a670cd` | 0.2825 | 57 | 0 |
| 3 | `0xd4a89d4941abccb4f866f91a45dac92322e8ebb5` | 0.1983 | 27 | 0 |
| 4 | `0x99950366c89d529ca212081b73cfeefa56e9c89a` | 0.0000 | 0 | 0 |
| 5 | `0x7d122b71b3ca97af60ee69d28a430378b5056003` | 0.0000 | 0 | 0 |
| 6 | `0xe01cd3c97e0a1ef6b45c8beb568f3e0fe7168972` | 0.0000 | 0 | 0 |

Only signers #2 and #3 have any onchain history. Signers #1 #4 #5 #6 have never signed a transaction on BSC from these addresses.

---

## 3. Findings

### 3.1 ✅ T1 Token contract is rug pull free

- No `mint` function. Supply cannot be inflated.
- No `burn` function. Supply cannot be silently reduced.
- No `pause` / `unpause` functions. Trading cannot be frozen.
- No `blacklist` / `setBlacklist`. Holders cannot be denied transfers.
- No transfer tax or `swapAndLiquify`. Fees cannot be silently added.
- No `maxTxAmount` / `maxWalletAmount`. No anti whale tripwires that could be misused.
- No `tradingEnabled` flag. Transfers cannot be globally gated.
- Bytecode is not a proxy. Cannot be upgraded post deploy.
- Only owner controlled functions are `transferOwnership(address)` and `renounceOwnership()`.

**Severity:** **INFO.** This is the strongest possible outcome for a token contract audit.

### 3.2 ⚠️ T2 Treasury concentration. 61.85 % of supply in a single contract

The token owner proxy at `0x5c952063…` holds:
- 618 493 609 LARP (61.85 % of 1 000 000 000 supply)
- 7 080.00 BNB

This is a single point of failure for token distribution. Even though the treasury proxy is multisig gated (§3.3) any Safe quorum compromise drains both balances in a single transaction. The standard mitigations are:

- Documented vesting schedule with onchain cliffs that the proxy enforces.
- Movement of unlocked tranches to time locked sub vaults rather than holding the entire pool in one contract.
- A second independent watchdog Safe with a veto path on outbound transfers above a threshold.

None of these are visible from the proxy bytecode alone. Either disclose the vesting contract logic or migrate to a vested escrow.

**Severity:** **MEDIUM.** Standard for an early stage launched token but worth disclosing publicly so holders know the unlock schedule.

### 3.3 ✅ T3 Treasury is gated by a 3 of 6 Gnosis Safe (v1.3.0)

Positive finding. The proxy treasury is owned by a Gnosis Safe v1.3.0 with 6 signers and a threshold of 3. This is materially stronger than a single EOA. The Safe has executed 76 transactions which means the multisig is operational and signers know how to sign.

**Severity:** **INFO.** This is the correct pattern. The other concerns below are about hygiene not architecture.

### 3.4 ⚠️ T4 Four of six Safe signers have never signed a transaction

| Signer | Nonce | BNB | Status |
|---|---|---|---|
| #1 `0xa9b9e257…` | 0 | 0 | cold / unused |
| #2 `0x4682a691…` | 57 | 0.28 | active |
| #3 `0xd4a89d49…` | 27 | 0.20 | active |
| #4 `0x99950366…` | 0 | 0 | cold / unused |
| #5 `0x7d122b71…` | 0 | 0 | cold / unused |
| #6 `0xe01cd3c9…` | 0 | 0 | cold / unused |

A Safe with threshold 3 and only 2 active signers is effectively dependent on signers #2 and #3 plus exactly one of the cold signers being able to come back online when needed. If the two active signers lose their hardware wallets or seed phrases the Safe is unrecoverable without coordinating four cold signers that have never broadcasted a BSC tx and whose readiness is unknown.

This is not exploitable but is an operational fragility. Two recommended mitigations:

1. Periodically rotate signing duty so every signer broadcasts at least one Safe tx per quarter and proves they still control the key.
2. Optionally raise the threshold to 4 of 6 once all signers are proven active. This forces ongoing key custody hygiene.

**Severity:** **MEDIUM (operational).**

### 3.5 ⚠️ T5 Treasury contract is upgradeable

The treasury at `0x5c952063…` is an ERC1967 proxy. The Safe can call `upgradeTo` (or `upgradeToAndCall`) on the implementation slot via the standard UUPS path and replace the 24 520 byte implementation with anything.

In practice the upgrade requires 3 of 6 Safe signatures so the risk is bounded by the Safe quorum. But the unlimited upgradability means that any current claim about vesting unlocks fee splits or distribution schedule can be unilaterally rewritten by a Safe quorum at any time.

Possible mitigation: put a `TimelockController` between the Safe and the proxy admin function so any upgrade has a public delay (for example 48 h) during which holders can observe and react.

**Severity:** **MEDIUM (governance).**

### 3.6 ⚠️ T6 No public DEX liquidity at scrape time

- PancakeSwap V2 (factory `0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73`) `getPair(LARP WBNB)` returns `0x0`.
- PancakeSwap V3 (factory `0x0BFbCF9fa4f9C56B0F40a671Ad40E0805A091865`) `getPool(LARP WBNB fee)` returns `0x0` at fee tiers 100 / 500 / 2500 / 10000.

The token has no PancakeSwap liquidity at scrape time. It is therefore either pre launch traded on a non Pancake venue or held purely in the treasury with no public market. This is informational rather than a vulnerability. Buyers seeing this token on a price aggregator should be aware that any quote not sourced from a verified pool may be misleading.

**Severity:** **INFO.**

### 3.7 ℹ️ T7 LARP token contract is verified bytecode but verification status on BscScan should be confirmed

The bytecode at `0xc67415…` is consistent with OpenZeppelin ERC20 + Ownable plus initialization helpers. Selector inventory matches a canonical OZ ERC20 base. Project team should ensure the source is verified on BscScan so holders can read the constructor arguments (initial supply destination) and the licence string.

**Severity:** **INFO.**

---

## 4. Risk Matrix

| Domain | Risk | Evidence |
|---|---|---|
| Token logic (rug pull surface) | ✅ LOW | No mint no burn no tax no pause no blacklist (T1) |
| Token upgradeability | ✅ LOW | Not a proxy. Bytecode is final (T1) |
| Treasury concentration | ⚠️ MEDIUM | 61.85 % of supply in one contract (T2) |
| Treasury logic upgradeability | ⚠️ MEDIUM | ERC1967 proxy. Safe can swap impl (T5) |
| Governance key control | ✅ MEDIUM LOW | 3 of 6 Gnosis Safe v1.3.0 (T3) |
| Signer operational hygiene | ⚠️ MEDIUM | 4 of 6 signers have nonce 0 (T4) |
| Liquidity | ⚠️ INFO | No PCS V2 or V3 LARP/WBNB pair (T6) |
| Source verification | ℹ️ INFO | Confirm BscScan verified status (T7) |

---

## 5. Recommendations

1. Publish the vesting / distribution schedule for the 618.5 M LARP held in the treasury. Include cliff dates per tranche and the address each tranche unlocks to.
2. Add a `TimelockController` (recommended minimum 48 h) between the Gnosis Safe and the `upgradeTo` admin path on the treasury proxy so any treasury logic change is publicly observable before execution.
3. Rotate signing duty across all 6 Safe signers periodically. Every signer should broadcast at least one Safe transaction per quarter to prove ongoing key custody. Document this rotation publicly.
4. Optionally raise Safe threshold from 3 to 4 once all signers are operational.
5. Move the 7 080 BNB held by the treasury to a separate cold wallet or a dedicated stablecoin treasury Safe so that liquidity operations and treasury custody are not in the same upgradeable contract.
6. Confirm or initiate BscScan source code verification for the LARP token. Holders should be able to read the source and constructor arguments.
7. Once a public DEX pool is created publish the pool address and the initial liquidity provider address. Until then any third party price feed for LARP should be treated as unverified.

---

## 6. Final verdict

- **Token contract:** ✅ **CLEAN.** Fixed supply ERC20 with no abusive features. Not upgradeable. Standard Ownable pattern.
- **Ownership:** ✅ **MULTISIG GATED.** 3 of 6 Gnosis Safe v1.3.0. 76 transactions executed.
- **Treasury:** ⚠️ **CONCENTRATED AND UPGRADEABLE.** 61.85 % of supply plus 7 080 BNB sit in a single ERC1967 proxy that the Safe can upgrade.
- **Operational hygiene:** ⚠️ Four of six Safe signers have never signed a BSC tx. Resilience depends on two active signers.
- **Liquidity:** ⚠️ No PancakeSwap V2 or V3 LARP/WBNB pair at scrape time. Token is illiquid on public venues.

The LARP token itself is among the safest token contracts you can ship. The remaining risk is operational (signer activity) and governance (treasury upgradeability and concentration). All of these can be addressed without touching the token bytecode.

---

## Appendix A. Reproducibility commands

All numeric claims in this report can be reproduced with `curl` + a JSON parser of your choice. Replace `$RPC` with any BSC mainnet RPC endpoint.

### A.1 Token public state
```bash
RPC=https://bsc-dataseed.binance.org/
CA=0xc67415c9b915f27f402ddbc7bb1be1ecc55effff

# name() symbol() decimals() totalSupply() owner()
for SEL in 0x06fdde03 0x95d89b41 0x313ce567 0x18160ddd 0x8da5cb5b; do
  curl -s -X POST -H "Content-Type: application/json" --data \
    "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"eth_call\",\"params\":[{\"to\":\"$CA\",\"data\":\"$SEL\"},\"latest\"]}" $RPC
  echo
done
```

### A.2 Token has no mint function (live revert proof)
```bash
CA=0xc67415c9b915f27f402ddbc7bb1be1ecc55effff
DATA='0x40c10f19000000000000000000000000000000000000000000000000000000000000bEEF0000000000000000000000000000000000000000000000000000000000000001'
curl -s -X POST -H "Content-Type: application/json" --data \
  "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"eth_estimateGas\",\"params\":[{\"from\":\"0x000000000000000000000000000000000000bEEF\",\"to\":\"$CA\",\"data\":\"$DATA\"}]}" $RPC
# expected: {"error":{"message":"execution reverted: 0x"}}
```

### A.3 Owner is a proxy. Walk to implementation
```bash
OWNER=0x5c952063c7fc8610ffdb798152d69f0b9550762b

# EIP1967 implementation slot
curl -s -X POST -H "Content-Type: application/json" --data \
  "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"eth_getStorageAt\",\"params\":[\"$OWNER\",\"0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc\",\"latest\"]}" $RPC
# expected: 0x...7f9411ea1a34b0b0d91a54b4776d00e78a329bbf
```

### A.4 Owner proxy delegates owner() to a Safe
```bash
OWNER=0x5c952063c7fc8610ffdb798152d69f0b9550762b
curl -s -X POST -H "Content-Type: application/json" --data \
  "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"eth_call\",\"params\":[{\"to\":\"$OWNER\",\"data\":\"0x8da5cb5b\"},\"latest\"]}" $RPC
# expected: 0x...161dd0cfdb0aa25e2504f725655ef9b799375b71
```

### A.5 Confirm Safe v1.3.0 threshold 3 of 6
```bash
SAFE=0x161dd0cfdb0aa25e2504f725655ef9b799375b71

# VERSION()
curl -s -X POST -H "Content-Type: application/json" --data \
  "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"eth_call\",\"params\":[{\"to\":\"$SAFE\",\"data\":\"0xffa1ad74\"},\"latest\"]}" $RPC
# expected: returns string "1.3.0"

# getThreshold()
curl -s -X POST -H "Content-Type: application/json" --data \
  "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"eth_call\",\"params\":[{\"to\":\"$SAFE\",\"data\":\"0xe75235b8\"},\"latest\"]}" $RPC
# expected: 0x...03

# getOwners()
curl -s -X POST -H "Content-Type: application/json" --data \
  "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"eth_call\",\"params\":[{\"to\":\"$SAFE\",\"data\":\"0xa0e67e2b\"},\"latest\"]}" $RPC
# expected: returns 6 addresses
```

### A.6 Treasury holdings
```bash
RPC=https://bsc-dataseed.binance.org/
CA=0xc67415c9b915f27f402ddbc7bb1be1ecc55effff
OWNER=0x5c952063c7fc8610ffdb798152d69f0b9550762b

# BNB balance
curl -s -X POST -H "Content-Type: application/json" --data \
  "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"eth_getBalance\",\"params\":[\"$OWNER\",\"latest\"]}" $RPC

# LARP balance via balanceOf(owner)
curl -s -X POST -H "Content-Type: application/json" --data \
  "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"eth_call\",\"params\":[{\"to\":\"$CA\",\"data\":\"0x70a082310000000000000000000000005c952063c7fc8610ffdb798152d69f0b9550762b\"},\"latest\"]}" $RPC
```

### A.7 No PancakeSwap LP exists for LARP/WBNB
```bash
# PCS V2 factory.getPair(LARP, WBNB)
curl -s -X POST -H "Content-Type: application/json" --data \
  '{"jsonrpc":"2.0","id":1,"method":"eth_call","params":[{"to":"0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73","data":"0xe6a43905000000000000000000000000c67415c9b915f27f402ddbc7bb1be1ecc55effff000000000000000000000000bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c"},"latest"]}' $RPC
# expected: 0x...0  (no pair)
```

---

*End of report.*
