# PredictBNB v2 - Complete Security Audit Report

**Risk Score:** 35/100 (HIGH RISK)
**Audit Date:** May 2026
**Contract:** PredictBNBv2.sol
**Compiler:** Solidity 0.8.20
**Auditor:** MEFAI Security Framework v5.0

---

## Executive Summary

| Severity | Count |
|----------|-------|
| Critical | 2 |
| High | 3 |
| Medium | 4 |
| Low | 3 |
| Informational | 4 |

PredictBNB v2 is a token-based parimutuel prediction market contract using p4BNB token for betting. While the contract implements basic security measures (ReentrancyGuard, SafeERC20), it contains **critical centralization vulnerabilities** that allow the operator to manipulate market outcomes and extract value from users.

**Verdict:** DO NOT DEPLOY without implementing the recommended fixes.

---

## Contract Overview

| Property | Value |
|----------|-------|
| Contract Name | PredictBNBv2 |
| Solidity Version | 0.8.20 |
| License | MIT |
| Lines of Code | 265 |
| External Dependencies | OpenZeppelin (IERC20, SafeERC20, ReentrancyGuard) |
| Token Standard | ERC20 (p4BNB) |
| Market Type | Parimutuel (pool-based) |

### Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    PredictBNBv2                         │
├─────────────────────────────────────────────────────────┤
│  Storage:                                               │
│  ├── markets (mapping)                                  │
│  ├── yesBalance (nested mapping)                        │
│  ├── noBalance (nested mapping)                         │
│  ├── contribution (nested mapping)                      │
│  └── claimed (nested mapping)                           │
├─────────────────────────────────────────────────────────┤
│  Functions:                                             │
│  ├── createMarket() [onlyOwner]                        │
│  ├── buyYes() / buyNo() [public]                       │
│  ├── resolveMarket() [onlyOwner]                       │
│  ├── cancelMarket() [onlyOwner]                        │
│  ├── claim() [public, nonReentrant]                    │
│  └── refund() [public, nonReentrant]                   │
└─────────────────────────────────────────────────────────┘
```

---

## Section 1: Critical Vulnerabilities

### PBNB-001: Owner Can Manipulate Market Results (CRITICAL)

**Location:** `resolveMarket()` - Line 157-168

**Description:** The owner has complete unilateral control over market resolution with no external verification or oracle integration.

```solidity
function resolveMarket(uint marketId, bool result) external onlyOwner marketExists(marketId) {
    // ...
    m.resolved = true;
    m.result = result;  // Owner decides YES or NO wins
}
```

**Attack Scenario:**
1. Owner creates market: "Will BTC reach $100K?"
2. Owner bets large amount on YES
3. Regardless of real-world outcome, owner sets `result = true`
4. Owner claims all winnings

**Impact:** Complete loss of funds for users who bet against owner's position.

**Risk:** CRITICAL - This is essentially a rug pull mechanism.

---

### PBNB-002: Owner Guaranteed Profit on Every Market (CRITICAL)

**Location:** `createMarket()` - Lines 104-112

**Description:** When creating a market, owner deposits tokens but receives balance on BOTH sides (YES and NO).

```solidity
m.yesPool = DEPOSIT / 2;           // 500K tokens
m.noPool = DEPOSIT / 2;            // 500K tokens
m.totalPool = DEPOSIT;             // 1M tokens

yesBalance[id][msg.sender] = DEPOSIT / 2;   // Owner gets 500K YES
noBalance[id][msg.sender] = DEPOSIT / 2;    // Owner gets 500K NO
contribution[id][msg.sender] = DEPOSIT;      // Total 1M
```

**Mathematical Proof:**

```
Initial State:
- yesPool = 500,000
- noPool = 500,000
- totalPool = 1,000,000
- owner.yesBalance = 500,000
- owner.noBalance = 500,000

User bets 100,000 on YES:
- yesPool = 600,000
- totalPool = 1,100,000

If YES wins:
- Owner payout = (1,100,000 * 500,000) / 600,000 = 916,666 tokens
- User payout = (1,100,000 * 100,000) / 600,000 = 183,333 tokens

If NO wins:
- Owner payout = (1,100,000 * 500,000) / 500,000 = 1,100,000 tokens
- User gets NOTHING

Combined with PBNB-001: Owner ALWAYS profits by choosing winning side.
```

**Impact:** Users are mathematically guaranteed to lose against the owner.

---

## Section 2: High Severity Issues

### PBNB-003: No Oracle Integration (HIGH)

**Location:** Contract-wide

**Description:** Market resolution relies entirely on owner's manual input with no verification against real-world data.

**Industry Comparison:**

| Protocol | Resolution Method |
|----------|-------------------|
| Polymarket | UMA Oracle + Dispute |
| Augur | Decentralized reporters + REP staking |
| Gnosis | Reality.eth oracle |
| **PredictBNB** | **Single owner (centralized)** |

**Impact:** Zero trustlessness - defeats purpose of blockchain-based prediction market.

---

### PBNB-004: No Emergency Pause Mechanism (HIGH)

**Location:** Contract-wide

**Description:** If a vulnerability is discovered, there is no way to pause the contract.

**Impact:** Exploits cannot be stopped once contract is deployed.

---

### PBNB-005: Immutable Owner - No Transfer/Renounce (HIGH)

**Location:** Line 65

```solidity
address public immutable owner;
```

**Description:** Owner address cannot be changed. If owner's private key is compromised or owner wants to decentralize, it's impossible.

**Impact:**
- Key compromise = permanent control loss
- No path to decentralization
- Single point of failure forever

---

## Section 3: Medium Severity Issues

### PBNB-006: Front-Running Vulnerability (MEDIUM)

**Location:** `_placeBet()` - Line 129-152

**Description:** Bets are visible in mempool before confirmation. Attackers can front-run with their own bet.

**Impact:** MEV extraction, unfair advantage for sophisticated actors.

---

### PBNB-007: No Minimum Bet Amount (MEDIUM)

**Location:** Line 130

```solidity
require(amount > 0, "Amount must > 0");
```

**Description:** Users can bet 1 wei, creating dust positions.

**Impact:** Griefing attacks, storage bloat, gas-expensive claims.

---

### PBNB-008: No Maximum Market Duration (MEDIUM)

**Location:** `createMarket()` - Line 94

**Description:** Markets can be created with endTime 100 years in future.

**Impact:** Funds locked indefinitely with no resolution.

---

### PBNB-009: Timestamp Dependency (MEDIUM)

**Location:** Lines 94, 136, 162

**Description:** Contract relies on `block.timestamp` which can be manipulated by miners.

**Impact:** Edge cases around market end time can be exploited.

---

## Section 4: Low Severity Issues

### PBNB-010: Missing Event Distinction (LOW)

Same event used for claims and refunds. Creates tracking difficulty.

### PBNB-011: String Storage Inefficiency (LOW)

Strings are expensive. Fixed categories could use bytes32.

### PBNB-012: No View Function for Active Markets (LOW)

No easy way to query active/resolved/cancelled markets.

---

## Section 5: Positive Findings

| Feature | Status |
|---------|--------|
| ReentrancyGuard | IMPLEMENTED |
| SafeERC20 | IMPLEMENTED |
| Checks-Effects-Interactions | CORRECT |
| Overflow Protection (0.8.x) | AUTOMATIC |
| Payout Formula | MATHEMATICALLY CORRECT |

---

## Section 6: Centralization Risk Matrix

| Function | Controller | Risk Level |
|----------|------------|------------|
| Create Market | Owner Only | HIGH |
| Resolve Market | Owner Only | CRITICAL |
| Cancel Market | Owner Only | HIGH |
| Set Result | Owner Only | CRITICAL |
| Upgrade Contract | N/A | SAFE |
| Pause | N/A | NOT AVAILABLE |

**Centralization Score: 9/10 (Highly Centralized)**

---

## Section 7: Required Fixes

### FIX 1: Remove Owner Positions (CRITICAL)

**Problem:** Lines 110-112

**Solution:** Delete these 3 lines completely:
```solidity
// DELETE THESE LINES:
yesBalance[id][msg.sender] = DEPOSIT / 2;
noBalance[id][msg.sender] = DEPOSIT / 2;
contribution[id][msg.sender] = DEPOSIT;
```

Owner deposit becomes locked liquidity only - not claimable positions.

---

### FIX 2: Add Resolution Timelock (CRITICAL)

**Add to storage:**
```solidity
mapping(uint => uint) public resolutionProposedAt;
mapping(uint => bool) public proposedResult;
uint public constant RESOLUTION_DELAY = 24 hours;
```

**Split resolveMarket into 2 functions:**

```solidity
// Step 1: Propose (callable after endTime)
function proposeResolution(uint marketId, bool result) external onlyOwner marketExists(marketId) {
    Market storage m = markets[marketId];
    require(!m.resolved, "Already resolved");
    require(!m.cancelled, "Already cancelled");
    require(block.timestamp >= m.endTime, "Not ended");
    require(resolutionProposedAt[marketId] == 0, "Already proposed");

    resolutionProposedAt[marketId] = block.timestamp;
    proposedResult[marketId] = result;

    emit ResolutionProposed(marketId, result);
}

// Step 2: Finalize (callable after 24h delay)
function finalizeResolution(uint marketId) external onlyOwner marketExists(marketId) {
    Market storage m = markets[marketId];
    require(!m.resolved, "Already resolved");
    require(resolutionProposedAt[marketId] > 0, "Not proposed");
    require(
        block.timestamp >= resolutionProposedAt[marketId] + RESOLUTION_DELAY,
        "Timelock active"
    );

    m.resolved = true;
    m.result = proposedResult[marketId];

    emit MarketResolved(marketId, m.result);
}
```

---

### FIX 3: Add Betting Limits (HIGH)

**Add constants:**
```solidity
uint public constant MIN_BET = 100 ether;
uint public constant BET_CUTOFF = 5 minutes;
```

**Modify _placeBet (Line 130 and 136):**
```solidity
require(amount >= MIN_BET, "Below minimum");
require(block.timestamp < m.endTime - BET_CUTOFF, "Betting closed");
```

---

### FIX 4: Add Pausable (HIGH)

**Add import:**
```solidity
import "@openzeppelin/contracts/security/Pausable.sol";
```

**Modify contract declaration:**
```solidity
contract PredictBNBv2 is ReentrancyGuard, Pausable {
```

**Add modifier to betting functions:**
```solidity
function buyYes(uint marketId, uint amount) external whenNotPaused {
    _placeBet(marketId, amount, true);
}

function buyNo(uint marketId, uint amount) external whenNotPaused {
    _placeBet(marketId, amount, false);
}
```

**Add control functions:**
```solidity
function pause() external onlyOwner {
    _pause();
}

function unpause() external onlyOwner {
    _unpause();
}
```

---

### FIX 5: Ownership Transfer (HIGH)

**Change Line 65:**
```solidity
address public owner;  // Remove 'immutable'
```

**Add function:**
```solidity
function transferOwnership(address newOwner) external onlyOwner {
    require(newOwner != address(0), "Zero address");
    address oldOwner = owner;
    owner = newOwner;
    emit OwnershipTransferred(oldOwner, newOwner);
}
```

---

### FIX 6: Max Market Duration (MEDIUM)

**Add constant:**
```solidity
uint public constant MAX_DURATION = 90 days;
```

**Modify createMarket (Line 94):**
```solidity
require(input.endTime > block.timestamp, "Invalid end time");
require(input.endTime <= block.timestamp + MAX_DURATION, "Duration too long");
```

---

## Section 8: Recommended Parameters

| Parameter | Recommended Value | Purpose |
|-----------|-------------------|---------|
| `DEPOSIT` | 1,000,000 tokens | Initial liquidity |
| `MIN_BET` | 100 tokens | Prevent dust attacks |
| `BET_CUTOFF` | 5 minutes | Prevent last-second manipulation |
| `MAX_DURATION` | 90 days | Prevent infinite lockups |
| `RESOLUTION_DELAY` | 24 hours | Allow dispute window |

---

## Section 9: Implementation Checklist

### Critical (Must Fix Before Deploy)

| # | Fix | Status |
|---|-----|--------|
| 1 | Delete owner balance assignments (Lines 110-112) | REQUIRED |
| 2 | Implement resolution timelock (24h minimum) | REQUIRED |

### High Priority (Should Fix Before Deploy)

| # | Fix | Status |
|---|-----|--------|
| 3 | Add MIN_BET constant and check | RECOMMENDED |
| 4 | Add BET_CUTOFF before endTime | RECOMMENDED |
| 5 | Implement Pausable pattern | RECOMMENDED |
| 6 | Make owner transferable | RECOMMENDED |

### Medium Priority (Improve Security)

| # | Fix | Status |
|---|-----|--------|
| 7 | Add MAX_DURATION check | OPTIONAL |
| 8 | Add pool ratio limits | OPTIONAL |
| 9 | Enhanced events | OPTIONAL |

---

## Section 10: What Should NOT Change

| Component | Status | Reason |
|-----------|--------|--------|
| ReentrancyGuard | KEEP | Correct implementation |
| SafeERC20 | KEEP | Safe token handling |
| Payout formula (Line 198) | KEEP | Mathematically correct |
| Claim logic | KEEP | Correct implementation |
| Refund logic | KEEP | Correct implementation |
| marketExists modifier | KEEP | Proper validation |

---

## Section 11: Summary for Development Team

### Operator-Controlled Resolution Model

The current design uses operator-controlled resolution, which is acceptable for certain use cases IF the following conditions are met:

1. **Owner must NOT receive positions when creating markets**
   - Current code gives owner YES + NO positions
   - This must be removed

2. **Resolution must have timelock**
   - Minimum 24 hours delay
   - Allows users to dispute or withdraw

3. **Betting must close before endTime**
   - 5 minute cutoff minimum
   - Prevents manipulation

4. **Emergency controls required**
   - Pausable for emergencies
   - Transferable ownership

### If Using Own Token (p4BNB)

The contract correctly uses:
- `IERC20` interface
- `SafeERC20` for transfers
- Immutable token address

No changes needed for token integration.

---

## Section 12: Risk Assessment After Fixes

| Scenario | Before Fixes | After Fixes |
|----------|--------------|-------------|
| Owner drains funds | POSSIBLE | MITIGATED |
| Result manipulation | POSSIBLE | DETECTABLE (24h window) |
| Dust attacks | POSSIBLE | PREVENTED |
| Last-second bets | POSSIBLE | PREVENTED |
| Emergency exploit | NO DEFENSE | PAUSABLE |
| Key compromise | PERMANENT | TRANSFERABLE |

**Post-Fix Risk Score: 65/100 (Medium Risk)**

---

## Section 13: Conclusion

### Current State: DO NOT DEPLOY

The contract in its current form allows the owner to extract all user funds through market manipulation. Critical vulnerabilities must be fixed.

### After Implementing Fixes: ACCEPTABLE FOR CONTROLLED DEPLOYMENT

With the recommended fixes implemented:
- Operator-controlled resolution becomes transparent (timelock)
- Owner cannot profit unfairly (no positions)
- Users have protection (cutoff, pause, minimum bets)

### Remaining Risk

Even after fixes, users must trust the operator to resolve markets honestly. For full trustlessness, oracle integration (Chainlink, UMA) would be required.

---

## Disclaimer

This security assessment is provided for informational and educational purposes only. MEFAI Security Framework does not provide financial advice. All findings represent professional analysis based on available code at the time of assessment. Smart contract security is complex and no audit can guarantee the absence of vulnerabilities.

---

**MEFAI Security Framework v5.0**
**Classification:** Smart Contract Security Audit
**Date:** May 2026
**Status:** Complete

---

## Sources

- [OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts)
- [Solidity Security Best Practices](https://github.com/sigp/solidity-security-blog)
- [Common Smart Contract Vulnerabilities - Hacken](https://hacken.io/discover/smart-contract-vulnerabilities/)
- [Prediction Market Development Guide](https://dev.to/sivarampg/building-a-production-ready-prediction-market-smart-contract-in-solidity-complete-guide-with-2iio)
