# MEFAI Security — Independent Audit Review

### Target — `PredictBNBv1` @ `0x7936b54c88efec944df6d3bf14a866f211ea6177` (BSC)

> **MEFAI SECURITY** · Independent Smart Contract Verification
>
> This report independently re-audits the *Hexific* security report published at
> `https://predictbnb.fun/contract/audit/audit-report.pdf` and compares its findings
> line-by-line with the verified source code on BscScan.

---

| Field | Value |
|---|---|
| **Report Date** | 2026-05-17 |
| **Reviewed Audit** | Hexific Security — `audit-1778962419504` (2026-05-17 03:13 AM) |
| **Reviewed Contract** | `PredictBNBv1` — verified source on BscScan |
| **Compiler** | `pragma solidity ^0.8.20` (single file, ~9.4 KB) |
| **Hexific Severity Claim** | 0 Critical · 0 Major · 0 Medium · 3 Minor · 4 Info |
| **MEFAI Re-Audit Result** | **3 Critical · 3 High · 3 Medium · 2 Low** (additional to what Hexific reported) |

---

## 1. Executive Summary

The Hexific audit assigns a clean rating to `PredictBNBv1`, advertising
**"0 Critical, 0 Major, 0 Medium"**. After re-auditing the exact same source code
that is verified on BscScan, MEFAI finds this conclusion **misleading and incomplete**:

- The contract is a fully **centralized betting market with no oracle**. The owner
  alone declares the outcome of every market.
- The owner can **cancel any market unilaterally** after observing how bets are placed,
  then refund themselves — effectively a risk-free option.
- There is **no ownership transfer function**: if the owner key is lost, every existing
  and future market becomes permanently frozen and user funds are locked.
- Payouts use the legacy `.transfer()` stipend (2300 gas), which silently breaks for
  smart-contract wallets (multisig, account-abstraction, etc.) and can lock winnings.
- The `token` field is stored but **never used** — it implies multi-token support that
  does not exist in the code.

The Hexific report only surfaces seven low-impact items that look exactly like raw
*Slither* detector output (`timestamp`, `solc-version`, `reentrancy-unlimited-gas`,
`immutable-states`) and applies no manual review of the contract's economic model or
trust assumptions.

---

## 2. Verification: Does the audit actually cover this contract?

Yes. Every code snippet in the Hexific PDF (`createMarket` 131–185, `_placeBet` 224–296,
`resolveMarket` 301–341, `claim` 377–451, `refund` 456–500, `owner` line 103, `pragma`
line 2) matches the verified source of `PredictBNBv1` on BscScan byte-for-byte.

> Stylistic note: the on-chain source uses 2–3 blank lines between almost every
> statement, inflating a ~9.4 KB contract to 579 lines. This unusual formatting happens
> to align the audit's line numbers with very wide ranges, which makes the contract
> *look* larger than it is.

---

## 3. Hexific's Claimed Findings

| # | Severity | Finding | Detector |
|---|---|---|---|
| 1 | MINOR | `createMarket` uses `block.timestamp` | timestamp |
| 2 | MINOR | `resolveMarket` uses `block.timestamp` | timestamp |
| 3 | MINOR | `_placeBet` uses `block.timestamp` | timestamp |
| 4 | INFO | Pragma `^0.8.20` contains known issues | solc-version |
| 5 | INFO | Reentrancy in `refund()` | reentrancy-unlimited-gas |
| 6 | INFO | Reentrancy in `claim()` | reentrancy-unlimited-gas |
| 7 | INFO | `owner` should be immutable | immutable-states |

The cover page advertises "3 Minor" while the body text reads "7 minor issues or
suggestions" — an internal presentation inconsistency. The seven findings are exactly
what an out-of-the-box Slither run produces; no manual review is evident.

---

## 4. Critique of Hexific's Reported Items

| Hexific Finding | MEFAI Assessment |
|---|---|
| 3 × `block.timestamp` Minor | Technically correct but trivial. Miner timestamp manipulation is bounded to ~15 s and is irrelevant for markets whose `endTime` is hours or days away. Filler material. |
| Reentrancy in `claim()` — Info | **Mis-classified.** The code follows the Checks-Effects-Interactions pattern: `claimed[..][..] = true;` is set on lines 438–441 *before* `payable(msg.sender).transfer(payout)` on line 444. There is no reentrancy risk; the real concern is the use of `.transfer()` itself (see High-1 below). |
| Reentrancy in `refund()` — Info | Same observation: `claimed[..][..] = true;` on lines 487–490 precedes `.transfer(amount)` on line 493. Not a reentrancy bug; same gas-stipend concern applies. |
| `owner` should be immutable — Info | **Wrong recommendation.** Making `owner` immutable would make it impossible to ever transfer ownership — which is the exact opposite of what the contract needs. The real defect is the complete absence of any ownership-management surface. |
| Pragma `^0.8.20` — Info | Generic warning. Since the contract is already deployed, the bytecode is fixed; this is not a live risk. |

---

## 5. Issues Hexific Completely Missed

### 🔴 CRITICAL — C1: Fully centralized, oracle-less outcome resolution

**Location:** `resolveMarket` (lines 301–341), `createMarket` (lines 131–185),
`cancelMarket` (lines 346–372)

**Description:** All three administrative entry points are gated by `onlyOwner`.
`resolveMarket` accepts the winning side directly as a `bool` parameter and writes it
to storage without any external verification.

```solidity
function resolveMarket(
    uint marketId,
    bool result
) external onlyOwner {
    ...
    m.result = result;            // line 334-335: outcome is whatever owner says
    emit MarketResolved(marketId, result);
}
```

**Impact:** The owner can declare any side the winner regardless of the real-world
event, then have an accomplice claim the entire pool. There is no oracle (Chainlink,
UMA, optimistic-oracle), no challenge window, no time-lock, no multisig requirement.
This single role holds unilateral, irreversible control over user funds.

---

### 🔴 CRITICAL — C2: Owner holds a risk-free option via unilateral cancel + refund

**Location:** `cancelMarket` (lines 346–372) / `refund` (lines 456–500)

**Description:** `cancelMarket` is `onlyOwner` with no time constraint. After
cancellation, every depositor — including the owner who seeded 0.05 BNB — can pull
back their full `contribution[..][..]`.

```solidity
function cancelMarket(uint marketId) external onlyOwner {
    ...
    m.cancelled = true;
    emit MarketCancelled(marketId);
}
```

**Impact:** The owner can observe how bets accumulate. If the developing odds
disadvantage them or an accomplice, the owner simply cancels the market and reclaims
their seed. This is a free, risk-free option held by a single party.

---

### 🔴 CRITICAL — C3: No ownership-transfer mechanism — lost-key risk freezes all funds

**Location:** Constructor (lines 108–112), state variable on line 103

```solidity
address public owner;          // line 103

constructor() {
    owner = msg.sender;        // line 110
}
```

**Description:** The contract exposes no `transferOwnership`, no `renounceOwnership`,
no two-step rotation, and no recovery surface. The owner is whatever EOA deployed the
contract, forever.

**Impact:** If the deployer key is lost, compromised, or simply abandoned:

- No new market can ever be created (`createMarket` is `onlyOwner`).
- No existing market can be resolved (`resolveMarket` is `onlyOwner`).
- No market can be cancelled (`cancelMarket` is `onlyOwner`).

Every open market becomes a permanent black hole for user deposits. Users could not
even trigger a refund because cancellation itself requires the lost key.

---

### 🟠 HIGH — H1: Legacy `.transfer()` breaks smart-contract wallets

**Location:** `claim` line 443–444; `refund` line 492–493

```solidity
payable(msg.sender).transfer(payout);    // line 444
...
payable(msg.sender).transfer(amount);    // line 493
```

**Description:** `.transfer()` hard-caps the call to the 2300 gas stipend. Any
recipient whose fallback / `receive` consumes more than 2300 gas (Gnosis Safe,
ERC-4337 smart accounts, most contract wallets) will revert.

**Impact:** Winners that bet from a smart-contract wallet cannot withdraw, and
depositors that placed funds from such a wallet cannot refund a cancelled market.
Their funds remain locked in the contract. Modern guidance is to use
`(bool ok,) = msg.sender.call{value: payout}("")` combined with a reentrancy guard.

---

### 🟠 HIGH — H2: Misleading `token` field implies non-existent multi-token support

**Location:** `Input.token` (line 44), `Market.token` (line 58), assignment in
`createMarket` (lines 161–162)

```solidity
struct Input  { ... address token; ... }     // line 44
struct Market { ... address token; ... }     // line 58
m.token = input.token;                       // line 161-162
```

**Description:** The `token` field is stored but referenced nowhere else. Every bet
path (`buyYes`, `buyNo`, `_placeBet`) hard-requires `msg.value == DEPOSIT` — i.e.
native BNB only.

**Impact:** A user reading the ABI or block-explorer state may reasonably believe
markets can be denominated in arbitrary ERC-20 tokens. They cannot. This is a
truth-in-interface defect that can mislead integrators and users.

---

### 🟠 HIGH — H3: Untracked BNB received via `receive()` is permanently unaccounted

**Location:** `receive()` line 578

```solidity
receive() external payable {}    // line 578
```

**Description:** Direct BNB transfers to the contract are silently accepted. They are
not credited to any `yesBalance`, `noBalance`, `contribution`, or `totalPool`.

**Impact:** Users who accidentally send BNB straight to the address (common with
copy-paste mistakes or wallet auto-routing) have **no on-chain path to recover the
funds**. There is no sweep, no admin rescue, no per-user balance. The BNB becomes
structurally stuck.

---

### 🟡 MEDIUM — M1: Integer-division dust accumulates and is never swept

**Location:** `claim` lines 431–436

```solidity
uint payout = (m.totalPool * bet) / pool;    // line 431-436
```

**Description:** Solidity integer division truncates. Across all winning claimers the
sum of payouts is generally less than `totalPool`; the residual dust stays in the
contract balance forever. There is no treasury, no rescue, no re-distribution function.

**Impact:** Over many markets the contract slowly accumulates a permanent balance that
is invisible to users and unreclaimable by the protocol.

---

### 🟡 MEDIUM — M2: Creator's seed BNB is structurally one-way on normal resolution

**Location:** `createMarket` lines 167–180 vs. `claim` lines 402–429

**Description:** `createMarket` credits the creator's 0.05 BNB to
`m.yesPool / m.noPool / m.totalPool / contribution[id][msg.sender]` but *not* to
`yesBalance` or `noBalance`. `claim()` pays out based on the bettor's
`yesBalance / noBalance` (lines 404–407, 416–419), so the seed portion specifically
can never be reclaimed by the creator via `claim()` on a resolved market — it always
flows to the winning side's payouts. Only a cancellation enables the creator to recover
the seed via `refund()`.

**Impact:** Combined with C2 (owner-only cancel), this turns the seed into a
cancel-or-donate lever for the owner: cancel before resolution and recover the 0.05 BNB,
or let the market resolve and forfeit it to whichever side wins. The asymmetry is not
documented in the contract or in any on-chain comment.

---

### 🟡 MEDIUM — M3: Minimal events force users to trust off-chain indexers

**Location:** events block lines 9–33

```solidity
event MarketCreated(uint indexed marketId);          // line 9-11
event MarketResolved(uint indexed marketId, bool result);
event MarketCancelled(uint indexed marketId);
```

**Description:** `MarketCreated` emits only the id; title, category, endTime, creator
and token are not in the event. Anyone monitoring on-chain state must perform an
additional `markets(id)` call per market.

**Impact:** Light clients and trust-minimised UIs cannot rely on logs alone; they must
add an extra RPC round-trip per market or depend on a centralised indexer that may
itself be operated by the project.

---

### 🟢 LOW — L1: `Claimed` event ambiguous between winnings and refunds

**Location:** `claim` line 446–450 and `refund` line 495–499 both emit the same
`Claimed` event.

**Impact:** Off-chain analytics cannot distinguish "won a bet" from "refunded a
cancelled market" without correlating with `m.resolved` or `m.cancelled`. Trivial fix
would be a dedicated `Refunded` event.

---

### 🟢 LOW — L2: Fixed bet size of 0.05 BNB harms UX and risk distribution

**Location:** `_placeBet` lines 234–237

```solidity
require(
    msg.value == DEPOSIT,
    "0.05 BNB required"
);
```

**Impact:** A user wishing to bet 1 BNB must issue 20 separate transactions, paying
20× the gas overhead and increasing the on-chain noise. There is no design rationale
for the equal-bet-size constraint in the contract or in any comment.

---

## 6. Severity Comparison Summary

| Severity | Hexific | MEFAI Re-Audit |
|---|---|---|
| 🔴 CRITICAL | 0 | **3** |
| 🟠 HIGH | 0 | **3** |
| 🟡 MEDIUM | 0 | **3** |
| 🟢 LOW / MINOR | 3 | 2 (additional) |
| ⚪ INFO | 4 | 0 net-new |

---

## 7. Conclusion

The Hexific report appears to be the raw output of an automated Slither scan
re-formatted into a six-page PDF. It contains **no architectural analysis, no
trust-assumption review, no economic-model assessment, and no oracle/centralisation
discussion**. By stamping *"0 Critical / 0 Major / 0 Medium"* it gives users the
impression of a clean bill of health that the contract simply does not deserve.

MEFAI's independent re-audit identifies **three Critical, three High, three Medium,
and two additional Low-severity issues** that the Hexific report missed entirely. In
aggregate the contract:

1. requires **unconditional trust in the owner** for every market outcome,
2. gives the owner a **risk-free option** to cancel and refund,
3. can **permanently freeze user funds** if the owner key is lost,
4. is **incompatible with modern smart-contract wallets**, and
5. implies **features (multi-token support) that are not actually implemented**.

Depositing into `PredictBNBv1` is economically equivalent to handing the deposit to
the owner with a request to please return it. Users should weigh that risk against
the marketing claim of an audited contract.

---

*Generated by **MEFAI SECURITY** — Independent Smart Contract Verification. This
document is a technical comparison of publicly available materials (the Hexific PDF
and the verified BscScan source code) and does not constitute legal, financial, or
investment advice.*
