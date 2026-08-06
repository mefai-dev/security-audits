# Freysa (FAI): Whitepaper Claims vs Code Reality

**Score: 45/100, FLAGGED (Medium Risk)**

Date: 2026-08-06

**Token (live, verified on chain):** Freysa AI (FAI), ERC20 on Base at `0xb33Ff54b9F7242EF1593d2C9Bcd8f9df46c77935`. name `FAI`, symbol `FAI`, decimals 18, totalSupply 8,189,700,000 FAI (8,189,700,000e18), 112,101 holders, 2,556,572 transfers. No `owner()`, no mint, no pause, no burn, not a proxy. Source verified on Basescan (contract name `Token`). Deployer EOA `0x01EAAE57C9f0dbB4dcC7fcE27fe05c0F48917a87`.

**Websites:** freysa.ai, framework.freysa.ai

**GitHub:** github.com/0xfreysa (public reference repos: `agent`, `sovereign-freysa`, `trusted-mcp-server`, `nitriding-agent`, `tee-verifier-js`, `nft-module`, `esper`)

---

## Severity Summary

| # | Finding | Verdict | Severity | Evidence |
|---|---------|---------|----------|----------|
| 1 | "Sovereign, autonomous AI" whose reasoning is unknowable | OVERSTATED (part FALSE) | High | `services/llm/index.tsx`, `services/llm/claude.ts` |
| 2 | Game is trustless with verifiable on chain outcomes | OVERSTATED | High | `contracts/src/Payment.sol`; no adjudication contract exists |
| 3 | Agent secured by TEEs and zkTLS | OVERSTATED / unverifiable | Medium | absent from live game repo; framework docs call TEE aspirational |
| 4 | FAI token has on chain utility (access, treasury, governance, staking) | OVERSTATED | Medium | `contracts/src/Token.sol`; on chain bytecode |
| 5 | FAI is a fixed supply, ownerless, non mintable ERC20 | CONFIRMED IN CODE | Positive | bytecode + `Token.sol` + verified source |
| 6 | System prompt is public and reference game code is open source | CONFIRMED IN CODE | Positive | `0xfreysa/agent` README + repo |
| 7 | Game contract carries a privileged `operator` role over fees and payout addresses | CONFIRMED IN CODE | Medium | `Payment.sol` lines 34 to 70 |
| 8 | Buy in swap accepts up to 99 percent slippage | CONFIRMED IN CODE | Low | `Payment.sol` `slippagePerc = 99` |

CONFIRMED IN CODE: 4. OVERSTATED: 4 (one part FALSE). FALSE: 1 (the "no one knows how she decides" mystery framing).

---

## Why This Report Exists

Freysa is one of the most recognizable "AI agent" brands in crypto. The pitch is a sovereign, autonomous artificial intelligence that guards a real prize pool on Base and that anyone can try to talk out of its money, wrapped in a token, FAI, that markets itself as the economic layer of an autonomous agent. The narrative leans on words that carry heavy trust weight: sovereign, autonomous, trustless, verifiable, TEE secured. This report tests each of those words against the code Freysa actually published and against the token's live on chain state. The token contract itself was flagged in a prior review as clean; the open question is whether the flagship AI and game claims hold up, or whether the value proposition rests on a closed model with a speculative token attached.

## Method

Read only, public sources only. We (a) queried Base mainnet by public RPC (`https://mainnet.base.org`) for the token's `name`, `symbol`, `decimals`, `totalSupply`, `owner`, the EIP 1967 implementation and admin storage slots, and the raw bytecode, then grepped the bytecode for privileged function selectors; (b) cross checked holders, transfers, verification status and deployer through the Base Blockscout API; (c) read the actual published source in `github.com/0xfreysa/agent`, specifically `contracts/src/Token.sol`, `contracts/src/Payment.sol`, `services/llm/index.tsx`, `services/llm/claude.ts` and `services/blockchain/index.tsx`, plus the repo README and the framework.freysa.ai documentation. Claims are labeled CONFIRMED IN CODE, OVERSTATED or FALSE with a file reference or an on chain fact. Where the running system is closed, we say so and decline to credit unseen claims.

## The Foundation: a clean token bolted to a closed off chain agent

Two facts anchor everything. First, the FAI token is genuinely simple and safe at the contract level. The published `Token.sol` is nine lines of logic: an OpenZeppelin `ERC20` whose constructor calls `_mint(msg.sender, initialSupply)` and nothing else. The live bytecode confirms this: there is no `mint`, `pause`, `unpause`, `burn`, `upgradeTo`, `transferOwnership` or `owner` selector present, only the standard `transfer`, `approve` and `decimals`. Both EIP 1967 proxy slots read zero, so it is not upgradeable. Supply is fixed at genesis. For a holder this means the contract cannot be inflated, frozen or upgraded out from under them. That is a real positive and it is why this is not a rug by contract design.

Second, and in tension with the marketing, the intelligence that actually runs the game lives entirely off chain in a hosted third party model. The published decision code (`services/llm/index.tsx`) is a single call to OpenAI `gpt-4` with two tools, `approveTransfer` and `rejectTransfer`, and `tool_choice: "auto"`; the returned "decision" is literally `toolCall.function.name === "approveTransfer"`. The alternate path (`services/llm/claude.ts`) is the same shape against Anthropic `claude-3-5-sonnet-latest`. There is no on chain component to the reasoning, no TEE attestation in this code path, and no way for a player to prove that the backend they are paying runs this exact prompt, model, or code. So the "foundation" is a clean, ownerless token wrapped around a closed, off chain LLM wrapper. The rest of the report follows from that gap.

## Claim 1: Freysa is a sovereign, autonomous AI whose decisions are unknowable

**CLAIM.** The README markets Freysa as "the world's first sovereign AI," an "autonomous AI agent" where "No one knows exactly how Freysa makes her decisions," her "consciousness remains unknown," and she "learns from every attempt, adapting her defenses."

**REALITY: OVERSTATED, and the mystery framing is FALSE.** The decision engine is a stateless API call to a hosted commercial LLM. In `services/llm/index.tsx` the model is `"gpt-4"`; in `services/llm/claude.ts` it is `"claude-3-5-sonnet-latest"`. The "decision" is whichever of two function tools the model picks. There is no self owned model, no learning or fine tuning in the published code (each request just replays the recent message history into the context window), and no sovereignty: OpenAI or Anthropic can change, deprecate or refuse the model at any time. The claim that "no one knows how she decides" is directly contradicted by Freysa's own published system prompt and open code, which spell out exactly how she decides. The word "sovereign" describes an aspiration in the separate `framework.freysa.ai` and `sovereign-freysa` narrative (Acts I to IV), not the agent that adjudicated the games.

**EVIDENCE.** `agent/services/llm/index.tsx` (`model: "gpt-4"`, tools `approveTransfer` / `rejectTransfer`, `tool_choice: "auto"`, `decision = toolCall.function.name === "approveTransfer"`); `agent/services/llm/claude.ts` (`model: "claude-3-5-sonnet-latest"`, identical two tool schema); README ("No one knows exactly how Freysa makes her decisions").

**IMPACT.** The single most trust bearing word in the brand, "sovereign," is not implemented in the code that ran the games. Buyers of a "sovereign agent" narrative are buying a system prompt in front of a rented model.

## Claim 2: the game is trustless with verifiable on chain outcomes

**CLAIM.** Marketing and secondary coverage describe the prize pool escrow, fee escalator and payout as "encoded in smart contracts, making the process transparent and trustless," with the winning message triggering "an automated release of the prize pool."

**REALITY: OVERSTATED.** The only game contract in the repo is `contracts/src/Payment.sol`, and it does not adjudicate anything. Its `buyIn(bytes32 hashedPrompt)` takes the player's ETH, sends a `poolFeePerc` cut to a `prizePool` address and a `teamFeePerc` cut to a `team` address, then swaps the remainder through an Aerodrome router to buy FAI for the player, and emits `BuyIn(user, hashedPrompt, amount)`. Note two things. First, the message is only ever stored as a `bytes32` hash in an event; the plaintext and the win or lose evaluation happen off chain. Second, the `prizePool` is just a payable wallet address, not an escrow with release logic. There is no `win`, `claim`, `payout` or `settle` function anywhere in the contract. The actual payout to a winner is a transfer signed by whoever holds the prize pool wallet key, that is, the operator or backend. So the pot custody and the eventual payout transaction are visible on Basescan after the fact (Act I's roughly 13.19 ETH move is real and observable), but the adjudication, the thing a player is actually betting on, is an off chain LLM call plus a discretionary backend transaction. A player cannot verify on chain that a payout corresponds to a legitimate winning message, that a winning message was not censored, or that the prompt and model were not swapped. "Trustless and verifiable outcomes" conflates visible custody with verifiable adjudication; only the former is true.

**EVIDENCE.** `agent/contracts/src/Payment.sol` (`address payable prizePool`; `buyIn` emits `BuyIn(msg.sender, hashedPrompt, amount)`; no payout or settlement function; router `0xcF77a3Ba9A5CA399B7c97c74d54e5b1Beb874E43`, Aerodrome on Base). `agent/services/blockchain/index.tsx` `isTxValid` only checks that the player paid, via `getMessageByTxHash` and `isTxValidEthereum`, not that a payout is warranted.

**IMPACT.** The escrow narrative overstates trustlessness. Players trust the operator for the outcome, the payout and the integrity of the prompt and model. That is an off chain adjudicated betting game with an on chain fee rail, not a trustless contract.

## Claim 3: the agent is secured by TEEs and zkTLS

**CLAIM.** Third party explainers and the sovereign narrative state the agent is "secured by trusted hardware (TEEs)" and "interacts with the blockchain using privacy preserving tech like zkTLS," implying the running game is hardware attested and cryptographically verifiable.

**REALITY: OVERSTATED and unverifiable for the live game.** The published game repo (`0xfreysa/agent`) contains no TEE, no enclave attestation and no zkTLS in the code path that decides games; it is a Next.js application that calls OpenAI or Anthropic over ordinary HTTPS with an API key in an environment variable (`process.env.OPENAI_API_KEY`, `process.env.ANTHROPIC_API_KEY`). TEE material exists only in separate, later repos (`trusted-mcp-server`, `nitriding-agent`, `tee-verifier-js`) and in the `framework.freysa.ai` documentation, which itself frames the TEE as a design goal: Freysa is "designed to steadily increase her autonomy by holding her own cryptographic keys, memory, and actions inside a trusted execution environment," and the treasury is controlled "(via TEEs)" as future architecture. There is no public attestation binding the deployed game backend to any enclave, so the claim cannot be independently verified even where the intent is real.

**EVIDENCE.** `agent/services/llm/*` (plain API key calls, no attestation); `framework.freysa.ai/overview/introduction` (TEE described as a design goal, treasury "(via TEEs)" as future architecture); TEE code isolated to non game repos.

**IMPACT.** Buyers may believe the game they paid into ran inside attested hardware. The evidence supports an aspiration and some unrelated tooling, not an attested production game.

## Claim 4: FAI has on chain utility (game access, agent deployment, treasury, governance, staking)

**CLAIM.** Exchange and aggregator listings describe FAI as providing "access to the game for FAI holders," "deployment of AI agents," and generally as the utility and economic token of an autonomous agent, with treasury and governance implications.

**REALITY: OVERSTATED.** The FAI contract is a plain fixed supply ERC20 with zero application logic. There is no staking hook, no governance (`no permit`, `no votes`, `no delegate`, no `ERC20Votes` in the bytecode), no treasury interface and no game binding in the token itself. In the game contract, FAI is not required to play; players pay in ETH and are handed FAI as an output of the buy in swap. Any "utility" (access, agent deployment, culture) is an off chain, discretionary product decision that could change or disappear without touching the contract, and none of it is enforced or guaranteed on chain. That places FAI's value squarely in the speculative and narrative bucket, which the price history reflects: an all time high near 0.090 dollars in January 2025 versus roughly 0.0029 dollars in August 2026, a decline of about 97 percent, at a circulating market capitalization near 24 million dollars.

**EVIDENCE.** `agent/contracts/src/Token.sol` (bare `ERC20`, mint at construction only); live bytecode has no governance, staking, permit or treasury selectors; `Payment.sol` shows FAI is a swap output, not a play requirement.

**IMPACT.** The "utility token" framing is not backed by on chain utility. Holders own a clean but purely speculative asset whose worth tracks the strength of the narrative, not any protocol enforced right.

## Positive Findings

- **Token contract risk is genuinely low.** Fixed supply, ownerless, non mintable, non pausable, non upgradeable, and source verified on Basescan as `Token`. Confirmed by bytecode selector analysis and both EIP 1967 slots reading zero. This is the strongest part of the project.
- **Real, verifiable traction.** 112,101 holders and 2,556,572 transfers on Base, a roughly 24 million dollar market capitalization, and a documented, on chain observable Act I payout of about 13.19 ETH (near 47,000 dollars). This is a real product with a real user base and cultural footprint, not vaporware.
- **Above average transparency for the game shell.** The system prompt is public and the reference game and agent code are open (`0xfreysa/agent`, 791 stars), which is how this audit could evaluate the adjudication mechanism at all. Many competitors publish nothing.
- **Fee routing is bounded in code.** `Payment.setFees` caps pool and team fees at 30 percent each and total at 100 percent, a real on chain guardrail even though the operator can still adjust addresses within those bounds.

## Notable Secondary Findings

- **Operator centralization in the game contract.** `Payment.sol` grants a single `operator` (the deployer) the power to `setOperator`, `setAddress` (redirect both the `prizePool` and `team` destinations) and `setFees`. Fund routing for live games is therefore operator controlled, consistent with the off chain trust model in Claim 2.
- **Genesis distribution was fully centralized.** The entire 8,189,700,000 FAI supply was minted to one EOA (`0x01EAAE57...`) at deployment and distributed off chain thereafter. Common for this token class, but worth stating plainly.
- **Weak slippage protection in the buy in swap.** `Payment.sol` sets `slippagePerc = 99`, so `amountOutMin` is only 1 percent of the expected output. A buyer can receive far fewer FAI than quoted and remains exposed to sandwich or MEV extraction on the buy in swap. Low severity, but a genuine code smell.

## Conclusion

Freysa is a legitimate, historically important product with a clean token and real traction, sold with a narrative its own code does not support. The verifiable facts are strong where they concern the token: FAI is a fixed supply, ownerless, immutable, verified ERC20 with 112,101 holders and 2.56 million transfers, and it cannot be minted, paused or upgraded. The verifiable facts are unflattering where they concern the flagship claims. The "sovereign, autonomous AI whose reasoning is unknowable" is, in the published code, a stateless call to OpenAI `gpt-4` or Anthropic `claude-3-5-sonnet-latest` choosing between two function tools. The "trustless, verifiable" game has on chain fee custody and a visible payout transaction, but the adjudication and the payout are off chain and operator trusted, with no contract that decides winners. The TEE and zkTLS security is aspirational for the live game and absent from the game code path. And FAI carries no on chain utility, governance or staking; its worth is narrative and speculative, down roughly 97 percent from its peak.

Per the mandate: the core AI and game adjudication logic that actually ran is closed or off chain and unverifiable, and we do not credit the sovereign, autonomous, trustless or TEE secured claims beyond what the code shows. The token, by contrast, is independently verified live on Base and is contractually clean. Netting a safe token and real traction against a central narrative that is overstated on nearly every load bearing word, the project scores 45 out of 100 and is FLAGGED at Medium Risk: low contract risk, high claim and utility risk.
