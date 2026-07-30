# Security Audit Report: MemeToro - BNB Smart Chain (BSC)

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | MemeToro |
| **Website** | https://memetoro.com |
| **Contract Address** | `0x5c9D5a0deF4ab3ed54fB206D1124Bb847e7688c1` |
| **Chain** | BNB Smart Chain (BSC) |
| **Audit Type** | Presale |
| **Mefai Security Score** | **38/100** |
| **Overall Risk** | **HIGH** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research. It represents a point in time analysis based on public information, the project's own published statements, and onchain data available at the time of review. The lines under Claim versus Reality quote each project's own public marketing statements and place next to them what the public evidence shows; those assessments are Mefai Security Research's analysis and opinion based on the cited sources, and are not statements about the private intentions of any team. Findings drawn from litigation or third party reporting are attributed and treated as allegations, not proven facts. Onchain data can change. This report is not investment advice, an endorsement, or a recommendation. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Summary

A presale stage AI memecoin project. No live tradable token exists yet. The core promise that an advanced AI agent creates viral memecoins is entirely unproven at this pre launch stage, and the site displays a Coinsult logo with no linked audit statement.

## Website claim versus reality
- "Memetoro's advanced AI agent lets users seamlessly create, trade, and invest in safe and fair launched memecoins" / UNPROVEN. Pre launch, no product, no code and no onchain evidence support that an AI creates memecoins. This is a promise, not a demonstrated capability.
- "MemeToro AI creates the next viral memecoins from live data streams, ensuring no developer interference" / UNVERIFIED. No live data pipeline or model is demonstrated or documented.
- "$MT powers every layer of the MemeToro ecosystem" (staking, platform access, prediction markets) / Aspirational. Utility depends entirely on a product that does not exist yet onchain.
- Coinsult audit / UNVERIFIED on site. Press releases claim a Coinsult audit, but the site shows only a logo with no audit statement or link. The contract address is not shown on the site.

### Onchain contract audit
- The presale receiver `0x5c9D...88c1` is a real contract (bytecode present), currently holds 0 BNB (funds swept), nonce 1.
- GoPlus address security is fully clean: no cybercrime, phishing, sanctioned, money laundering or honeypot related flags.
- DexScreener shows no live pair, confirming no tradable liquidity yet (presale only). A full token safety audit is not possible until a live token exists.

### Sources
memetoro.com (verbatim claims); BSC RPC bsc-dataseed.binance.org (eth_getCode, eth_getBalance, eth_getTransactionCount); GoPlus address_security chain 56; DexScreener search API; Benzinga and The Manila Times and cryptonews.net (presale stage and Coinsult claim).
