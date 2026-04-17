# Security Audit Report: Ensoul - BNB Smart Chain

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-03-25 |
| **Contract Address** | `0xf671f96f7763e88ea92ff7db79d57c0ee3c7ffff` |
| **Chain** | BNB Smart Chain (BSC) |
| **Language** | Solidity |
| **Audit Type** | Smart Contract + Token |
| **Deployment** | Four.Meme Launchpad (Advanced Token) |
| **Mefai Security Score** | **88/100** |
| **Overall Risk** | **LOW-MEDIUM** |

---

## Disclaimer

This report represents a point-in-time security assessment conducted by Mefai Security Research. The findings and recommendations contained herein are based on the information available and the state of the codebase at the time of the audit. This report does not constitute a guarantee that the audited system is free of vulnerabilities or defects. No part of this report should be considered as investment advice, an endorsement, or a recommendation regarding the security of any project, token, or protocol.

Mefai Security Research assumes no liability for any losses, damages, or adverse consequences resulting from the use of or reliance on this report. The responsibility for implementing fixes and maintaining security lies solely with the project team.

---

## 1. Contract Overview

| Field | Value |
|-------|-------|
| **Token Name** | Ensoul |
| **Token Symbol** | Ensoul |
| **Decimals** | 18 |
| **Total Supply** | 1,000,000,000 (fixed) |
| **Verified Source** | Yes (BscScan) |
| **Proxy** | No - not upgradeable |
| **Ownership** | **RENOUNCED** - verified on-chain (`owner()` = `0x0000000000000000000000000000000000000000`) |
| **Deployment** | Four.Meme Launchpad - Advanced Token (with fee & dividend system) |
| **Fee Rate** | 3% (immutable - ownership renounced) |
| **Fee Distribution** | 100% to founder wallet |
| **Founder Wallet** | `0x517a456a7c4040c7a861d59349de3da43fb10302` |
| **Contract BNB Balance** | 0 BNB |
| **Contract Token Balance** | ~330,410 Ensoul (accumulated fees) |

---

## 2. Security Assessment Summary

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 3 |
| Informational | 5 |

### Overall Risk: **LOW-MEDIUM**

The contract is secure from a technical standpoint. Ownership is renounced, LP is 100% burned, no minting exists, and the contract is not upgradeable. The LOW-MEDIUM rating (rather than LOW) is due to the 3% transfer fee directed entirely to the founder wallet, and missing BscScan profile metadata.

---

## 3. Architecture Analysis

The contract was deployed via the **Four.Meme** launchpad using the **Advanced Token** template. This is a more feature-rich version of the standard Four.Meme token, including a built-in fee system with burn, liquidity, founder, and holder dividend distribution channels.

### Key Architecture Points

- **Fixed supply:** 1 billion tokens minted during `init()`. No mint function exists.
- **Ownership:** **RENOUNCED** - all admin functions permanently disabled.
- **No proxy:** EIP-1967 proxy slot is empty. Code is permanent.
- **Fee system:** 3% transfer fee, currently configured to send 100% to the founder wallet. Fee parameters are immutable (ownership renounced).
- **Dividend system:** Built-in holder fee distribution via `claimFee()` mechanism, but currently inactive (`totalShares = 0`, `feeHolder = 0`).
- **PancakeSwap integration:** Direct DEX integration with `PANCAKE_ROUTER()` and `PANCAKE_FACTORY()`.

### Fee Configuration (Immutable)

| Parameter | Value | Description |
|-----------|-------|-------------|
| `feeRate()` | 300 (3%) | Total fee on transfers |
| `rateBurn()` | 0 (0%) | Portion of fee burned |
| `rateLiquidity()` | 0 (0%) | Portion of fee added to LP |
| Founder share | 100% | Remaining fee goes to founder wallet |
| `founder()` | `0x517a456a7c4040c7a861d59349de3da43fb10302` | Fee recipient |
| `feeAccumulated()` | ~330,410 Ensoul | Total fees collected to date |

All fee parameters are **permanently locked** because they require `onlyOwner` to modify and ownership is renounced.

### Detected Functions

| Function | Selector | Type |
|----------|----------|------|
| `name()` / `symbol()` / `decimals()` / `totalSupply()` | Standard | ERC20 |
| `balanceOf()` / `transfer()` / `transferFrom()` / `approve()` / `allowance()` | Standard | ERC20 |
| `increaseAllowance()` / `decreaseAllowance()` | Standard | ERC20 |
| `owner()` / `renounceOwnership()` / `transferOwnership()` | Standard | Ownable |
| `init()` | `0x2eabc917` | Four.Meme Initializer |
| `_mode()` / `setMode()` / `MODE_NORMAL()` / `MODE_TRANSFER_RESTRICTED()` / `MODE_TRANSFER_CONTROLLED()` | Four.Meme | Transfer mode (locked at Normal) |
| `feeRate()` / `feeBurn()` / `feeLiquidity()` / `feeFounder()` / `feeHolder()` | Four.Meme | Fee tracking |
| `rateBurn()` / `rateLiquidity()` | Four.Meme | Fee distribution rates |
| `feeAccumulated()` / `feePerShare()` | Four.Meme | Fee accounting |
| `claimFee()` / `claimableFee()` / `claimedFee()` | Four.Meme | Holder dividend system |
| `founder()` / `pair()` / `WETH()` / `PANCAKE_ROUTER()` / `PANCAKE_FACTORY()` | Four.Meme | Configuration |
| `totalShares()` / `userCount()` / `userInfo()` / `users()` / `minShare()` | Four.Meme | Dividend tracking |
| `addLiquidity()` / `swapForETH()` | Four.Meme | DEX operations |
| `isBlacklisted()` | Four.Meme | Blacklist check (locked - ownership renounced) |
| `DEAD()` / `quote()` | Four.Meme | Constants |

---

## 4. Security Checklist

| Check | Status | Details |
|-------|--------|---------|
| **Ownership** | SAFE | Renounced - `owner()` returns zero address. All admin functions permanently disabled. |
| **Minting** | SAFE | No mint function. Fixed 1B supply forever. |
| **Proxy/Upgrade** | SAFE | No proxy pattern. Contract code is permanent. |
| **Fee Immutability** | SAFE | Fee rate (3%) and distribution are locked. Cannot be changed (ownership renounced). |
| **Blacklist** | SAFE (Locked) | `isBlacklisted()` view exists but no addresses can be added (ownership renounced). |
| **Transfer Mode** | SAFE (Locked) | Permanently set to Normal (0). |
| **LP Security** | SAFE | All LP tokens burned to dead address. |
| **Dividend System** | SAFE | Built-in but inactive (`totalShares = 0`). No risk. |

---

## 5. Findings

### Finding #1: 3% Transfer Fee to Founder Wallet (Low)

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Fee Mechanics |
| **Status** | By Design |

**Description:**

The contract charges a 3% fee on transfers (`feeRate = 300`). The burn rate and liquidity rate are both 0%, meaning 100% of the collected fee is directed to the founder wallet (`0x517a456a7c4040c7a861d59349de3da43fb10302`). As of the audit date, approximately 330,410 Ensoul tokens have been accumulated as fees.

The fee rate and distribution are **immutable** because ownership has been renounced. The 3% fee cannot be increased, decreased, or redirected.

**Impact:**

Users should be aware that every transfer incurs a 3% fee. This affects DeFi integrations and DEX swap slippage. The fee is standard for the Four.Meme Advanced Token template and is within normal range for BSC community tokens.

---

### Finding #2: Missing Token Logo and BscScan Metadata (Low)

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Transparency / Metadata |
| **Status** | Open |

**Description:**

The token does not have a logo/icon registered on BscScan, and BscScan token profile information (website, social links, description) is not filled in. While this has no security impact, it reduces visibility and trust for users verifying the token through block explorers.

**Recommendation:**

Submit a token information update request to BscScan to add a logo, website, and social media links.

---

### Finding #3: Fee-on-Transfer DeFi Integration Note (Low)

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | DeFi Integration |
| **Status** | By Design |

**Description:**

The 3% fee means DeFi protocols that don't handle fee-on-transfer tokens may experience accounting discrepancies. Users should set appropriate slippage (at least 3-5%) on DEX swaps.

---

### Finding #4: Ownership Renounced (Informational - Positive)

| Attribute | Value |
|-----------|-------|
| **Severity** | Informational |
| **Type** | Positive Security Feature |
| **Status** | Verified On-Chain |

**Description:**

`owner()` returns `0x0000000000000000000000000000000000000000`. All `onlyOwner` functions are permanently disabled including `setMode()`, `init()`, fee parameter changes, blacklist management, and `transferOwnership()`. No admin actions are possible.

---

### Finding #5: LP Tokens Burned (Informational - Positive)

| Attribute | Value |
|-----------|-------|
| **Severity** | Informational |
| **Type** | Positive Security Feature |
| **Status** | Verified On-Chain |

**Description:**

PancakeSwap V2 pair: `0xE23045b5Bbf338Cda9e9FbC07637A1523aE318A0`

| Metric | Value |
|--------|-------|
| LP Total Supply | ~59,397 |
| LP at Dead Address | ~59,397 (100%) |
| Reserve (WBNB) | ~33.34 WBNB |
| Reserve (Ensoul) | ~114,421,915 Ensoul |

All LP tokens have been sent to the dead address (`0x...dEaD`), making the liquidity permanently locked. Rug-pull via LP removal is impossible.

---

### Finding #6: Not a Proxy Contract (Informational - Positive)

| Attribute | Value |
|-----------|-------|
| **Severity** | Informational |
| **Type** | Positive Security Feature |
| **Status** | Verified On-Chain |

**Description:**

EIP-1967 implementation slot is empty. The contract is not upgradeable. Deployed bytecode is permanent.

---

### Finding #7: No Mint Function (Informational - Positive)

| Attribute | Value |
|-----------|-------|
| **Severity** | Informational |
| **Type** | Positive Security Feature |
| **Status** | Verified On-Chain |

**Description:**

No `mint()` function exists. The `init()` initializer is permanently locked behind `onlyOwner` (zero address). Total supply is fixed at 1,000,000,000 Ensoul forever.

---

### Finding #8: Blacklist and Transfer Mode Permanently Locked (Informational - Positive)

| Attribute | Value |
|-----------|-------|
| **Severity** | Informational |
| **Type** | Positive Security Feature |
| **Status** | Verified On-Chain |

**Description:**

The contract contains `isBlacklisted()` and a transfer mode system (`setMode()`), both standard features of the Four.Meme Advanced Token template. Since ownership is renounced, no address can ever be blacklisted and the transfer mode is permanently set to Normal (0). These features are effectively dead code.

---

## 6. Vulnerability Assessment Matrix

| Category | Status | Notes |
|----------|--------|-------|
| **Reentrancy** | SAFE | Standard Four.Meme template, audited bytecode |
| **Integer Overflow** | SAFE | Solidity 0.8.x built-in protection |
| **Access Control** | SAFE | Ownership renounced - no admin functions callable |
| **Front-Running** | Standard | Standard ERC20 approve race condition |
| **Flash Loan** | N/A | No oracles or leverage mechanics |
| **Proxy/Upgrade** | SAFE | Not upgradeable |
| **Centralization** | SAFE | Ownership renounced |
| **Fee Manipulation** | SAFE | Fee rate (3%) is immutable, cannot be changed |
| **Supply Inflation** | SAFE | No mint function, fixed 1B supply |
| **LP Rug Pull** | SAFE | All LP tokens burned |
| **Blacklist** | SAFE | Ownership renounced - cannot add addresses |

---

## 7. Mefai Security Score

### **88/100 - LOW-MEDIUM RISK**

| Category | Check | Result | Score |
|----------|-------|--------|-------|
| Ownership & Access Control | `owner()` = zero address | Renounced | **20/20** |
| Supply & Minting | No `mint()` function, fixed 1B | No minting possible | **20/20** |
| Liquidity & LP Security | All LP burned to dead address | Permanently locked | **20/20** |
| Code & Program Safety | Verified source, Four.Meme Advanced standard, 0 medium+ findings | Clean | **15/15** |
| Fee & Transfer Mechanics | 3% immutable fee, 100% to founder wallet, cannot be changed | Fixed but notable | **10/15** |
| Transparency & Metadata | Verified source, but no token logo, no BscScan profile info | Partial | **3/10** |
| **TOTAL** | | | **88/100** |

> Scoring methodology: [SCORING.md](../SCORING.md)

This contract demonstrates strong security practices:

1. **Ownership renounced** - no admin functions can be called
2. **LP 100% burned** - liquidity permanently locked, rug-pull impossible
3. **No minting** - fixed supply, no inflation
4. **No proxy** - code cannot be changed
5. **Fee immutable** - 3% rate cannot be increased or decreased
6. **Blacklist locked** - cannot blacklist any address
7. **Transfer mode locked** - permanently set to Normal
8. **Standard Four.Meme Advanced Token** - widely deployed and tested bytecode

There are **no critical, high, or medium severity findings**. The three low-severity findings relate to the 3% fee structure (by design), missing BscScan metadata, and standard fee-on-transfer DeFi considerations.

**This contract is safe for token holders.** The deployer has renounced ownership and burned all LP tokens. The 3% transfer fee is the primary consideration - it is immutable and directed to the founder wallet.

---

## 8. Liquidity & Market Data

| Metric | Value | Source |
|--------|-------|--------|
| DEX | PancakeSwap V2 | Factory verification |
| Pair Address | `0xE23045b5Bbf338Cda9e9FbC07637A1523aE318A0` | On-chain |
| Token0 (WBNB) | `0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c` | On-chain |
| Token1 (Ensoul) | `0xf671f96f7763e88ea92ff7db79d57c0ee3c7ffff` | On-chain |
| Reserve (WBNB) | ~33.34 | On-chain |
| Reserve (Ensoul) | ~114,421,915 | On-chain |
| LP Total Supply | ~59,397 | On-chain |
| LP Burned | ~59,397 (100%) | On-chain - dead address |
| Price (USD) | ~$0.0001883 | DexScreener |
| Liquidity (USD) | ~$43,102 | DexScreener |
| FDV | ~$188,349 | DexScreener |
| Volume 24h | ~$6,971 | DexScreener |
| Pair Created | 2026-03-02 | DexScreener |

---

## Findings Summary

| ID | Title | Severity | Status |
|----|-------|----------|--------|
| F-001 | 3% Transfer Fee to Founder Wallet | Low | By Design |
| F-002 | Missing Token Logo and BscScan Metadata | Low | Open |
| F-003 | Fee-on-Transfer DeFi Integration Note | Low | By Design |
| F-004 | Ownership Renounced | Informational | Positive |
| F-005 | LP Tokens Burned | Informational | Positive |
| F-006 | Not a Proxy Contract | Informational | Positive |
| F-007 | No Mint Function | Informational | Positive |
| F-008 | Blacklist and Transfer Mode Permanently Locked | Informational | Positive |

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

## Appendix B: On-Chain Verification Commands

```bash
# Owner verification
cast call 0xf671f96f7763e88ea92ff7db79d57c0ee3c7ffff "owner()(address)" --rpc-url https://bsc-dataseed.binance.org/
# Returns: 0x0000000000000000000000000000000000000000

# Fee rate
cast call 0xf671f96f7763e88ea92ff7db79d57c0ee3c7ffff "feeRate()(uint256)" --rpc-url https://bsc-dataseed.binance.org/
# Returns: 300 (3%)

# Founder wallet
cast call 0xf671f96f7763e88ea92ff7db79d57c0ee3c7ffff "founder()(address)" --rpc-url https://bsc-dataseed.binance.org/
# Returns: 0x517a456a7c4040c7a861d59349de3da43fb10302

# Mode verification
cast call 0xf671f96f7763e88ea92ff7db79d57c0ee3c7ffff "_mode()(uint256)" --rpc-url https://bsc-dataseed.binance.org/
# Returns: 0 (MODE_NORMAL)

# LP burned verification
cast call 0xE23045b5Bbf338Cda9e9FbC07637A1523aE318A0 "balanceOf(address)(uint256)" 0x000000000000000000000000000000000000dEaD --rpc-url https://bsc-dataseed.binance.org/
# Returns: ~59396969616568150000000 (100% of LP supply)

# EIP-1967 proxy check
cast storage 0xf671f96f7763e88ea92ff7db79d57c0ee3c7ffff 0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc --rpc-url https://bsc-dataseed.binance.org/
# Returns: 0x0 (not a proxy)
```

---

## Appendix C: Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| BSC RPC (Direct) | N/A | On-chain state verification |
| Bytecode Selector Extraction | Custom | Function interface discovery |
| OpenChain Signature Database | N/A | Function selector resolution |
| DexScreener API | N/A | Market data and pair verification |
| PancakeSwap V2 Factory | On-chain | Pair address verification |
| Manual review | N/A | Architecture and security analysis |

---

## Appendix D: Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-03-25 | Mefai Security Research | Initial report |

---

## Contact

**Mefai Security Research**
- Web: [mefai.io](https://mefai.io)
- GitHub: [github.com/mefai-dev](https://github.com/mefai-dev)

---

*This report was prepared by Mefai Security Research. All on-chain data verified via direct BSC RPC calls on 2026-03-25. Unauthorized distribution or modification of this document is prohibited without prior written consent.*
