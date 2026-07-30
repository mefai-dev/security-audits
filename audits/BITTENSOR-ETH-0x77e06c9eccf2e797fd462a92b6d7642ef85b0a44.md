# Security Audit Report: Bittensor - Bittensor / Subtensor L1

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | 2026-07-30 |
| **Project** | Bittensor |
| **Website** | https://bittensor.com |
| **Contract Address** | `0x77e06c9eccf2e797fd462a92b6d7642ef85b0a44` |
| **Chain** | Bittensor / Subtensor L1 |
| **Audit Type** | Token |
| **Mefai Security Score** | **63/100** |
| **Overall Risk** | **MEDIUM** |

---

## Disclaimer

This report is an independent claim versus reality assessment by Mefai Security Research. It represents a point in time analysis based on public information, the project's own published statements, and onchain data available at the time of review. The lines under Claim versus Reality quote each project's own public marketing statements and place next to them what the public evidence shows; those assessments are Mefai Security Research's analysis and opinion based on the cited sources, and are not statements about the private intentions of any team. Findings drawn from litigation or third party reporting are attributed and treated as allegations, not proven facts. Onchain data can change. This report is not investment advice, an endorsement, or a recommendation. Mefai Security Research assumes no liability for losses arising from reliance on this report. Named projects are welcome to respond, and documented corrections will be published.

---

## Summary

A genuine, roughly three year old network with real revenue producing subnets and Bitcoin style 21 million tokenomics. However independent empirical data shows heavy stake and reward concentration and foundation linked governance control, and rewards correlate far more with stake than with measured value. It is not a scam, it is an overstated but real project.

## Website claim versus reality
- "decentralized machine intelligence network" / Materially overstated. Peer analysis of all 64 subnets (arXiv 2507.02951) finds stake Gini about 0.9825, validator top 1 percent holding 64.53 percent of validator stake, and over half of subnets requiring under 1 percent of wallets to reach 51 percent. The paper documents severe stake concentration and argues the network falls well short of Bitcoin grade decentralization. In April 2026 the subnet developer Covenant AI publicly exited, and its founder Sam Dare described the network's governance as decentralization theatre (his characterization, attributed to him).
- "independent subnets produce digital commodities" / True for a minority, overstated network wide. Chutes (SN64) is a real inference provider serving very high volume on OpenRouter, and reported network AI revenue was about 43 million dollars in the first quarter of 2026, though independent analysis argues genuine external revenue is far lower (roughly 3 to 15 million dollars a year) and heavily subsidized by TAO emissions. But inference revenue is structurally difficult to verify, and many subnets are described as low value zombie subnets.
- "the chain pays participants in proportion to the value they contribute" / Empirically contested, the biggest gap. Validator stake to reward correlation is 0.80 to 0.95 while performance to reward is about 0.50, and for miners performance to reward is only 0.10 to 0.30. Opentensor's own paper documents validators copying weights instead of evaluating.
- Bitcoin style 21 million cap with halvings / TRUE. Max and total supply equal 21 million (CoinGecko), first halving occurred about December 2025, emission cut from 1 to 0.5 TAO per block.
- wTAO "backed 1 to 1 by locked TAO" / Contract real, custody centralized. The Ethereum bridge is run by a single pseudonymous operator who manually signs every redemption. Treat wTAO as high trust custodial.

### Onchain and decentralization audit
- Ethereum wTAO verified across three RPC nodes: name Wrapped TAO, symbol wTAO, 9 decimals, about 114,921 supply (a small community bridge, not the core protocol). The bulk of TAO lives natively on Subtensor.
- Root subnet is a fixed 64 validators; block production historically ran under Opentensor authority nodes; governance via a foundation linked multisig. Full decentralization roadmap targets about December 2027 (unfulfilled).

### Sources
bittensor.com and docs; arxiv.org/html/2507.02951v1 (concentration and reward correlations); Ethereum RPC ethereum-rpc.publicnode.com and eth.drpc.org (wTAO); dlnews.com (bridge single operator); api.coingecko.com (supply); theblock.co and tradingview (Covenant AI exit, decentralization theatre); learnbittensor.org (weight copying); openrouter.ai and taoprotocol.org (subnet revenue).
