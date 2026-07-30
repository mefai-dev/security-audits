# Security Audit Report: io.net - Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | io.net |
| **Website** | https://io.net |
| **Contract Address** | `BZLbGTNCSFfoth2GYDtwr7e4imWzpR5jqcUuGEwr646K` |
| **Chain** | Solana |
| **Audit Type** | Token |
| **Mefai Security Score** | **50/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research. It represents a point in time analysis based on public information, the project's own published statements, and onchain data available at the time of review. The lines under Claim versus Reality quote each project's own public marketing statements and place next to them what the public evidence shows; those assessments are Mefai Security Research's analysis and opinion based on the cited sources, and are not statements about the private intentions of any team. Findings drawn from litigation or third party reporting are attributed and treated as allegations, not proven facts. Onchain data can change. This report is not investment advice, an endorsement, or a recommendation. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Summary

A genuine GPU rental marketplace that resells real third party compute at transparent prices, and its token has no rug vector (mint and freeze authorities revoked). However its core metric, GPU count, has a documented history of roughly 40 to 50 times inflation, and its decentralized and open source positioning masks a centralized orchestration backend.

## Website claim versus reality
- "The Open Source AI Infrastructure Platform" / Partly misleading. The GitHub has only worker binaries, a setup script and demos. The network orchestration and scheduling logic is not open sourced and runs on a proprietary centralized backend (confirmed by the April 2024 incident being an internal worker api flaw).
- "a decentralized GPU network that aggregates underutilized GPUs" / Aggregation is real, decentralization is thin. io.net's own homepage inventory shows all internal supplier SKUs sold out and 100 percent of currently rentable compute brokered from independent external suppliers, while scheduling and verification remain centralized.
- "over 320,000 verified GPUs" / Inflated. The same marketing source concedes only close to 7,000 are readily available. Independent analysis put daily verified active GPUs at 6,720 out of 327,000 registered (about 2 percent), declining 11.1 percent quarter over quarter.
- 400,000 spoofed workers / Publicly admitted by io.net. In an April 2024 statement io.net said it was aware of virtual GPU abuse spoofing approximately 400,000 workers. The official incident report admits an api that accepted metadata updates without proper authentication.

### Onchain contract audit
- Solana RPC getAccountInfo: mintAuthority null and freezeAuthority null (both REVOKED, good), supply about 798.8 million of an 800 million cap.
- Top holder concentration UNVERIFIED (every free RPC rate limited getTokenLargestAccounts; verify at solscan.io holders tab).

### Sources
io.net homepage __NEXT_DATA__ (live inventory); x.com/ionet (contract, 400,000 spoofed workers admission); Solana JSON RPC api.mainnet-beta.solana.com (getAccountInfo, getTokenSupply); ionet.medium.com (2024 breach postmortem); ownyourmind.ai (327k registered versus 6,720 active); blocmates.com (320k versus 7,000 available); github.com/ionet-official.
