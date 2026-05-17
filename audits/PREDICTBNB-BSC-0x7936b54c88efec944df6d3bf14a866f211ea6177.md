# Security Audit Review: PREDICTBNB · BNB Smart Chain

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-05-17 |
| **Contract Address** | `0x7936b54c88efec944df6d3bf14a866f211ea6177` |
| **Contract Name** | `PredictBNBv1` |
| **Chain** | BNB Smart Chain (BSC) |
| **Language** | Solidity (`^0.8.20`) |
| **Audit Type** | Independent Re-Audit of Third-Party Report |
| **Reviewed Report** | Hexific Security · `audit-1778962419504` (2026-05-17 03:13 UTC) |
| **Reviewed Report URL** | `https://predictbnb.fun/contract/audit/audit-report.pdf` |
| **Source Verified (BscScan)** | YES (single file, ~9.4 KB) |
| **Mefai Security Score** | **32/100** |
| **Overall Risk** | **HIGH · CRITICAL CENTRALIZATION** |

---

## Disclaimer

This report represents a point·in·time security assessment conducted by Mefai Security Research. The findings and recommendations contained herein are based on the information available and the state of the codebase at the time of the audit. This report does not constitute a guarantee that the audited system is free of vulnerabilities or defects. No part of this report should be considered as investment advice, an endorsement, or a recommendation regarding the security of any project, token, or protocol.

Mefai Security Research assumes no liability for any losses, damages, or adverse consequences resulting from the use of or reliance on this report. The responsibility for implementing fixes and maintaining security lies solely with the project team.

This document is a technical comparison of publicly available materials (the Hexific PDF and the verified BscScan source code) and does not allege bad faith on the part of any third party named in it.

---

## 1. Contract Overview

| Field | Value |
|-------|-------|
| **Protocol Type** | Binary YES/NO Prediction Market |
| **Resolution Mechanism** | Manual, owner·controlled (no oracle) |
| **Deposit Unit** | 0.05 BNB per bet (fixed) |
| **Compiler** | `pragma solidity ^0.8.20` |
| **Proxy Pattern** | None (single, non·upgradeable contract) |
| **External Imports** | None (no OpenZeppelin, no Chainlink, no SafeERC20) |
| **Ownership** | Single EOA, set in constructor, **non·transferable** |
| **Re·entrancy Guard** | None used (CEI pattern relied upon manually) |
| **Source Lines** | 579 lines (heavily blank·line padded; effective code ~200 lines) |

### Function Map

| Function | Visibility | Access | Notes |
|----------|-----------|--------|-------|
| `createMarket(Input)` | external payable | `onlyOwner` | Owner seeds market with exactly 0.05 BNB |
| `buyYes(uint)` | external payable | open | Wraps `_placeBet(.., true)` |
| `buyNo(uint)` | external payable | open | Wraps `_placeBet(.., false)` |
| `_placeBet(uint,bool)` | internal | n/a | Requires `msg.value == 0.05 BNB` |
| `resolveMarket(uint,bool)` | external | `onlyOwner` | Owner declares winning side as `bool` |
| `cancelMarket(uint)` | external | `onlyOwner` | Owner cancels with no time constraint |
| `claim(uint)` | external | open | Winner withdraws via legacy `.transfer()` |
| `refund(uint)` | external | open | Refund of cancelled markets via `.transfer()` |
| `getPools(uint)` | external view | open | Read·only pool stats |
| `getUserPosition(uint,address)` | external view | open | Read·only user position |
| `receive()` | external payable | open | Empty body; un·accounted BNB |

---

## 2. Hexific Report Re·Review

### 2.1 Reviewed Findings

The Hexific PDF advertises **0 Critical · 0 Major · 0 Medium · 3 Minor · 4 Informational** for a total of 7 items.

| # | Hexific Severity | Hexific Finding | Detector Name |
|---|------------------|-----------------|----------------|
| 1 | Minor | `createMarket` uses `block.timestamp` | `timestamp` |
| 2 | Minor | `resolveMarket` uses `block.timestamp` | `timestamp` |
| 3 | Minor | `_placeBet` uses `block.timestamp` | `timestamp` |
| 4 | Informational | Pragma `^0.8.20` contains known issues | `solc-version` |
| 5 | Informational | Re·entrancy in `refund()` | `reentrancy-unlimited-gas` |
| 6 | Informational | Re·entrancy in `claim()` | `reentrancy-unlimited-gas` |
| 7 | Informational | `owner` should be `immutable` | `immutable-states` |

The detector names map 1·to·1 to Slither's default static·analysis ruleset. The body of the PDF contains code excerpts only; there is no architectural narrative, no economic·model discussion, no oracle assessment and no trust·assumption analysis.

### 2.2 Cross·Check of Hexific Line References

| PDF Reference | On·Chain Reality | Match |
|---------------|-------------------|-------|
| pragma line 2 | `pragma solidity ^0.8.20;` at line 2 | OK |
| `owner` line 103 | `address public owner;` at line 103 | OK |
| `createMarket` 131·185 | function spans exactly lines 131·185 | OK |
| `_placeBet` 224·296 | function spans exactly lines 224·296 | OK |
| `resolveMarket` 301·341 | function spans exactly lines 301·341 | OK |
| `claim` 377·451 | function spans exactly lines 377·451 | OK |
| `refund` 456·500 | function spans exactly lines 456·500 | OK |

Every line·range cited in the Hexific PDF matches the verified BscScan source byte·for·byte. The audit was performed against the same code that is currently deployed.

### 2.3 Mefai Assessment of Each Hexific Item

| Hexific Item | Mefai Re·Assessment |
|--------------|---------------------|
| 3 × `block.timestamp` Minor | Correct but trivial. Miner timestamp drift is bounded to ~15 s and is irrelevant for markets whose `endTime` is hours or days in the future. |
| Re·entrancy in `claim()` | **Mis·classified.** `claim()` follows Checks·Effects·Interactions: `claimed[..][..] = true` at lines 438·441 is set **before** `payable(msg.sender).transfer(payout)` at line 444. There is no exploitable re·entrancy; the real concern is the use of legacy `.transfer()` itself (see F·004). |
| Re·entrancy in `refund()` | Same observation: `claimed[..][..] = true` at lines 487·490 precedes `.transfer(amount)` at line 493. Not re·entrant. |
| `owner` should be `immutable` | **Wrong recommendation.** Making `owner` `immutable` would lock the variable forever, which is the opposite of what the contract needs (see F·003 below). |
| Pragma `^0.8.20` | Generic version warning. Since the bytecode is already deployed, this is not a live risk. |

---

## 3. Security Assessment Summary

### Risk Rating (Mefai Re·Audit, additional to Hexific)

| Severity | Count |
|----------|-------|
| Critical | 3 |
| High | 3 |
| Medium | 3 |
| Low | 2 |
| Informational | 0 net·new |
| **Total** | **11** |

### Overall Risk: **HIGH · CRITICAL CENTRALIZATION**

The contract has no critical implementation bugs in the narrow technical sense (no overflow, no re·entrancy, no signature malleability). The risk is structural: every economic decision is made by a single externally·owned account with no oracle, no time·lock, no multisig, no recovery surface, and no transfer of authority. Combined with legacy payment primitives that break modern wallets, the contract is unsuitable for arms·length users.

---

## 4. Architecture Analysis

### 4.1 Trust Model

The contract has a single trust root: the constructor·assigned `owner`. This single key:

* creates every market (`createMarket` is `onlyOwner`, line 136)
* resolves every market (`resolveMarket` is `onlyOwner`, line 309)
* cancels any market unilaterally (`cancelMarket` is `onlyOwner`, line 350)

There is no oracle (Chainlink, UMA, Pyth, optimistic·oracle pattern), no committee vote, no challenge window, no dispute resolution, no time·lock. The winning side of every market is whatever value the owner passes to `resolveMarket(uint, bool result)`.

### 4.2 Economic Flow

```
createMarket (owner seeds 0.05 BNB)
   ├─ m.yesPool   = 0.025 BNB
   ├─ m.noPool    = 0.025 BNB
   ├─ m.totalPool = 0.05  BNB
   └─ contribution[id][owner] = 0.05 BNB    (creator's stake)

buyYes / buyNo  (each user adds exactly 0.05 BNB)
   ├─ yesBalance[id][user] += 0.05  (or noBalance)
   ├─ m.yesPool             += 0.05  (or noPool)
   ├─ m.totalPool           += 0.05
   └─ contribution[id][user] += 0.05

resolveMarket(id, result)   ← owner only, no oracle
   └─ m.resolved = true; m.result = result

claim   →  payout = totalPool * userBet / winningPool   (rounded down)
refund  →  payout = contribution[id][user]              (only if cancelled)
```

### 4.3 Notable Design Choices

* **Fixed bet size of 0.05 BNB.** Larger positions require multiple transactions.
* **The `Market.token` field is stored but never used.** Only native BNB is accepted.
* **`receive()` is an empty payable function.** Direct transfers vanish into the contract balance with no per·user tracking.
* **Both `claim` and `refund` emit the same `Claimed` event,** making them indistinguishable in logs.

---

## 5. Findings

### F·001: Centralized, Oracle·less Outcome Resolution

| Attribute | Value |
|-----------|-------|
| **Severity** | Critical |
| **Type** | Trust / Centralization |
| **Status** | Open (by design) |
| **Missed by Hexific** | Yes |

**Description:**

`resolveMarket(uint marketId, bool result)` at lines 301·341 accepts the winning side directly as a `bool` parameter and writes it to storage at lines 334·335. The function is gated only by `onlyOwner` (line 309). There is no external verification of the real·world event, no oracle call, no challenge window, no time·lock.

```solidity
function resolveMarket(
    uint marketId,
    bool result
) external onlyOwner {
    ...
    m.result = result;            // line 334·335: outcome is whatever owner says
    emit MarketResolved(marketId, result);
}
```

**Impact:**

The owner can declare any side the winner regardless of the real·world outcome and an accomplice claims the entire pool. There is no on·chain control to prevent this. The Hexific report does not mention this at all.

---

### F·002: Owner Holds a Risk·Free Option via Unilateral Cancel + Refund

| Attribute | Value |
|-----------|-------|
| **Severity** | Critical |
| **Type** | Economic / Centralization |
| **Status** | Open (by design) |
| **Missed by Hexific** | Yes |

**Description:**

`cancelMarket` at lines 346·372 is `onlyOwner` with no time constraint. After cancellation, every depositor including the owner can pull back their full `contribution[..][..]` via `refund()` at lines 456·500.

```solidity
function cancelMarket(uint marketId) external onlyOwner {
    ...
    m.cancelled = true;
    emit MarketCancelled(marketId);
}
```

**Impact:**

The owner can observe how bets accumulate. If the developing odds disadvantage them or an associate, the owner simply cancels the market and reclaims their seed. This is a risk·free option held by a single party.

---

### F·003: No Ownership·Transfer Mechanism · Lost·Key Risk Freezes All Funds

| Attribute | Value |
|-----------|-------|
| **Severity** | Critical |
| **Type** | Operational / Access Control |
| **Status** | Open |
| **Missed by Hexific** | Yes (Hexific instead recommended making `owner` `immutable`, which is the opposite of what is needed) |

**Description:**

The contract exposes no `transferOwnership`, no `renounceOwnership`, no two·step rotation, and no recovery surface. Grep evidence:

```
$ grep -E 'transferOwnership|renounceOwnership|setOwner|owner\s*=' contract.sol
110:        owner = msg.sender;
```

The only assignment to `owner` is in the constructor at line 110.

**Impact:**

If the deployer key is lost, compromised, or abandoned:

* No new market can ever be created (`createMarket` is `onlyOwner`).
* No existing market can be resolved (`resolveMarket` is `onlyOwner`).
* No market can be cancelled (`cancelMarket` is `onlyOwner`).

Every open market becomes a permanent black hole. Users cannot even trigger a refund because cancellation itself requires the lost key.

---

### F·004: Legacy `.transfer()` Breaks Smart·Contract Wallets

| Attribute | Value |
|-----------|-------|
| **Severity** | High |
| **Type** | Compatibility / Funds·At·Risk |
| **Status** | Open |
| **Missed by Hexific** | Hexific listed the same code as "re·entrancy" but did not raise the real `.transfer()` gas issue |

**Description:**

`.transfer()` hard·caps the call to the 2300 gas stipend. Grep evidence:

```
$ grep -nE '\.call\{|\.send\(|\.transfer\(|nonReentrant|ReentrancyGuard' contract.sol
444:            .transfer(payout);
493:            .transfer(amount);
```

No `.call{}`, no `nonReentrant`, no `ReentrancyGuard`.

**Impact:**

Recipients whose `fallback` / `receive` consumes more than 2300 gas (Gnosis Safe, ERC·4337 smart accounts, most contract wallets) will revert. Winners that bet from such a wallet cannot withdraw and depositors cannot refund a cancelled market. Their funds remain locked in the contract.

---

### F·005: Misleading `token` Field Implies Non·Existent Multi·Token Support

| Attribute | Value |
|-----------|-------|
| **Severity** | High |
| **Type** | Interface / User Deception |
| **Status** | Open |
| **Missed by Hexific** | Yes |

**Description:**

`Input.token` (line 44) and `Market.token` (line 58) are declared and assigned in `createMarket` at lines 161·162 but referenced nowhere else in the contract. Every bet path (`buyYes`, `buyNo`, `_placeBet`) hard·requires `msg.value == DEPOSIT` · native BNB only. Grep evidence:

```
$ grep -n token contract.sol
44:        address token;
58:        address token;
161:        m.token =
162:            input.token;
```

There is no `IERC20`, no `transferFrom`, no `safeTransfer` anywhere in the source:

```
$ grep -nE 'IERC20|transferFrom|safeTransfer' contract.sol
(no matches)
```

**Impact:**

Users reading the ABI or block·explorer state may reasonably believe markets can be denominated in arbitrary ERC·20 tokens. They cannot. This is a truth·in·interface defect that can mislead integrators and end users.

---

### F·006: Untracked BNB Received via `receive()` Is Permanently Unaccounted

| Attribute | Value |
|-----------|-------|
| **Severity** | High |
| **Type** | Funds·At·Risk |
| **Status** | Open |
| **Missed by Hexific** | Yes |

**Description:**

`receive()` at line 578 has an empty body:

```solidity
receive() external payable {}    // line 578
```

Direct BNB transfers to the contract are silently accepted and not credited to any `yesBalance`, `noBalance`, `contribution`, or `totalPool` accounting structure. There is no admin sweep, no rescue function:

```
$ grep -nE 'sweep|rescue|withdraw|treasury|recover' contract.sol
(no matches)
```

**Impact:**

Users who accidentally send BNB straight to the address (common with copy·paste mistakes or wallet auto·routing) have **no on·chain path to recover the funds**. The BNB becomes structurally stuck.

---

### F·007: Integer·Division Dust Accumulates and Is Never Swept

| Attribute | Value |
|-----------|-------|
| **Severity** | Medium |
| **Type** | Accounting |
| **Status** | Open |
| **Missed by Hexific** | Yes |

**Description:**

Payout formula at lines 431·436:

```solidity
uint payout = (m.totalPool * bet) / pool;    // line 431·436
```

Solidity integer division truncates. Across all winning claimers the sum of payouts is generally less than `totalPool`; the residual dust stays in the contract balance forever. The protocol has no treasury, no rescue, no re·distribution function.

**Impact:**

Over many markets the contract slowly accumulates a permanent un·withdrawable balance.

---

### F·008: Creator's Seed BNB Is Structurally One·Way on Normal Resolution

| Attribute | Value |
|-----------|-------|
| **Severity** | Medium |
| **Type** | Economic Asymmetry |
| **Status** | Open (by design) |
| **Missed by Hexific** | Yes |

**Description:**

`createMarket` credits the creator's 0.05 BNB to `m.yesPool / m.noPool / m.totalPool / contribution[id][msg.sender]` but **not** to `yesBalance` or `noBalance` (lines 167·180). `claim()` pays out based on the bettor's `yesBalance / noBalance` (lines 404·407, 416·419), so the seed portion specifically can never be reclaimed by the creator via `claim()` on a resolved market · it always flows to the winning side's payouts. Only a cancellation enables the creator to recover the seed via `refund()`.

**Impact:**

Combined with F·002 (owner·only cancel), this turns the seed into a cancel·or·donate lever for the owner: cancel before resolution and recover the 0.05 BNB, or let the market resolve and forfeit it to whichever side wins. The asymmetry is not documented in the contract or in any on·chain comment.

---

### F·009: Minimal Events Force Users to Trust Off·Chain Indexers

| Attribute | Value |
|-----------|-------|
| **Severity** | Medium |
| **Type** | Observability |
| **Status** | Open |
| **Missed by Hexific** | Yes |

**Description:**

Events declared at lines 9·33:

```solidity
event MarketCreated(uint indexed marketId);
event MarketResolved(uint indexed marketId, bool result);
event MarketCancelled(uint indexed marketId);
```

`MarketCreated` emits only the id; title, category, endTime, creator and token are not in the event. Anyone monitoring on·chain state must perform an additional `markets(id)` call per market.

**Impact:**

Light clients and trust·minimised UIs cannot rely on logs alone; they must add an extra RPC round·trip per market or depend on a centralised indexer that may itself be operated by the project.

---

### F·010: `Claimed` Event Ambiguous Between Winnings and Refunds

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Observability |
| **Status** | Open |
| **Missed by Hexific** | Yes |

**Description:**

`claim()` at lines 446·450 and `refund()` at lines 495·499 both emit the same `Claimed` event. Off·chain analytics cannot distinguish "won a bet" from "refunded a cancelled market" without correlating with `m.resolved` or `m.cancelled`.

**Impact:**

Block·explorer analytics and indexers may mis·label withdrawals. Trivial fix would be a dedicated `Refunded` event.

---

### F·011: Fixed Bet Size of 0.05 BNB Harms UX and Risk Distribution

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Usability |
| **Status** | Open (by design) |
| **Missed by Hexific** | Yes |

**Description:**

`_placeBet` at lines 234·237:

```solidity
require(
    msg.value == DEPOSIT,
    "0.05 BNB required"
);
```

**Impact:**

A user wishing to bet 1 BNB must issue 20 separate transactions, paying 20× the gas overhead and increasing on·chain noise. There is no design rationale in the contract or in any comment for the equal·bet·size constraint.

---

## 6. Findings Summary

| ID | Title | Severity | Hexific |
|----|-------|----------|---------|
| F·001 | Centralized, oracle·less outcome resolution | Critical | Missed |
| F·002 | Owner risk·free option via cancel + refund | Critical | Missed |
| F·003 | No ownership·transfer mechanism | Critical | Recommended wrong fix |
| F·004 | Legacy `.transfer()` breaks smart wallets | High | Mis·classified as re·entrancy |
| F·005 | Misleading `token` field (multi·token claim false) | High | Missed |
| F·006 | `receive()` drops BNB with no recovery | High | Missed |
| F·007 | Integer·division dust never swept | Medium | Missed |
| F·008 | Creator seed structurally one·way | Medium | Missed |
| F·009 | Minimal events force off·chain trust | Medium | Missed |
| F·010 | `Claimed` event ambiguous | Low | Missed |
| F·011 | Fixed 0.05 BNB bet size | Low | Missed |

---

## 7. Security Checklist

### Access Control & Governance

* [ ] **Ownership transfer:** No `transferOwnership` / `renounceOwnership` (F·003)
* [ ] **Multisig:** Owner is a single EOA
* [ ] **Time·lock:** None on resolve / cancel
* [ ] **Role separation:** No role splitting (owner does everything)

### Oracle & Outcome Determination

* [ ] **Oracle integration:** None · owner declares outcome manually (F·001)
* [ ] **Challenge / dispute window:** None
* [ ] **Resolution audit trail:** Only an event with the bool the owner passed in

### Payment Primitives

* [ ] **Modern call pattern:** Uses legacy `.transfer()` (F·004)
* [ ] **Re·entrancy guard:** None used; CEI relied upon manually
* [ ] **Untracked deposits:** `receive()` silently swallows BNB (F·006)
* [ ] **Dust recovery:** No sweep / treasury (F·007)

### Interface Honesty

* [ ] **All declared fields used:** `Market.token` declared but unused (F·005)
* [ ] **Event richness:** `MarketCreated` lacks creator / title / endTime (F·009)
* [ ] **Event disambiguation:** `Claimed` reused for refunds (F·010)

### Source Verification

* [x] **BscScan verification:** Source is verified, single file
* [x] **Compiler pinned:** `^0.8.20` (caret allows minor versions)
* [x] **No proxy:** Bytecode is fixed, no upgrade slot

---

## 8. Mefai Security Score

| Category | Check | Result | Points |
|----------|-------|--------|--------|
| 1. Ownership & Access Control | Single EOA, no transfer, no multisig, no time·lock | Centralized, no recovery | **2/20** |
| 2. Outcome Resolution | No oracle, owner sets `result` directly | Fully discretionary | **0/20** |
| 3. Payment Safety | `.transfer()` 2300 gas, no re·entrancy guard, empty `receive()` | Funds·at·risk for SC wallets | **5/15** |
| 4. Code Quality | CEI followed in claim/refund, no overflow risk on 0.8.x, no external imports | Clean on basics | **12/15** |
| 5. Economic Model | Cancel·or·donate seed lever, dust trap, fixed bet size, refund/seed asymmetry | Multiple structural issues | **5/15** |
| 6. Interface & Transparency | Dead `token` field, minimal events, ambiguous `Claimed`, source is verified | Mixed | **8/15** |

**Total Score: 32/100 · HIGH RISK (CRITICAL CENTRALIZATION)**

---

## 9. Claims vs On·Chain Reality

The following table compares claims made by the Hexific audit PDF against independently verified on·chain data and source code.

| # | Claim | Source | On·Chain Reality | Verdict |
|---|-------|--------|-------------------|---------|
| 1 | "0 Critical" findings | Hexific PDF cover page | Three Critical issues identified (F·001 · F·003), all related to centralization and ownership. | **MISLEADING** |
| 2 | "0 Major / 0 Medium" findings | Hexific PDF cover page | Three High and three Medium issues identified (F·004 · F·009). | **MISLEADING** |
| 3 | Re·entrancy in `claim()` (Info) | Hexific PDF page 5 | `claim()` follows CEI: `claimed[][] = true` at lines 438·441 precedes `.transfer` at line 444. No re·entrancy. Real risk is `.transfer()` gas stipend. | **MIS·CLASSIFIED** |
| 4 | Re·entrancy in `refund()` (Info) | Hexific PDF page 5 | Same · `claimed[][] = true` at lines 487·490 precedes `.transfer` at line 493. | **MIS·CLASSIFIED** |
| 5 | "`owner` should be immutable" (Info) | Hexific PDF page 6 | Making `owner` immutable would prevent any future ownership transfer · the contract's real defect is the absence of any ownership·management surface (F·003). | **WRONG RECOMMENDATION** |
| 6 | Pragma `^0.8.20` contains known severe issues (Info) | Hexific PDF page 4 | Generic version warning. Bytecode is already deployed and frozen. | **TRUE BUT NOT ACTIONABLE** |
| 7 | 3 × `block.timestamp` Minor | Hexific PDF pages 2·3 | Correct usage flag but bounded miner drift (~15 s) is irrelevant for market durations measured in hours / days. | **TRIVIAL** |
| 8 | Contract advertises an audited prediction market | `predictbnb.fun` (publishes Hexific PDF) | The audit is the verbatim output of an automated Slither scan with no architectural analysis. The on·chain contract is fully owner·controlled with no oracle. | **MARKETING DOES NOT MATCH RISK PROFILE** |

---

## 10. On·Chain Verification Commands

All findings can be independently verified using the following commands.

### Fetch verified source from BscScan UI

```bash
curl -sL "https://bscscan.com/address/0x7936b54c88efec944df6d3bf14a866f211ea6177" \
  | python3 -c "
import sys, re, html
h = sys.stdin.read()
m = re.search(r\"data-csource='([^']+)'\", h)
open('contract.sol','w').write(html.unescape(m.group(1)))
print('saved contract.sol')
"
```

### Verify Hexific line references match the source

```bash
# Expect: pragma at line 2, owner at line 103, function ranges as documented
grep -n 'pragma solidity' contract.sol
grep -n 'address public owner' contract.sol
grep -n 'function createMarket\|function _placeBet\|function resolveMarket\|function claim\|function refund' contract.sol
```

### Verify F·003 (no ownership transfer)

```bash
grep -En 'transferOwnership|renounceOwnership|setOwner|owner\s*=' contract.sol
# Expected: a single match at line 110 (owner = msg.sender; in constructor)
```

### Verify F·004 (legacy `.transfer()`, no reentrancy guard)

```bash
grep -nE '\.call\{|\.send\(|\.transfer\(|nonReentrant|ReentrancyGuard' contract.sol
# Expected: only two matches · .transfer(payout) line 444, .transfer(amount) line 493
```

### Verify F·005 (`token` field unused)

```bash
grep -n 'token' contract.sol
# Expected: 4 lines · struct decl (44, 58), assignment (161, 162). Nothing in any logic path.

grep -nE 'IERC20|transferFrom|safeTransfer' contract.sol
# Expected: no matches
```

### Verify F·006 (`receive()` empty, no rescue)

```bash
sed -n '575,579p' contract.sol
# Expected: receive() external payable {}

grep -nE 'sweep|rescue|withdraw|treasury|recover' contract.sol
# Expected: no matches
```

### Re·derive payout formula (F·007 dust)

```python
# Toy market: yesPool=10, noPool=2, totalPool=12, 1 winning bettor with bet=10
totalPool, bet, pool = 12, 10, 10
payout = (totalPool * bet) // pool   # Solidity integer division
print(payout)                         # = 12 in this simple case
# In non·trivial cases sum of payouts < totalPool · residual dust stays in contract
```

---

## 11. Conclusion

The Hexific report is the verbatim output of an automated Slither scan re·formatted into a six·page PDF. It contains no architectural analysis, no trust·assumption review, no economic·model assessment, and no oracle / centralisation discussion. By stamping *"0 Critical / 0 Major / 0 Medium"* it gives users the impression of a clean bill of health that the contract does not deserve.

Mefai's independent re·audit identifies **three Critical, three High, three Medium, and two Low·severity issues** that the Hexific report missed entirely. In aggregate the contract:

1. requires **unconditional trust in the owner** for every market outcome,
2. gives the owner a **risk·free option** to cancel and refund,
3. can **permanently freeze user funds** if the owner key is lost,
4. is **incompatible with modern smart·contract wallets**, and
5. implies **features (multi·token support) that are not implemented**.

Depositing into `PredictBNBv1` is economically equivalent to handing the deposit to the owner with a request to please return it. Users should weigh that risk against the marketing claim of an audited contract.

---

*Generated by **Mefai Security Research** · Independent Smart Contract Verification.*
