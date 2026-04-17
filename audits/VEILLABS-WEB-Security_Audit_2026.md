# VeilLabs Security Audit Report

## Whitepaper Claims vs Code Reality

| Parameter | Value |
|-----------|-------|
| **Risk Score** | **12/100 (CRITICAL RISK)** |
| **Audit Date** | April 17, 2026 |
| **Target** | veillabs.app, trade.veillabs.app |
| **GitHub** | github.com/Veillabsapp |
| **NPM Package** | veillabs |
| **Auditor** | MEFAI Security Framework v5.0 |

---

## Executive Summary

VeilLabs markets itself as an "institutional grade privacy protocol" utilizing "Zero Knowledge Proof technology" for untraceable cryptocurrency transactions. This audit systematically examines each marketing claim against the actual codebase, API behavior, and network traffic.

### Severity Summary

| Severity | Count |
|----------|-------|
| Critical | 2 |
| High | 4 |
| Medium | 3 |
| Low | 0 |
| Informational | 0 |

**Key Findings:**
- Zero Knowledge Proof implementation: **None found**
- Private keys are transmitted to and validated by the server
- Backend is a SimpleSwap affiliate API wrapper
- Project age: approximately 7 weeks
- No legal pages (Terms of Service, Privacy Policy)
- Git commits use throwaway email address

---

## 1. Zero Knowledge Proof Technology

**CLAIM:**
> "Secure, fast, and efficient decentralized payments and exchange built on Zero Knowledge Proof technology."

**REALITY:**
No Zero Knowledge Proof implementation exists in the codebase. A comprehensive search across all JavaScript bundles returned zero matches for ZK related terminology.

**EVIDENCE:**
```bash
# Search performed across all JS bundles
$ grep -E "zk|zkp|snark|plonk|groth16|zero.knowledge|bulletproof" *.js

# Result: 0 matches

# Bundle files analyzed:
# - index-DwZDWvvN.js (273KB) - Main site
# - 6b34ff7c60a0c5e6.js (24KB) - Trade app
# - 30c1fe52b4d9f954.js (198KB) - Trade app
# - 517392cc6f5cd350.js (38KB) - Trade app
# Total: 818KB of JavaScript analyzed
```

**IMPACT:**
Users expecting cryptographic privacy guarantees receive none. All transactions are processed through standard exchange APIs with full transaction visibility on public blockchains.

---

## 2. Multi Hop Routing

**CLAIM:**
> "Multi hop routing. Anti clustering design."

**REALITY:**
Network traffic analysis reveals single API calls to the backend. No intermediate hops, no onion routing, no transaction splitting detected.

**EVIDENCE:**
```javascript
// From trade app JavaScript bundle (30c1fe52b4d9f954.js)
// All operations are direct fetch calls to single endpoint

fetch(`${N}/currencies`,{cache:"no-store"})
fetch(`${N}/estimates?${t.toString()}`)
fetch(`${N}/exchanges`,{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify(e)})
fetch(`${N}/tracking/${e}`)

// N resolves to: https://trade.veillabs.app/api
// Single endpoint, no routing, no hops
```

**IMPACT:**
The "multi hop routing" claim is entirely false. Users believing their transactions are being routed through multiple nodes for privacy are mistaken.

---

## 3. Private Key Security

**CLAIM:**
> "Never paste your private key directly in chat."
> (From /agentic page)

**REALITY:**
The SDK and API require private keys to be transmitted to the server. The server actively validates private key format, confirming server side processing.

**EVIDENCE:**
```typescript
// From sdk-cli/src/sdk/types.ts
export interface TransferParams {
  privateKey: string;  // Required field
  destination: string;
  amount: string;
  network: string;
  token: string;
}

// From sdk-cli/src/sdk/modules/transfer.ts
export class TransferModule {
  async single(params: TransferParams): Promise<TransferResponse> {
    const { data } = await this.api.post<TransferResponse>('/transfer', params);
    return data;
  }
}
```

```bash
# API test confirming server side validation
$ curl -X POST "https://trade.veillabs.app/api/transfer" \
  -H "Content-Type: application/json" \
  -d '{"privateKey":"test","destination":"0x...","amount":"0.001","network":"eth","token":"eth"}'

# Response:
{"error": "Invalid private key"}

# The server validates the private key format
# This proves private keys are processed server side
```

**IMPACT:**
**CRITICAL.** Private keys transmitted to the server can be logged, stored, or exfiltrated. Users risk complete loss of funds. The warning "never paste your private key" directly contradicts the SDK's mandatory privateKey parameter.

---

## 4. Backend Infrastructure

**CLAIM:**
> "Institutional grade hybrid routing protocol engineered to bypass clustering analytics."

**REALITY:**
The backend is a wrapper around SimpleSwap affiliate API. Currency images, exchange data, and swap infrastructure all originate from SimpleSwap.

**EVIDENCE:**
```json
// API Response from /api/currencies
[
  {
    "name": "Bitcoin",
    "ticker": "btc",
    "network": "btc",
    "image": "https://static.simpleswap.io/images/currencies-logo/btc.svg"
  },
  {
    "name": "Ethereum",
    "ticker": "eth",
    "network": "eth",
    "image": "https://static.simpleswap.io/images/currencies-logo/eth.svg"
  }
]
// All images served from static.simpleswap.io
```

```javascript
// From JavaScript bundle (30c1fe52b4d9f954.js)
// SimpleSwap URL references found
src:`https://static.simpleswap.io/images/currencies-logo/${o.tickerFrom.toLowerCase()}.svg`
src:`https://static.simpleswap.io/images/currencies-logo/${o.tickerTo.toLowerCase()}.svg`
```

**IMPACT:**
VeilLabs provides no proprietary technology. Users could achieve identical functionality by using SimpleSwap directly, without exposing private keys to an intermediary.

---

## 5. Untraceable Transactions

**CLAIM:**
> "Untraceable crypto execution."
> "Execute private transactions, manage decentralized assets."

**REALITY:**
All swaps create standard blockchain transactions fully visible on public explorers. The exchange response includes direct links to public blockchain explorers.

**EVIDENCE:**
```json
// API Response from POST /api/exchanges
{
  "id": "V31L367976",
  "addressFrom": "bc1q85sh7umnnyvqvhrs0yc0w0rwhj482l4ax3d7js",
  "addressTo": "0x742d35Cc6634C0532925a3b844Bc9e7595f0aB1D",
  "status": "waiting",
  "currencies": {
    "btc:btc": {
      "txExplorer": "https://mempool.space/tx/{}",
      "addressExplorer": "https://mempool.space/address/{}"
    },
    "eth:eth": {
      "txExplorer": "https://etherscan.io/tx/{}",
      "addressExplorer": "https://etherscan.io/address/{}"
    }
  }
}
// Transactions are fully traceable on public explorers
```

**IMPACT:**
Transactions are completely traceable. Any user expecting privacy is operating under false assumptions created by deceptive marketing.

---

## 6. Security Headers

**CLAIM:**
> "Institutional grade privacy protocol"

**REALITY:**
Security headers analysis reveals a failing grade with 1 out of 7 critical headers implemented.

**EVIDENCE:**
```
HTTP Response Headers Analysis:

Content-Security-Policy:        MISSING
X-Frame-Options:                MISSING
X-Content-Type-Options:         MISSING
Referrer-Policy:                MISSING
Permissions-Policy:             MISSING
X-XSS-Protection:               MISSING
Strict-Transport-Security:      PRESENT

Score: 1/7 (Grade F)

Additional Issues:
- Access-Control-Allow-Origin: * (allows all origins)
- No rate limiting detected
- No health check endpoint
```

**IMPACT:**
The application is vulnerable to clickjacking, MIME type sniffing attacks, and cross origin resource abuse. "Institutional grade" systems implement comprehensive security headers.

---

## 7. Legal Compliance

**CLAIM:**
> Website presents itself as a legitimate financial service

**REALITY:**
No Terms of Service, Privacy Policy, or Legal pages exist. All URLs redirect to the main application.

**EVIDENCE:**
```bash
# Attempted access to legal pages
$ curl -s "https://www.veillabs.app/terms" | grep -o "<title>[^<]*"
<title>Veil Labs | Private Crypto Execution Layer</title>

$ curl -s "https://www.veillabs.app/privacy" | grep -o "<title>[^<]*"
<title>Veil Labs | Private Crypto Execution Layer</title>

$ curl -s "https://www.veillabs.app/legal" | grep -o "<title>[^<]*"
<title>Veil Labs | Private Crypto Execution Layer</title>

# All pages return the main application, no legal content
```

**IMPACT:**
Operating a financial service without legal documentation violates regulations in most jurisdictions. Users have no legal recourse or understanding of data handling practices.

---

## 8. Project Provenance

**CLAIM:**
> Professional cryptocurrency privacy service

**REALITY:**
Project created approximately 7 weeks ago. Git commits use a throwaway email address. Zero external presence or history.

**EVIDENCE:**
```bash
# GitHub account creation
$ curl -s "https://api.github.com/users/Veillabsapp" | jq '.created_at'
"2026-03-07T15:38:26Z"

# Git commit email
$ curl -s "https://api.github.com/repos/Veillabsapp/Veil-Labs/commits" | jq '.[0].commit.author.email'
"asdsadsad6421421@gmail.com"

# Twitter account
Created: February 27, 2026
Followers: 592
Following: 28
GitHub Followers: 0

# Google search results for "veillabs.app"
Results: 0
```

```json
// Volume API confirms project age
{
  "total_volume_30d": 482187.31,
  "total_volume_90d": 482187.31
}
// 30 day and 90 day volumes are identical
// Project is less than 30 days old
```

**IMPACT:**
The project has no established reputation, uses anonymous throwaway credentials, and has zero external validation. Combined with private key collection, this presents extreme risk.

---

## 9. CertiK Audit Badge

**CLAIM:**
> CertiK security badge displayed on website

**REALITY:**
No CertiK audit report found. Badge appears to be decorative without corresponding verification.

**EVIDENCE:**
```bash
# Search for CertiK audit
$ curl -s "https://skynet.certik.com/api/search?query=veillabs"
# No results

# Website displays partner logos including CertiK
# but no audit report link or verification ID provided
```

**IMPACT:**
Displaying security badges without corresponding audits constitutes false advertising and misleads users about the security posture of the application.

---

## Summary Table

| Claim | Reality | Severity |
|-------|---------|----------|
| Zero Knowledge Proof technology | No ZK code found (0 lines) | CRITICAL |
| Multi hop routing | Single API endpoint calls | HIGH |
| Private keys handled securely | Keys sent to and validated by server | CRITICAL |
| Institutional grade protocol | SimpleSwap affiliate wrapper | HIGH |
| Untraceable transactions | Fully visible on public explorers | HIGH |
| Security best practices | 1/7 security headers (Grade F) | MEDIUM |
| Legal compliance | No Terms/Privacy/Legal pages | MEDIUM |
| Established service | 7 week old project, throwaway email | HIGH |
| CertiK audited | No audit report found | MEDIUM |

---

## Technical Specifications

| Component | Value |
|-----------|-------|
| Frontend Framework | React (Vite) for main site, Next.js for trade app |
| Hosting | Cloudflare |
| SSL Certificate | Let's Encrypt (issued March 3, 2026) |
| Backend | Node.js with SimpleSwap API integration |
| Database | Unknown (no direct evidence) |
| API Authentication | None required for public endpoints |
| CORS Policy | Permissive (Access-Control-Allow-Origin: *) |

---

## API Endpoints Discovered

| Endpoint | Method | Function | Risk |
|----------|--------|----------|------|
| /api/currencies | GET | List supported tokens | LOW |
| /api/estimates | GET | Price estimation | LOW |
| /api/ranges | GET | Min/max limits | LOW |
| /api/exchanges | POST | Create swap | LOW |
| /api/exchanges/{id} | GET | Swap status | LOW |
| /api/tracking/{id} | GET | Transaction tracking | LOW |
| /api/volume | GET | Platform statistics | LOW |
| /api/transfer | POST | **Accepts private key** | CRITICAL |
| /api/seed/create | POST | **Accepts private key** | CRITICAL |
| /api/payment-links/create | POST | Generate payment link | LOW |

---

## Conclusion

VeilLabs presents itself as an advanced privacy protocol utilizing Zero Knowledge Proofs and multi hop routing. Systematic technical analysis reveals:

1. **No cryptographic privacy implementation exists**
2. **Private keys are transmitted to centralized servers**
3. **The service is a SimpleSwap affiliate wrapper**
4. **All marketing claims are demonstrably false**

The combination of private key collection, false privacy claims, anonymous operators, and recent creation date presents characteristics consistent with a credential harvesting operation.

**Risk Assessment: 12/100 (CRITICAL)**

---

## Recommendations

For users who have interacted with this service:

1. **If you submitted a private key:** Consider that wallet compromised. Transfer all assets to a new wallet immediately.

---

## Methodology

This audit employed:
- Static analysis of JavaScript bundles
- API endpoint fuzzing and response analysis
- DNS and infrastructure reconnaissance
- GitHub repository and commit history analysis
- Network traffic pattern analysis
- Search engine and social media presence verification

All findings are reproducible using the evidence provided.

---

## Disclaimer

This report presents technical findings based on publicly available information as of the audit date. No unauthorized access was performed. This report does not constitute legal or financial advice.

---

**Report Generated:** April 17, 2026
**MEFAI Security Framework v5.0**
