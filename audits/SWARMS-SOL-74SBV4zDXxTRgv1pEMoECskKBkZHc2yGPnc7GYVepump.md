# Swarms (SWARMS): Whitepaper Claims vs Code Reality

**Score: 42/100, MEDIUM to HIGH RISK**

**Date:** 2026-08-05
**Token:** SWARMS (Solana SPL mint `74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump`, a pump.fun meme mint, 6 decimals, mint authority renounced)
**Websites:** swarms.world, docs.swarms.world, dao.swarms.world, investors.swarms.world
**GitHub:** github.com/kyegomez/swarms (framework, ~7k stars), github.com/kyegomez/swarms-platform (marketplace, now removed and returning 404, so its code was inspected through a public mirror whose files match the cited quotes verbatim)

---

## Severity Summary

| Severity | Count |
|----------|-------|
| Critical | 3 |
| High | 2 |
| Medium | 2 |
| Low | 1 |
| Informational | 1 |

---

## Why This Report Exists

Most reports we publish document projects whose code contradicts their marketing. Swarms is a more unusual subject, and it deserves a careful, fair reading, because two very different things share one name.

The first thing is real. `kyegomez/swarms` is a genuine, actively maintained multi agent orchestration framework in Python with roughly 7,000 GitHub stars, a published PyPI package, a working prompt and agent marketplace, and thousands of lines of substantive engineering. We credit that plainly and repeatedly below.

The second thing is a pump.fun meme coin. The claim under audit is not "does the framework exist" (it does) but "does the SWARMS token have any real role in that framework, marketplace, or DAO." We read the actual public source of both the framework and the marketplace, we traced every payment and access path in the code, and we verified the token mint and the so called DAO treasury directly on the Solana chain.

We are not making accusations and we are not spreading FUD. We read the source and show what is implemented. The finding is the one the structure of the project predicts: the code is real, and the token is decoupled from it. The framework does not use the token. The marketplace charges in US dollars through Stripe. Agent monetization in the code is paid in USDC on Base, not in SWARMS. And the "DAO" is a bare wallet address, not a governance contract.

## Method

For every major claim on the token, DAO, and investor pages we located the relevant code in the two real repositories, fetched the raw files, and read what is actually implemented. We then queried the Solana mainnet RPC to check the token mint authority and the treasury account owner. Each claim is labelled CONFIRMED IN CODE, OVERSTATED, or FALSE, with a repository path, file, line, and a short verbatim snippet, or with the on chain fact.

---

## The Foundation: A Real Framework With a Meme Coin Bolted On

**CLAIM:**
> Swarms is "infrastructure for the agentic economy," a multi agent orchestration framework, a marketplace, and a DAO, powered by the $SWARMS token.

**REALITY:** The framework half is real. The token half is a separate pump.fun launch that the framework code never references. The token mint address does not appear anywhere in the framework source (it appears only on the platform's swarmeconomy marketing page as a display only copy to clipboard address and pump.fun link, never in a payment or staking code path), and the only place `$Swarms` appears in the framework is inside example files, one of which is a chatbot whose entire job is to talk about the coin.

**EVIDENCE:**
```
# grep of the token mint across the whole framework repo: zero hits in code
$ grep -rniE '74SBV4' kyegomez/swarms/     -> (no results)

# $Swarms in the framework appears only in an example system prompt, e.g.
# examples/guides/demos/crypto/swarms_coin_agent.py:12
"You are an advanced financial analysis and ecosystem economics agent,
 specializing in the $Swarms cryptocurrency."
# examples/guides/demos/crypto/swarms_coin_agent.py:42
"Each agent operates with its tokenomics, with $Swarms as the base currency
 for value exchange."
# examples/guides/demos/crypto/swarms_coin_agent.py:66
"Universal Currency: Power all agent interactions with $Swarms."
```
The entire vision, roadmap, and "universal currency" narrative lives inside `SWARMS_AGENT_SYS_PROMPT`, a hardcoded string handed to a demo agent that answers price questions by calling the CoinGecko API. It is marketing copy dressed as a system prompt, not a feature.

**IMPACT:** The strongest, most defensible claim Swarms makes ("we have a real multi agent framework") is true. The claim being audited ("the token powers it") is not visible anywhere in the framework's own code. Informational.

---

## Claim 1: A real multi agent orchestration framework

**CLAIM:**
> "The Enterprise-Grade Production-Ready Multi Agent Orchestration Framework." Sequential, concurrent, hierarchical, and graph based agent architectures.

**REALITY:** CONFIRMED IN CODE. This is genuine, substantial software: 967 Python files, real orchestration primitives, a published `swarms` PyPI package with monthly download badges, and named multi agent architectures used throughout the codebase. Credit where due.

**EVIDENCE:**
```
# kyegomez/swarms/README.md:50
"Swarms is the most reliable, scalable, and adaptive multi agent orchestration
 framework available today ... sequential, concurrent, and hierarchical systems."
# swarms/ package tree: swarms/structs, swarms/agents, swarms/prompts, ... (967 .py files)
# server/routers/chat.ts:985-989 (marketplace) references real architectures:
'GroupChat','MultiAgentRouter','AutoSwarmBuilder','HiearchicalSwarm','MajorityVoting'
```
Note that `MajorityVoting` here is a multi agent decision pattern, not token governance.

**IMPACT:** Positive. The framework is the real asset of this project. This is the single strongest confirmation in the report. Informational.

---

## Claim 2: A real agent and prompt marketplace

**CLAIM:**
> Swarms Marketplace: "The Agentic Labor Marketplace," discover and share production ready prompts and agents; agent monetization.

**REALITY:** CONFIRMED IN CODE that the marketplace exists and works, but OVERSTATED on the token's role. The marketplace is a standard Next.js plus Supabase application. Access is a plain `SWARMS_API_KEY` Bearer key, not a token holding. Listings are priced in US dollars, and payment runs through Stripe. The token is not the currency of the marketplace.

**EVIDENCE:**
```python
# swarms/agents/agent_marketplace_handler.py:40,92,106
MARKETPLACE_BASE_URL = "https://swarms.world/api"
api_key = os.getenv("SWARMS_API_KEY")
"Authorization": f"Bearer {cls.check_api_key()}"
# publishing a paid agent is priced in USD, not SWARMS:
# agent_marketplace_handler.py:272-273,318-319
is_free: bool = True,
price_usd: float = 0.0,
"is_free": is_free, "price_usd": price_usd,
```
```jsonc
// swarms-platform/package.json  (marketplace payment stack)
"@stripe/react-stripe-js": "^2.6.0", "@stripe/stripe-js": "2.4.0", "stripe": "^17.6.0"
```

**IMPACT:** The marketplace is real, but it is a conventional API key plus Stripe/USD product. Holding or spending SWARMS is not required to use it. "Agent monetization" is denominated in dollars in the code. Medium.

---

## Claim 3: $SWARMS is the base currency for all agent transactions

**CLAIM:**
> "Base currency for all agent transactions." "A unified medium of exchange for all agent interactions." "Universal Currency: Power all agent interactions with $Swarms."

**REALITY:** FALSE. No code path in the framework or the marketplace uses SWARMS as a medium of exchange for agent work. The only cryptocurrency payment protocol wired into the framework is x402, and every x402 example pays in USDC on the Base network to an arbitrary EVM wallet. SWARMS is a Solana token and is never involved.

**EVIDENCE:**
```python
# examples/guides/x402_examples/research_agent_x402_example.py:27-29
price="$0.01",
pay_to_address="0xYourWalletAddressHere",
network_id="base-sepolia",
# examples/guides/x402_examples/memecoin_agent_x402.py:129-133,209
price="$0.10",  # 10 cents per analysis
pay_to_address=os.getenv("WALLET_ADDRESS", "0xYourWalletAddress"),
network_id="base-sepolia",   # "pay $0.10 in USDC to access the analysis"
# README.md:839  x402 described as a generic "Cryptocurrency payment protocol for API endpoints"
```

**IMPACT:** Agent to agent payment in the code is USDC on an Ethereum L2, denominated in dollars. The "universal currency" for agents is not SWARMS in any file we could find. High.

---

## Claim 4: On chain DAO and community governance

**CLAIM:**
> "Transparent and efficient dao-driven decision making." "Governance rights in the ecosystem." "Establish community governance." Participate by "sending $swarms tokens to the treasury address."

**REALITY:** FALSE as a governance mechanism. There is no governance program, no voting contract, no proposal logic, and no on chain DAO anywhere in the code. The "DAO" is a front end that connects a Phantom wallet and builds a plain SPL token transfer to a treasury address read from an environment variable. We verified on chain that this treasury is a bare wallet owned by the System Program, not a program and not executable.

**EVIDENCE:**
```typescript
// modules/platform/account/components/crypto-wallet/index.tsx:14-19
const SWARMS_TOKEN_ADDRESS  = new PublicKey(process.env.NEXT_PUBLIC_SWARMS_TOKEN_ADDRESS!);
const DAO_TREASURY_ADDRESS  = new PublicKey(process.env.NEXT_PUBLIC_DAO_TREASURY_ADDRESS!);
// index.tsx:176-210  the whole "DAO participation" is one SPL transfer to the treasury
createTransferCheckedInstruction(sourceAccount, SWARMS_TOKEN_ADDRESS,
    destinationAccount /* = treasury ATA */, senderPublicKey, rawAmount, 6, []);
```
```
# Solana mainnet RPC getAccountInfo("7MaX4muAn8ZQREJxnupm8sgokwFHujgrGfH9Qn81BuEV")
owner program: 11111111111111111111111111111111   (System Program = plain wallet)
executable: False                                   (not a governance program)
# No Rust / Anchor / Cargo.toml / on chain program exists anywhere in the repo.
```

**IMPACT:** "Send your tokens to the treasury to govern" is a transfer to a personal wallet with no on chain governance behind it. There are no votes, no proposals, and no smart contract enforcing anything. Critical.

---

## Claim 5: Staking rewards, up to 20% APY, 30 day minimum stake

**CLAIM:**
> "Staking rewards and incentives." "Staking Rewards: Up to 20% APY through staking." DAO requirements: "1000 $swarms minimum," "30-day minimum stake."

**REALITY:** FALSE. There is no staking contract, no lockup, no reward accrual, and no APY logic in any repository. What the code actually does when you "stake" is transfer your tokens one way to the treasury wallet, then credit your off chain account with the US dollar value of those tokens as prepaid platform credits at a flat 1:1 ratio. The tokens are not returned, there is no yield, and there is no time lock.

**EVIDENCE:**
```typescript
// modules/platform/account/components/crypto-wallet/index.tsx:207-234
const usdValue = parseFloat(amountToSend) * swarmsPrice;   // CoinGecko price
await addUsdBalanceMutation.mutateAsync({ amountUsd: usdValue, transactionHash: signature, ... });
```
```typescript
// server/routers/dashboard.ts:124-165
// verify the tx destination is the DAO treasury, then:
const USD_TO_CREDITS_RATIO = 1;
const creditsToAdd = input.amountUsd * USD_TO_CREDITS_RATIO;   // USD value -> credits
// UPDATE swarms_cloud_users_credits SET credit = newCreditBalance
```
```
# grep -rniE 'stak|apy|reward|proposal|vote' across schema.sql / supabase / server:
#   -> no staking table, no APY field, no reward accrual, no proposal or vote logic
```

**IMPACT:** Users are told they earn up to 20% APY by staking for 30 days. The code contains no staking, no APY, and no lock. It is a one way top up: give tokens to a wallet, receive dollar denominated API credits. Critical.

---

## Claim 6: Protocol revenue share

**CLAIM:**
> "Protocol revenue share" (listed as a benefit of sending tokens to the DAO on the investor and DAO pages).

**REALITY:** FALSE / unimplemented. No revenue distribution code exists. Nothing in the marketplace server routes, the credits table, or any Solana code pays a share of protocol revenue back to token holders or stakers. The only value flow that touches the token is inbound (user to treasury), converted to non refundable credits.

**EVIDENCE:**
```
# grep -rniE 'revenue|dividend|distribut|payout|share' across server/ and modules/:
#   -> no holder or staker revenue-share mechanism; only Stripe billing and credit top-ups
# The credit ledger is one directional: dashboard.ts adds credits; nothing pays SWARMS out.
```

**IMPACT:** There is no mechanism by which holding or staking SWARMS returns protocol revenue. The claim is marketing without an implementation. Critical.

---

## Claim 7: Balanced tokenomics with a tiny team allocation

**CLAIM:**
> Investor and press material: "only 2% was reserved for the team, among the smallest allocations in DAO history." The marketplace's own economy page instead shows a five slice distribution.

**REALITY:** FALSE and internally contradictory. The project's own `swarmeconomy` page hardcodes a distribution in which insiders hold far more than 2 percent, and in any case a pump.fun mint has no on chain vesting or allocation buckets to enforce any of these numbers. The mint authority is renounced, so supply is whatever the bonding curve produced, not a programmed cap table.

**EVIDENCE:**
```typescript
// app/swarmeconomy/page.tsx:203-209  (the site's own allocation chart)
{ name: "Liquidity Pool", percentage: 35 },
{ name: "Agent Rewards & Community", percentage: 25 },
{ name: "Founder", percentage: 20 },
{ name: "Team", percentage: 15 },
{ name: "Investors & Advisors", percentage: 5 },
// Founder 20 + Team 15 + Investors 5 = 40% insiders, not 2%.
```
```
# On chain reality (getAccountInfo on the mint):
mintAuthority: None      # renounced; no programmatic allocation / vesting
freezeAuthority: None
decimals: 6
# There are no vesting contracts or allocation programs in any repo.
```

**IMPACT:** The "2% team" story and the site's own 40 percent insider chart cannot both be true, and neither is enforced on chain. The distribution presented as balanced tokenomics is decorative. High.

---

## Claim 8: A large, fixed, secure token supply

**CLAIM:**
> `swarmeconomy` page: "Total Supply: 1,000,000,000,000 $Swarms Tokens," "Built on Solana for fast, secure transactions."

**REALITY:** OVERSTATED in the number, CONFIRMED in the "fixed and non malicious" sense. The on chain supply is about 1 billion whole tokens (roughly 1e15 base units at 6 decimals), not the 1 trillion the page prints, a 1000x labelling discrepancy. On the credit side, the mint authority is renounced and there is no freeze authority, so the contract is a standard, fixed supply SPL token with no honeypot style freeze or mint trap. That is the one genuinely reassuring token fact in this report.

**EVIDENCE:**
```
# getAccountInfo("74SBV4...pump") jsonParsed:
owner program: TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA   # standard SPL Token program
decimals: 6
mintAuthority: None        # cannot mint more
freezeAuthority: None      # cannot freeze holders
supply: 999968856364477    # ~1e15 base units = ~1,000,000,000 whole tokens (not 1e12)
```

**IMPACT:** The token contract itself is clean and fixed supply, which keeps this out of ICE style territory. But the headline "1,000,000,000,000" supply figure on the project's own page is off by three orders of magnitude from chain state. Low.

---

## Additional Note: The token touches the product in exactly one place, and it is one way

Worth stating plainly. Across two real repositories, the single code path where SWARMS interacts with the product is the crypto wallet top up: a user sends SWARMS to the treasury wallet, and the backend converts the dollar value into prepaid platform credits. Every other advertised utility (currency for agents, staking yield, governance, revenue share) has no implementation. The framework runs without the token, the marketplace bills in dollars through Stripe, and agent payments in the code are USDC on Base. Economically, the "DAO stake" is indistinguishable from selling your tokens to the treasury in exchange for non refundable API credit.

---

## Conclusion

Swarms is two projects wearing one name. The multi agent orchestration framework is real, active, and genuinely useful, and it earns full credit here (Claim 1), as does the fact that the marketplace exists and functions (Claim 2). The token contract is a clean, fixed supply, mint renounced SPL token with no freeze authority (Claim 8), which is why this is not an ICE style fraud score.

But the audit question is whether the SWARMS token has the utility its own pages advertise, and the answer from the code is almost entirely no. The token is the base currency for nothing: the framework never references it, the marketplace charges dollars through Stripe, and agent monetization is paid in USDC on Base (Claim 3). The DAO is not a contract; it is a plain System Program wallet you are told to send tokens to, with no governance, voting, or proposal logic anywhere (Claim 4). Staking with "up to 20% APY" and a "30 day minimum" does not exist as a lock or a yield; sending tokens simply converts them one way into off chain credits at 1 USD to 1 credit (Claim 5). Revenue share is unimplemented (Claim 6). The distribution story is internally contradictory and unenforceable on a pump.fun mint (Claim 7), and the advertised supply figure is off by 1000x (Claim 8).

None of the framework work is fraudulent, and the token contract is not a honeypot. But the token is decoupled from the product it claims to power, and the DAO and staking claims specifically induce users to transfer tokens to a personal wallet in exchange for governance and yield that the code does not provide. Score 42 out of 100, MEDIUM to HIGH RISK, driven by a real codebase paired with a token whose advertised utility is mostly unimplemented.

| Claim | Verdict |
|-------|---------|
| Real multi agent orchestration framework | CONFIRMED IN CODE |
| Real agent and prompt marketplace exists | CONFIRMED IN CODE (token role OVERSTATED) |
| $SWARMS is the base currency for all agent transactions | FALSE (USDC on Base, USD via Stripe) |
| On chain DAO and community governance | FALSE (treasury is a plain wallet, no contract) |
| Staking rewards up to 20% APY, 30 day minimum | FALSE (one way transfer to credits, no lock, no yield) |
| Protocol revenue share | FALSE (unimplemented) |
| Balanced tokenomics, ~2% team | FALSE (site chart shows ~40% insiders, no on chain vesting) |
| Large fixed secure supply | OVERSTATED figure, CONFIRMED clean contract |

Tally: CONFIRMED IN CODE 2, OVERSTATED 1, FALSE 5. (Plus one informational foundation note and one clean contract confirmation folded into Claim 8.)

---

## Verification and Sources (exact repositories and files read)

Framework (github.com/kyegomez/swarms, branch master):
- README.md (overview line 50, protocols table line 834-839 x402)
- swarms/agents/agent_marketplace_handler.py (SWARMS_API_KEY Bearer auth, MARKETPLACE_BASE_URL, is_free / price_usd)
- examples/guides/demos/crypto/swarms_coin_agent.py (the $Swarms "universal currency" system prompt)
- examples/guides/demos/crypto/dao_swarm.py (a fictional climate DAO demo, unrelated to the SWARMS DAO)
- examples/guides/x402_examples/research_agent_x402_example.py, memecoin_agent_x402.py (USDC on base-sepolia / base)
- grep sweeps: token mint address absent from code; no on chain program; no staking/governance code

Marketplace (github.com/kyegomez/swarms-platform, mirror inspected, last commit 2025-05-29):
- app/swarmeconomy/page.tsx (roadmap "Launch $Swarms token", allocations Founder 20 / Team 15 / Investors 5, supply "1,000,000,000,000", utility bullets, pump.fun links, "Built on Solana")
- modules/platform/account/components/crypto-wallet/index.tsx (Phantom connect, SPL transfer to DAO treasury, DECIMALS 6, CoinGecko price, addCryptoTransactionCredit)
- server/routers/dashboard.ts (verify tx destination == DAO treasury, USD_TO_CREDITS_RATIO = 1, update swarms_cloud_users_credits)
- server/routers/chat.ts (MajorityVoting etc. are swarm architectures, not token governance)
- package.json (Stripe payment stack; @solana/web3.js + @solana/spl-token only for the top-up transfer)
- schema.sql / supabase / server (no staking, APY, reward, proposal, or vote tables)

On chain (Solana mainnet RPC, getAccountInfo):
- Mint 74SBV4zDXxTRgv1pEMoECskKBkZHc2yGPnc7GYVepump: SPL Token program owned, decimals 6, mintAuthority None, freezeAuthority None, supply ~1e15 base units
- Treasury 7MaX4muAn8ZQREJxnupm8sgokwFHujgrGfH9Qn81BuEV: owner System Program, executable false (a plain wallet, not a DAO program)

Public pages:
- swarms.world, docs.swarms.world (framework docs mention only x402 for crypto payment, no SWARMS token utility), dao.swarms.world ("Send your swarms tokens to our DAO treasury address"), investors.swarms.world (governance / staking / revenue share benefits, 1000 minimum, 30 day stake)

---

## Disclaimer

This report documents the relationship between the Swarms project's public marketing and documentation claims and its publicly available open source code, read from the kyegomez/swarms and kyegomez/swarms-platform GitHub repositories, together with direct Solana mainnet queries of the token mint and treasury account. The Swarms multi agent framework is a genuine, actively maintained open source project, and this review credits that fully; the findings above concern the SWARMS token's advertised utility, not the quality of the framework. Read only review. No systems were accessed or modified.

**Report Date:** 2026-08-05
**Website:** https://mefai.io
