# Security Audit Report: Fetch.ai - Ethereum

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | Fetch.ai |
| **Website** | https://fetch.ai |
| **Contract Address** | `0xaea46A60368A7bD060eec7DF8CBa43b7EF41Ad85` |
| **Chain** | Ethereum |
| **Audit Type** | Token |
| **Mefai Security Score** | **77/100** |
| **Overall Risk** | **LOW** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research. It represents a point in time analysis based on public information, the project's own published statements, and onchain data available at the time of review. The lines under Claim versus Reality quote each project's own public marketing statements and place next to them what the public evidence shows; those assessments are Mefai Security Research's analysis and opinion based on the cited sources, and are not statements about the private intentions of any team. Findings drawn from litigation or third party reporting are attributed and treated as allegations, not proven facts. Onchain data can change. This report is not investment advice, an endorsement, or a recommendation. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Summary

An established (2017 origin) project whose FET token and 2024 tri token merger genuinely executed onchain, and whose uAgents framework is real, open source and actively maintained. The overstatement is not fraud but hype framing: agent counts describe a directory of mostly offchain hosted services, the alliance is outdated (Ocean left in October 2025), and the ASI rebrand stalled.

## Website claim versus reality
- "One request. Millions of agents ready to act" and "2.7 million agents" / Overstated framing. The 2.7 million figure counts agents registered on Agentverse, an offchain directory platform, not the onchain Almanac registry (which reporting puts near 36,000). Agentverse classifies agents by deployment type (Hosted, Local, Mailbox, Proxy), meaning they are hosted services, not autonomous onchain programs. The uAgents README confirms that, for agents that do register onchain, only identity registration is onchain while execution is offchain. The 2.7 million is a cumulative offchain directory figure.
- "a library for creating autonomous AI agents in Python" / Substantially REAL. github.com/fetchai/uAgents is genuinely open source (Apache 2.0, about 1.6k stars, active). But autonomy is aspirational at scale, and Fetch's own CEO called agent DAO autonomy an overhyped crypto narrative.
- "AGIX and OCEAN merge into FET" at fixed ratios / TRUE and executed. Onchain the FET total supply grew from about 1.15 billion to 2.714 billion, consistent with the merger, which began July 1, 2024.
- Implied completed ASI token and rebrand / NOT completed. Onchain the symbol still returns FET and the name still returns Fetch.
- Merged alliance of Fetch, SingularityNET and Ocean / OUTDATED. Ocean Protocol withdrew on October 9, 2025, with about 270 million OCEAN unconverted. The current alliance is Fetch, SingularityNET and CUDOS.

### Onchain contract audit
- Ethereum FET `0xaea4...Ad85` verified three ways (RPC, Ethplorer, official docs): symbol FET, name Fetch, 18 decimals. totalSupply 2,714,384,546.672 (verified twice). It is an AccessControl mintable ERC 20 (MINTER_ROLE present), so designated role holders can mint, which is exactly the merger mechanism and remains a centralization consideration. About 169,641 holders.
- UNVERIFIED: about 84 million supply above the 2.63 billion target (possibly native staking inflation), not reconciled this session.

### Sources
fetch.ai and network.fetch.ai (official ERC 20 address); fetch.ai merger blog and docs.superintelligence.io (ratios and phases); blog.oceanprotocol.com (Ocean withdrawal); x.com/Fetch_ai (statement on Ocean); github.com/fetchai/uAgents; docs.agentverse.ai; arxiv.org/pdf/2510.18699; etherscan.io and ethplorer.io; Ethereum RPC ethereum-rpc.publicnode.com (symbol, name, totalSupply, EIP 1967 slot).
