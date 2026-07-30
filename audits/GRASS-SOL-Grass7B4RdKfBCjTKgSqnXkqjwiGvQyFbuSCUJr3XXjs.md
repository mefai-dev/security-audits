# Security Audit Report: Grass - Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | Grass |
| **Website** | https://www.grass.io |
| **Contract Address** | `Grass7B4RdKfBCjTKgSqnXkqjwiGvQyFbuSCUJr3XXjs` |
| **Chain** | Solana |
| **Audit Type** | Token |
| **Mefai Security Score** | **58/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research. It represents a point in time analysis based on public information, the project's own published statements, and onchain data available at the time of review. The lines under Claim versus Reality quote each project's own public marketing statements and place next to them what the public evidence shows; those assessments are Mefai Security Research's analysis and opinion based on the cited sources, and are not statements about the private intentions of any team. Findings drawn from litigation or third party reporting are attributed and treated as allegations, not proven facts. Onchain data can change. This report is not investment advice, an endorsement, or a recommendation. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Summary

A real, funded, functioning residential bandwidth and data business with a genuine token and revoked freeze authority, but its decentralized network marketing materially overstates a system that is, by its own docs, run through a single centralized validator, holds most of supply in a few unlabeled wallets, and keeps its mint authority live.

## Website claim versus reality
- "Get rewarded for the internet you don't use" / TRUE. The user acts as a residential IP proxy for verified institutional clients. Mechanism is real.
- "Trusted by over 8.5M users" and node counts (2M to 3M nodes) / UNVERIFIED and self reported. Figures trace back to Grass's own dashboards and conflict across sources. A node is a browser extension install, trivially inflatable.
- "no one, not even us, can access your private information" / Plausible but not fully trustless. The narrow claim (Grass does not read files on your disk) is credible, but the sequencer and validator are centralized, so the operator is a trusted party by design.
- "Grass is a decentralized network" / OVERSTATED, the core gap. Grass's own GitBook validator page states verbatim: initially the validator operates as a singular, centralized entity managing all transactions. An independent review calls it a centralized sequencer and a middleman between clients and PC owners. Distributed last mile IPs plus a centralized brain.

### Onchain contract audit
- Solana RPC: freeze authority null (revoked, good), mint authority ACTIVE and held by a single System owned keypair wallet `31rYartQwHeBMjAe2MgGpffGV57fQY3kug4BDN8tLGqQ` (no multisig or timelock, can still mint), supply about 1 billion (exactly 999,993,128.75).
- Concentration (live per account RPC reads, since getTokenLargestAccounts is rate limited on free endpoints): the single largest holder is about 26 percent of supply (about 259.5 million). Note that the widely quoted CoinCarp figures (top 10 about 75.7 percent, number 1 about 45.9 percent) are from a STALE snapshot that no longer matches live chain (several of those accounts have since shrunk or emptied). Critically, that largest holder account is owned by the SAME keypair that holds the mint authority, so mint power and the top holding sit in one lone key. Disclosed tokenomics (only 10 percent airdropped, rest team, investor, foundation and emissions) explain much of the concentration, with heavy investor unlocks around October 2026.

### Sources
grass.io and grass-foundation.gitbook.io (verbatim validator claim); Solana JSON RPC api.mainnet-beta.solana.com (getAccountInfo, getTokenSupply); coingecko.com and solflare.com (contract); coincarp.com and ainvest.com (holder concentration); tokenomist.ai (allocation); independent DePIN reviews on Medium, Gate and OKX.
