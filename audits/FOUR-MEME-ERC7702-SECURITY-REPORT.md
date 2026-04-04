# four.meme Security Report: ERC-7702 Delegation Attack Vector Analysis

A security assessment of [four.meme](https://four.meme/) BSC token launchpad analyzing the ERC-7702 wallet delegation attack surface and identifying areas for security improvement.

## Background

Multiple four.meme users reported complete wallet drainage after interacting with the platform. On-chain forensic investigation traced the attacks to the CrimeEnjoyer ERC-7702 drainer family which has compromised over 450,000 wallets globally.

This report documents the security architecture of four.meme and identifies areas where additional protections could reduce user exposure to ERC-7702 delegation attacks.

## Key Findings

### 1. No Subresource Integrity (SRI) on Any Script

four.meme loads 9+ JavaScript bundles (2MB+ total) without integrity verification.

**Verify yourself:**
```bash
curl -s https://four.meme/ | grep 'integrity='
# Returns nothing. Zero SRI tags.
```

Without SRI a CDN compromise or network-level attacker can modify any JavaScript file silently. The browser executes modified code without detecting the change.

### 2. No Content Security Policy (CSP)

```bash
curl -sI https://four.meme/ | grep -i content-security-policy
# Returns nothing. No CSP header.
```

No CSP means injected JavaScript runs with full page privilleges. No restrictions on inline scripts. No restrictions on eval(). No domain whitelisting.

### 3. Wildcard CORS Configuration

```bash
curl -sI -H "Origin: https://evil.com" https://four.meme/ | grep access-control
```
```
access-control-allow-origin: *
access-control-allow-credentials: true
access-control-allow-methods: PUT, GET, POST, OPTIONS
```

Any website can make authenticated cross-origin requests to four.meme API. A phishing page hosted on any domain can interact with four.meme's backend using the victim's credentials.

### 4. ERC-7702 Signing Functions Pre-loaded

The viem wallet library loaded on all token pages exposes ERC-7702 signing capability:

```bash
curl -s https://four.meme/_next/static/chunks/6802-eba0a6daffa29dc6.js | grep -o 'signAuthorization'
# Returns: signAuthorization

curl -s https://four.meme/_next/static/chunks/6802-eba0a6daffa29dc6.js | grep -o 'prepareAuthorization'  
# Returns: prepareAuthorization

curl -s https://four.meme/_next/static/chunks/6802-eba0a6daffa29dc6.js | grep -o 'authorizationList'
# Returns: authorizationList
```

The `_app` bundle also contains the transaction type serializer that automaticaly converts transactions with `authorizationList` to ERC-7702 type (0x04):

```javascript
// From _app-04e1e4a14ed5708f.js (viem library code)
// Transaction type detection:
if(void 0!==e.authorizationList) return "eip7702";

// Transaction type mapping:
{legacy:"0x0", eip2930:"0x1", eip1559:"0x2", eip4844:"0x3", eip7702:"0x4"}

// Serialization: if authorizationList exists it gets included in the signed transaction
void 0!==e.authorizationList&&(t.authorizationList=e.authorizationList.map(...))
```

four.meme's own page code (create-token.js, token_address.js, index.js) has **zero referrences** to signAuthorization or authorizationList. The functions exist only in the viem library. However because they are loaded in the page context any injected code can call them directly.

### 5. Backend API Returns Transaction Parameters

The token purchase flow relies on a backend API that returns transaction data:

```
POST /v1/private/token/presale/buy
Response: { buyArg: "...", signature: "..." }
```

The frontend uses this response directly in contract calls. If the API response is intercepted or the API server is compromised the returned data could be modified to include malicous parameters.

### 6. Unverified Smart Contracts

four.meme's main contract `0x5c952063c7fc8610FFDB798152D69F0B9550762b` uses an EIP-1967 proxy pattern. The implementation contract is not verified on BscScan. Users canot audit what code their transactions execute.

### 7. Active Phishing Domains

Multiple confusingly similar domains are active:

| Domain | Status | Threat |
|--------|--------|--------|
| 4meme.com | Active | Loads FingerprintJS and redirects users |
| fourmeme.xyz | Active | Redirects to /lander (classic scam pattern) |
| fourmeme.com | Active | Parked with custom HTML |
| four-meme.io | Active | Registered |

Verify:
```bash
dig 4meme.com +short
# Returns: 103.224.212.205

curl -s https://4meme.com/ | head -5
# Shows FingerprintJS loading
```

## On-Chain Evidence

### Victim Wallet Verification

```bash
# Check if a wallet has been compromised by ERC-7702 delegation
curl -s -X POST https://bsc-dataseed1.binance.org \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_getCode",
       "params":["0x8cbf7a53af6b88abb07ba481ac66d73a99985878","latest"],"id":1}'
```

**Response:**
```
0xef010091c478e3a87626be374e0c29338ad68f38556e2c
```

Breakdown:
- `0xef01` = ERC-7702 delegation prefix (normal wallets return `0x`)
- `0091c478e3a87626be374e0c29338ad68f38556e2c` = delegate contract address

### CrimeEnjoyer Drainer Contract

**Delegate:** `0x91c478e3a87626be374e0c29338ad68f38556e2c` (2727 bytes)

Decompiled function table:

| Function | Auth Required | Purpose |
|----------|--------------|---------|
| receive() | None | Auto-forwards incoming BNB to controller |
| fallback() | None | Same as receive. Backup sweep |
| multicall(Call[]) | tx.origin == owner | Executes arbitrary calls from victim wallet |
| selfdestruct() | tx.origin == owner | Destroys evidence. Sends remaining funds |
| name() | None | Returns "wee". CrimeEnjoyer fingerprint |

Controller address `0xa7bff280ba6308ebc8fa3f5a1fa1b455aa57e972` is hardcoded 5 times in bytecode.

### Controller Activity

```bash
curl -s -X POST https://bsc-dataseed1.binance.org \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_getTransactionCount",
       "params":["0xa7bff280ba6308ebc8fa3f5a1fa1b455aa57e972","latest"],"id":1}'
```

**Nonce: 3836+** (approximately 1 transaction every 68 seconds, still activley draining wallets)

## How the Attack Works

```
User visits four.meme token page
         |
   [Cloudflare CDN serves JS]  <-- No SRI = modifiable in transit
         |
   [Browser loads viem with signAuthorization()]
         |
   [User clicks Buy/Sell/Approve]
         |
   [If JS was modified: authorizationList injected into transaction]
         |
   [viem automaticaly sets type to 0x04 (ERC-7702)]
         |
   [MetaMask shows normal-looking transaction]
         |
   [User confirms]
         |
   [Wallet delegated to CrimeEnjoyer]
         |
   [All funds drained. All future deposits auto-forwarded]
```

The chain of conditions: **No SRI + No CSP + viem signAuthorization loaded in page context** creates a surface that could be exploited if any code injection occurs.

## Security Observations

four.meme's own code does NOT call signAuthorization or construct authorizationList. However the security gaps identified below directly contribute to the conditions that make ERC-7702 delegation attacks possible against four.meme users. These issues should be addressed urgently to prevent further wallet compromises:

1. **No SRI** on script tags. Adding integrity hashes would help detect unauthorized CDN modifications
2. **No CSP** header. A content security policy would limit the impact of any code injection
3. **Wildcard CORS** configuration. Restricting origins would reduce cross-origin attack surface
4. **No ERC-7702 warnings** for users. A delegation check could alert users to compromised wallets
5. **No wallet health check** (eth_getCode). Detecting active delegations at connect time would protect users
6. **Unverified contracts** on BscScan. Verification would allow the community to audit the code

## Recommendations for four.meme

### Immediate Actions
1. Add SRI hashes to all `<script>` tags
2. Implement strict CSP header: `script-src 'self'; default-src 'self'`
3. Restrict CORS to `https://four.meme` only
4. Tree-shake viem to remove unused signAuthorization from bundles

### Short Term
5. Add wallet health check: call `eth_getCode` on connected wallet and warn if `0xef01` prefix detected
6. Report phishing domains (4meme.com, fourmeme.xyz) to registrars
7. Verify all smart contracts on BscScan
8. Display ERC-7702 risk warning banner for BSC users

### Long Term
9. Sign API responses to prevent MITM modificaton
10. Implement certificate transparency monitoring
11. Set up phishing domain monitoring service

## How to Check Your Wallet

```javascript
// Run in browser console or Node.js
const provider = new ethers.JsonRpcProvider("https://bsc-dataseed1.binance.org");
const code = await provider.getCode("YOUR_WALLET_ADDRESS");

if (code === "0x") {
    console.log("SAFE: Normal wallet");
} else if (code.startsWith("0xef01")) {
    console.log("COMPROMISED: ERC-7702 delegation active!");
    console.log("Delegate:", "0x" + code.slice(6));
    console.log("DO NOT send any funds to this wallet!");
} else {
    console.log("This is a smart contract, not an EOA");
}
```

## Methodology

- JavaScript bundle analysis (2.3MB+ across 20+ files)
- Direct BSC RPC calls (eth_getCode, eth_getTransactionCount, eth_getBalance)
- CrimeEnjoyer delegate contract reverse engineering
- DNS and certificate transparncy analysis  
- HTTP security header verification
- Build manifest and dependency chain analysis
- Phishing domain enumeration and verification

## Disclaimer

This report was produced for defensive security research purposes. No exploitation was performed. All on-chain data is publicly accesible. All web security tests used standard HTTP requests to public endpoints. This report is intended to support four.meme in strengthening its security posture and helping users stay protected against ERC-7702 delegation attacks.

## Credits

MEFAI Security Research Team
Report Date: April 4, 2026

## References

- [EIP-7702: Set EOA account code](https://eips.ethereum.org/EIPS/eip-7702)
- [BSC Pascal Hardfork (EIP-7702 support)](https://www.bnbchain.org/en/blog/bsc-pascal-hardfork)  
- [Wintermute: ERC-7702 Delegation Analysis](https://wintermute.com)
- [MetaMask Security Advisory: ERC-7702 Phishing](https://metamask.io/news/)
