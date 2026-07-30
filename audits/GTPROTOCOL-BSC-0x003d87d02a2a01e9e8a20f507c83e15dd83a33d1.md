# Security Audit Report: GT Protocol - BNB Smart Chain (BSC)

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | GT Protocol |
| **Website** | https://www.gt-protocol.io |
| **Contract Address** | `0x003d87d02a2a01e9e8a20f507c83e15dd83a33d1` |
| **Chain** | BNB Smart Chain (BSC) |
| **Audit Type** | Token |
| **Mefai Security Score** | **58/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research. It represents a point in time analysis based on public information, the project's own published statements, and onchain data available at the time of review. The lines under Claim versus Reality quote each project's own public marketing statements and place next to them what the public evidence shows; those assessments are Mefai Security Research's analysis and opinion based on the cited sources, and are not statements about the private intentions of any team. Findings drawn from litigation or third party reporting are attributed and treated as allegations, not proven facts. Onchain data can change. This report is not investment advice, an endorsement, or a recommendation. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Summary

A real, CertiK audited, exchange listed token with clean mechanics (0 tax, sellable, not mintable). The noncustodial claim is broadly true in the correct sense (assets stay on the user's own wallet or on withdrawal disabled exchange API keys), but this is delegated automated execution, not the user signing every trade, and the AI is materially an OpenAI powered automation layer rather than a proprietary alpha engine.

## Website claim versus reality
- "an intuitive non custodial crypto investment experience" / Substantiated with a caveat. The DeFi side is genuine self custody (connect your own wallet). The CeFi side uses trading only, withdrawal disabled exchange API keys, so GT cannot move principal. Caveat: execution is delegated to bots, the user does not sign each trade, so noncustodial of assets is true but "user controls every trade" is misleading. The CeFi architecture is offchain and UNVERIFIABLE.
- "AI powered bots dominate the crypto market" / Overstated label. The token holds zero AI logic onchain. The docs state OpenAI was selected as the primary AI provider, so the AI is a third party LLM layer plus rules based automation, not a proprietary model.
- "CeFi, DeFi, NFT investments" / Partially verified. CeFi and DeFi are plausibly live. NFT execution is UNVERIFIED, absent from the 2026 product surface.
- Audited and KYC / CertiK VERIFIED, Hacken UNVERIFIED. CertiK Skynet lists the project (90.23 score, 2024 vintage). No independent Hacken report was found.

### Onchain contract audit
- name GT Protocol, symbol GTAI, 75 million supply. Ownership NOT renounced (single externally owned account, but limited powers: not mintable, not pausable, no blacklist per GoPlus). is_open_source 1, not a honeypot.
- CRITICAL: DEX LP is NOT locked (99.89 percent of LP held in an externally owned account, is_locked 0), on a thin pool about 33 thousand dollars. Top 10 about 54.4 percent. Microcap about 470 thousand dollars.

### Sources
coingecko.com and coinmarketcap.com; BSC RPC bsc-dataseed.binance.org (name, symbol, owner, getCode); GoPlus token_security chain 56; GeckoTerminal API and page (pool, LP lock); docs.gt-protocol.io via Wayback; gt-protocol.io homepage 2024 and 2026 via Wayback; skynet.certik.com and gt-protocol.medium.com (CertiK).
