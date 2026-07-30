# Security Audit Report: Goatseus Maximus - Solana

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | Goatseus Maximus |
| **Website** |  |
| **Contract Address** | `CzLSujWBLFsSjncfkh59rUFqvafWcY5tzedWJSuypump` |
| **Chain** | Solana |
| **Audit Type** | Token |
| **Mefai Security Score** | **68/100** |
| **Overall Risk** | **LOW** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research. It represents a point in time analysis based on public information, the project's own published statements, and onchain data available at the time of review. The lines under Claim versus Reality quote each project's own public marketing statements and place next to them what the public evidence shows; those assessments are Mefai Security Research's analysis and opinion based on the cited sources, and are not statements about the private intentions of any team. Findings drawn from litigation or third party reporting are attributed and treated as allegations, not proven facts. Onchain data can change. This report is not investment advice, an endorsement, or a recommendation. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Summary

A genuine, technically clean pump.fun memecoin with no mint or freeze authority, immutable metadata, 99.74 percent locked LP and very broad holder distribution, so low rug mechanics. However the central claim that an autonomous AI agent created the token does not survive scrutiny: an anonymous human minted it for under 2 dollars on pump.fun and the AI later endorsed it, and the AI itself is a human reviewed model.

## Claim versus reality
- Onchain metadata: "First meme created by @truth_terminal" / MISLEADING. CoinDesk (Ayrey interview): the AI chatbot did not physically create a memecoin, a follower replied with the offering of the GOAT token. The deploying wallet is a human key, and the token was created by an anonymous developer via pump.fun (deploy cost negligible, on the order of a couple of dollars). Human deployed, AI endorsed after.
- Media framing "autonomous AI agent" and "first AI agent millionaire" / Overstated. CoinDesk: the bot is not fully autonomous, Ayrey reviews the tweets before they go live and wallet decisions are made by a human discussion. Truth Terminal is a Llama 3.1 70B fine tune; the creator himself says it is disingenuous to call it an autonomous agent.
- "I endorse the GOAT token on Solana" / TRUE as stated. This is an endorsement, not authorship. The problem is downstream media collapsing AI endorsed a meme into AI created a token.

### Onchain contract audit
- Solana RPC: mintAuthority null and freezeAuthority null (REVOKED), metadata IMMUTABLE (mutable false), supply about 1 billion, creator balance 0.
- LP 99.74 percent locked (about 1.14 million dollars, Raydium), 247,561 holders (very broad).
- Top 10 about 53.5 percent; the largest single wallet at 18.62 percent is UNVERIFIED (likely exchange custody, no label). RugCheck score in its safest band, no risk flags, not rugged.
- Value collapsed about 99 percent from an approximately 1 billion dollar peak (all time high around mid November 2024; the token launched October 10, 2024) to about 12.7 million dollars, ordinary meme volatility, not a contract exploit.

### Sources
Solana JSON RPC api.mainnet-beta.solana.com (getAccountInfo, getTokenSupply); DexScreener API; pump.fun API (creator wallet, onchain description); RugCheck API (authorities, 247,561 holders, LP 99.74 percent locked); coindesk.com (Ayrey quotes on autonomy); coingecko.com learn (pump.fun launch); lesswrong.com (Llama fine tune, anonymous under 2 dollar deploy).
