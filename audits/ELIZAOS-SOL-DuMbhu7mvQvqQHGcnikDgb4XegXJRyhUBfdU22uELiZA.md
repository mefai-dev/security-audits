# Security Audit Report: ElizaOS - Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | ElizaOS |
| **Website** | https://elizaos.ai |
| **Contract Address** | `DuMbhu7mvQvqQHGcnikDgb4XegXJRyhUBfdU22uELiZA` |
| **Chain** | Solana |
| **Audit Type** | Token |
| **Mefai Security Score** | **30/100** |
| **Overall Risk** | **HIGH** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research. It represents a point in time analysis based on public information, the project's own published statements, and onchain data available at the time of review. The lines under Claim versus Reality quote each project's own public marketing statements and place next to them what the public evidence shows; those assessments are Mefai Security Research's analysis and opinion based on the cited sources, and are not statements about the private intentions of any team. Findings drawn from litigation or third party reporting are attributed and treated as allegations, not proven facts. Onchain data can change. This report is not investment advice, an endorsement, or a recommendation. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Summary

The open source framework is genuinely excellent and deserves credit. The token is largely detached speculation whose headline autonomous AI story was documented as human operated, now trades roughly 99.9 percent below its all time high on about 15 thousand dollars of liquidity, and is the subject of an active securities fraud class action whose allegations are unproven and may be contested.

## Website claim versus reality
- "AI agents that think, learn, and act autonomously" / Software is real, autonomy is misleading. GitHub `elizaOS/eliza` is real (18,845 stars, 5,591 forks, MIT license, daily commits, arXiv paper 2501.06781). But the paper itself states every aspect of Eliza is a regular TypeScript program under the full control of its user, so agents act under user instruction, not sovereign autonomy.
- Flagship autonomous AI venture fund ("pmairca", "Marc AIndreessen") / Reported and alleged to be human operated, not autonomous. Protos reported (October 28, 2024) that no fund on Daos.fun is actually operated by an AI and that the assets were the human creator's own token. A class action, Pikabea versus Walters (SDNY, 1:26 cv 03238, filed April 20, 2026), alleges that humans actually operated the AI agent and that it was not autonomous as advertised. These are reporting and unproven allegations, not court findings.
- "ai16z" branding tied to Andreessen Horowitz / Unauthorized mimicry that was forced to change. a16z partner Chris Dixon requested the name change, triggering the January 28, 2025 rebrand to elizaOS. The lawsuit alleges the founders named it to mimic a16z and built a "Marc AIndreessen" agent without authorization.
- Token utility and governance / Weak and detached. The official quickstart runs agents with three CLI commands, no token purchase or payment required. The framework is MIT licensed and plugins are free npm packages, so the token gates nothing in the real software.
- "Hard cap of 11,000,000,000 tokens with no emissions" / Partly misleading. Onchain the elizaOS mint authority is ACTIVE (not renounced) and is a one of two SPL multisig, so a single human key can mint. The hard cap is a policy promise, not a protocol enforced cap.

### Onchain contract audit
- elizaOS `DuMbhu...LiZA`: decimals 9, freeze authority null (good), mint authority ACTIVE (one of two multisig, single key can mint), supply about 9.33 billion on Solana.
- Legacy ai16z `HeLp6...jwC`: Token 2022, freeze authority null, mint authority active but a program owned PDA (better custody), supply about 1.1 billion.
- Migration 1 ai16z to 6 elizaOS expanded supply from about 1.1 billion toward an 11 billion cap. About 9.33 billion is minted so far and the mint authority is still active, so the remaining amount can still be minted. The lawsuit frames the expansion as a tenfold dilution with 40 percent to insiders (that percentage is UNVERIFIED against primary docs, only alleged).
- Value collapsed roughly 99.9 percent from an all time high of about 2.6 billion dollars (price 2.47 dollars on January 2, 2025) to about 3 million dollars, on about 15 thousand dollars of onchain liquidity for elizaOS (about 41 thousand dollars combined with legacy ai16z).
- Top holder concentration UNVERIFIED (every free RPC blocked getTokenLargestAccounts, needs a keyed RPC).

### Sources
Solana JSON RPC api.mainnet-beta.solana.com (getAccountInfo, getTokenSupply, getMultipleAccounts); api.geckoterminal.com; GitHub API repos/elizaOS/eliza and stats/contributors; elizaos.ai and docs.elizaos.ai; arxiv.org/abs/2501.06781; protos.com (human operated); Pikabea versus Walters 1:26 cv 03238 (SDNY); coincarp.com and theblock.co and cointelegraph.com (rebrand and migration); coinspeaker.com (2.5 billion all time high).
