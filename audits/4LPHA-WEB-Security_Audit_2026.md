# 4lpha AI Security Audit Report

## Report Information

| Field | Value |
|-------|-------|
| **Audit Firm** | Mefai Security Research |
| **Report Date** | May 19, 2026 |
| **Report Version** | 1.1 |
| **Target** | 4lpha.tech and github.com/kann420/4alpha |
| **Chain** | BNB Smart Chain (BSC) |
| **Language** | TypeScript |
| **Audit Type** | dApp and Infrastructure |
| **Methodology** | Manual white box review, automated secret scanning, cross channel claim verification, infrastructure recon |
| **Commit Hash** | abb7027 (main branch, May 19, 2026) |
| **Repository** | github.com/kann420/4alpha |
| **Classification** | Confidential |

---

## Disclaimer

This report represents a point in time security assessment conducted by Mefai Security Research. The findings and recommendations contained herein are based on the information available and the state of the codebase at the time of the audit. This report does not constitute a guarantee that the audited system is free of vulnerabilities or defects. No part of this report should be considered as investment advice, an endorsement, or a recommendation regarding the security of any project, token, or protocol.

The scope of this audit was limited to the public source repository, the public infrastructure of the target domain, and the components explicitly listed in the Scope section. No intrusive or active exploitation testing was performed against the live production service. Findings are classified by severity based on the potential impact and likelihood of exploitation at the time of analysis.

Mefai Security Research assumes no liability for any losses, damages, or adverse consequences resulting from the use of or reliance on this report. The responsibility for implementing fixes and maintaining security lies solely with the project team.

---

## Executive Summary

4lpha AI is a real time meme token intelligence and autonomous trading agent platform for Four.Meme and BNB Smart Chain. The application is a Next.js 16 and TypeScript codebase that combines launchpad data, market data, risk scoring, and large language model reasoning, and executes trades through delegated smart account sessions built on the ZeroDev account abstraction stack, through a Pieverse Claw execution backend, or through a local desk wallet.

This audit covered the public source repository, the full Git commit history across all branches, the marketing and landing assets shipped in the repository, and the public infrastructure of the 4lpha.tech domain. The review focused on the trading and smart account execution core, session delegation and policy scoping, API route authorization, the command execution surface, secret handling, and the consistency of the project claims with the implemented code.

The trading execution core is, on the whole, well engineered. Fund egress endpoints are disabled, the delegated session policy is tightly scoped to a small allowlist of trade actions, every state changing agent and session operation requires an owner wallet signature, error output is sanitized, and a complete Git history secret scan found no leaked credentials. However, the audit found one High severity issue: the OnchainOS token security scan, which the documentation describes as a mandatory control before automated buys, is not enforced anywhere in the execution paths. The audit produced 1 High, 3 Medium, 9 Low, and a set of Informational items. The overall risk is assessed as Medium. Most of the implemented product claims are consistent with the code, with the notable exception of the mandatory scan claim and several inconsistencies in the marketing and landing assets.

### Risk Rating

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 1 |
| Medium | 3 |
| Low | 9 |
| Informational | 7 |
| **Total** | **20** |

### Overall Risk Assessment: Medium

The trading execution core is well designed and the most dangerous capabilities are disabled or tightly scoped. The overall rating is held at Medium by one absent safety control that is advertised as mandatory, by unauthenticated read exposure of user data, and by the unbounded lifetime of delegated automation sessions.

The Mefai Security Score defined in SCORING.md is calibrated for token and smart contract audits, measuring ownership, minting, and liquidity controls. 4lpha has no deployed token and no custom on chain contract, so a numeric token score does not apply. This report uses the severity based rating above.

---

## Scope

### In Scope

| Area | Description |
|------|-------------|
| `app/api/**` | All Next.js API route handlers, including agents, smart account, chat, trade, and claw routes |
| `lib/smart-account/**` | ZeroDev provider, session policy, server executor, session and revoke messages |
| `lib/agents/**` | Owner authorization, agent service, repository, execution, persistence |
| `lib/onchainos/**` | OnchainOS client and the token security scan |
| `lib/runtime/**` | Request security, origin checks, rate limiting, error sanitization |
| `lib/claw/**`, `lib/pieverse/**` | Claw route authentication and the Pieverse purr execution runtime |
| `lib/fourmeme/**`, `lib/gmgn/**` | Provider clients and their command execution paths |
| `landing/`, `landing-preview/`, `4lpha-marketing/` | Marketing and landing assets |
| Git history | All 11 branches and the full commit history, including deleted files |
| Infrastructure | DNS, mail security records, content delivery configuration of 4lpha.tech |

### Out of Scope

- Active or intrusive penetration testing against the live production service
- Third party providers and their APIs (Four.Meme, OnchainOS, GMGN, Birdeye, InsightX, DGrid, ZeroDev, Pieverse)
- Third party npm dependencies beyond version review
- The browser extension wallet itself
- The external `purr` CLI binary
- Deployment platform configuration on Railway

---

## Methodology

### White Box Source Review

- Line by line review of the trading execution core and smart account session logic
- API route authorization and access control analysis
- Command execution and process spawning surface review
- Input validation and error handling review
- Secret handling and key material lifecycle review
- Database query construction review for injection safety

### Automated Analysis

- Full Git history secret scan with gitleaks across all branches
- Recovery and review of files deleted from the working tree but still present in history
- Repository wide pattern scan for dangerous primitives (dynamic evaluation, raw HTML injection, process spawning, SQL string construction)
- Commit authorship and provenance analysis

### Cross Channel Claim Verification

- Comparison of the project claims in the repository documentation, the landing and marketing assets, and the public profile against the implemented code

### Infrastructure Recon

- DNS record enumeration (A, AAAA, MX, TXT, NS, CAA)
- Mail security posture review (SPF, DMARC, DKIM)
- Content delivery and edge protection fingerprinting
- Certificate transparency review

---

## Architecture Overview

4lpha is a single Next.js application that runs the web user interface, the API layer, and a long lived autonomous agent worker. The major components are:

- **Web and API layer.** Next.js 16 App Router. API routes serve token intelligence, the Pulse feed, the Copilot chat, agent management, and smart account session management.
- **Agent runtime.** A long lived Node worker evaluates tokens, scores risk, and executes trades. A separate learning worker mines lessons from trade outcomes.
- **Execution targets.** An agent can execute through three providers: a ZeroDev smart account with a signed delegated session, a Pieverse Claw backend that shells out to a `purr` CLI, or a local desk wallet.
- **Data providers.** Four.Meme, OnchainOS, GMGN, Birdeye, InsightX, and Pokebook supply market and risk data. DGrid is the primary large language model gateway.
- **Persistence.** PostgreSQL stores agent state and smart account sessions. A local JSON store is used as a development fallback.

The trust model is non custodial. The user keeps the owner wallet. The delegated session grants the server executor a narrow, signed permission to perform buy and sell calls on two known venues, the Four.Meme TokenManager and the PancakeSwap V2 router, within signed spend and slippage limits.

---

## Findings

### Finding 1: OnchainOS pre buy security scan is documented as mandatory but is not enforced

| Attribute | Value |
|-----------|-------|
| **Severity** | High |
| **Type** | Missing Security Control / Business Logic |
| **Location** | `lib/onchainos/security.ts`, `lib/smart-account/serverExecutor.ts`, `lib/agents/execution.ts` |
| **Status** | Open |

**Description:**

The project documentation states in several places that the OnchainOS token security scan is mandatory before automated buys. This appears in `README.md`, in the recovered `AGENTS.md`, and in `docs/claw-autonomous-deploy.md`. The smart account session policy parser reinforces this by forcing `requireOnchainosTokenScan` to the literal value `true` and rejecting any other value.

Despite this, the scan is not enforced as a blocking control in any execution path.

`evaluateOnchainosBuySecurity` in `lib/onchainos/security.ts` only ever returns a verdict whose `action` field is `allow` or `warn`. There is no blocking outcome. A token that the scan flags as a honeypot, as dumping, as fake liquidity, or as HIGH or CRITICAL risk still produces only `action: "warn"`. The function also catches all of its own errors internally and never throws.

The function is referenced in only two places in the codebase:

- In `lib/smart-account/serverExecutor.ts`, `assertSessionPolicyAllowsPreparedTrade` calls `await evaluateOnchainosBuySecurity(preparedTrade.tokenAddress, "bsc")` when `requireOnchainosTokenScan` is set and the side is a buy. The return value is never assigned or inspected. Because the function never throws, this line has no effect on whether the trade proceeds.
- In `lib/ai/copilotTrade.ts`, the verdict is passed to `readAdvisorySecurityWarning` and surfaced to the user as a Copilot chat advisory string only.

The autonomous agent buy path `executeAgentBuy` in `lib/agents/execution.ts` does not call the scan at all for the Pieverse Claw provider or the local desk wallet provider, and routes the smart account provider through the discard path described above.

**Impact:**

The control that the documentation and the policy describe as a mandatory protection against automated buys of honeypot and high risk tokens does not stop anything. An autonomous agent running in live mode can buy a token that OnchainOS would flag as a honeypot or as critical risk. Honeypot and rug tokens are the single most common hazard in the meme token segment, so this is a routine condition rather than an edge case. The loss per incident is bounded by the per trade size in the session policy, but combined with Finding 3, where automation sessions allow an unlimited trade count, an agent can repeatedly enter flagged tokens. The issue also directly contradicts a security guarantee stated in the project documentation.

**Recommendation:**

Give `evaluateOnchainosBuySecurity` a blocking outcome. Treat HIGH and CRITICAL risk, honeypot, liquidity removal, and fake liquidity results as a hard block. In `assertSessionPolicyAllowsPreparedTrade`, and in every provider branch of `executeAgentBuy`, inspect the verdict and refuse the trade when the verdict is blocking. Decide deliberately how to treat an unavailable scan: failing closed for live execution is the safer default. Apply the same gate to the Pieverse Claw and desk wallet execution paths so the control is uniform across all execution targets.

---

### Finding 2: Unauthenticated read access to all agents, positions, and wallet addresses

| Attribute | Value |
|-----------|-------|
| **Severity** | Medium |
| **Type** | Broken Access Control / Information Disclosure |
| **Location** | `app/api/agents/route.ts` (GET), `app/api/agents/[id]/route.ts` (GET) |
| **Status** | Open |

**Description:**

The agent listing route `GET /api/agents` and the agent detail route `GET /api/agents/[id]` perform no owner authorization. Every write route in the same area calls `assertAgentOwnerAuthorized` and requires an owner wallet signature, but the two read routes do not.

`listAgentsService` returns every agent in the system together with aggregate statistics and a `wallets` array. `getAgentDetailService` returns the full definition of any agent by id, including strategy configuration, open positions, profit and loss, the owner wallet address, and the smart account address.

**Impact:**

Any unauthenticated visitor can enumerate every user of the platform, read their trading strategies and live positions, observe their realized and unrealized profit and loss, and link owner wallet addresses to smart account addresses. This is a privacy and operational data exposure. It also enables targeted abuse, since an observer learns the exact entry thresholds, capital limits, and holdings of other users.

**Recommendation:**

Require owner authorization on the read routes as well, scoped to the requesting wallet. The listing route should return only agents owned by the authenticated wallet. The detail route should verify that the requester owns the agent before returning its definition and positions. If a public discovery surface is intended, expose only a deliberately reduced and non sensitive projection.

---

### Finding 3: Delegated automation sessions have no expiry and no cumulative spend ceiling

| Attribute | Value |
|-----------|-------|
| **Severity** | Medium |
| **Type** | Business Logic / Delegation Scope |
| **Location** | `lib/smart-account/sessionPolicy.ts`, `lib/smart-account/serverExecutor.ts`, `app/api/smart-account/session-intent/route.ts` |
| **Status** | Open |

**Description:**

For automation sessions of kind `agent`, the default policy built in `session-intent` sets `expiresAt` to the constant `SMART_ACCOUNT_AGENT_SESSION_EXPIRES_AT_MS`, which is `99999999999000`, a timestamp in the year 5138. The same default sets `maxTradesPerSession` to `SMART_ACCOUNT_UNLIMITED_TRADES_PER_SESSION`, which is `Number.MAX_SAFE_INTEGER`.

In addition, the spend limit `maxSpendWei` is enforced per trade, not cumulatively. In `getPreparedTradeAutoConfirmCoverage` and `assertSessionPolicyAllowsPreparedTrade`, the check is `policySpendWei > maxSpendWei` for each individual prepared trade. The session does not track total spend across its lifetime.

The combination means an agent automation session never expires, can perform an unlimited number of trades, and has no cap on total funds moved over its lifetime. Only the per trade ceiling applies.

**Impact:**

The lifetime exposure of a delegated automation session is unbounded. The on chain ZeroDev policy still restricts actions to buy and sell calls on the Four.Meme TokenManager and the PancakeSwap V2 router, with the trade recipient forced to be the smart account, so a delegated session cannot transfer funds to an arbitrary address. The realistic worst case is value erosion rather than a direct transfer out: a compromised executor key or a faulty agent could repeatedly trade the smart account balance, one capped trade at a time, with no time bound and no aggregate limit, until the owner manually revokes the session. A non expiring delegation also widens the window in which a compromised executor key remains usable.

**Recommendation:**

Introduce a bounded maximum expiry for automation sessions, for example 30 or 90 days, and require the owner to renew. Track cumulative spend per session and reject trades once the signed `maxSpendWei` is reached as a lifetime budget rather than a per trade ceiling. Consider a separate explicit per trade field so the per trade and lifetime limits are both signed and both clear to the user.

---

### Finding 4: Executor private key and session details are stored unencrypted at rest by default

| Attribute | Value |
|-----------|-------|
| **Severity** | Medium |
| **Type** | Sensitive Data at Rest / Configuration |
| **Location** | `lib/smart-account/serverExecutor.ts` (`readStoreEncryptionKey`, `serializeStore`), `.env.example` |
| **Status** | Open |

**Description:**

The smart account executor store holds the executor private key and the delegated session details. Encryption of this store is opt in. `readStoreEncryptionKey` derives an AES 256 GCM key from `SMART_ACCOUNT_STORE_SECRET`, or falls back to `EXECUTOR_PRIVATE_KEY` if that value is present. When neither value is set, `readStoreEncryptionKey` returns null and `serializeStore` writes the executor private key and session details in plaintext into the store, which is either a Postgres row or a local JSON file.

The variable `SMART_ACCOUNT_STORE_SECRET` is not present in `.env.example`. A deployment that follows the documented example file will not set it. If `EXECUTOR_PRIVATE_KEY` is also unset, the system generates an executor key at runtime and persists it unencrypted.

**Impact:**

In a standard deployment that follows the example environment file, sensitive key material sits unencrypted at rest. Anyone with read access to the database or the deployment filesystem, including through a backup, a snapshot, a logging integration, or a separate database exposure, obtains the executor private key and the session approvals.

**Recommendation:**

Make encryption mandatory. Require `SMART_ACCOUNT_STORE_SECRET` at startup and fail closed if it is missing in production. Document the variable in `.env.example` and in the deployment guide. Avoid deriving the store encryption key from the executor private key, since that ties two distinct secrets together.

---

### Finding 5: Runtime state snapshot committed to public Git history

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Information Disclosure / Process |
| **Location** | `tmp_agents_snapshot.json`, commit `87cc6b7` (added) and `da0f4d6` (removed) |
| **Status** | Open |

**Description:**

A file named `tmp_agents_snapshot.json` was committed to the public repository in commit `87cc6b7` and removed in commit `da0f4d6`, whose message is `fix gitleaks: remove runtime snapshot, suppress doc placeholders`. The file is 367 kilobytes and contains a live runtime snapshot of agents and positions, including agent strategy configuration, execution mode, open positions, traded token addresses, capital limits, and profit and loss values. Removing a file from the working tree does not remove it from history. The snapshot remains fully recoverable from history.

A targeted secret scan of the recovered file with gitleaks and manual review found no credentials, private keys, API keys, or wallet material in it. The exposure is operational data, not secret material.

**Impact:**

The exposure itself is limited to operational and trading data of a development or test agent. The more significant point is the process gap: a runtime artifact reached version control and was caught only after the fact by the secret scanning workflow.

**Recommendation:**

Add runtime and temporary artifact patterns such as `tmp_*` and `*_snapshot.json` to `.gitignore`. Keep runtime state out of the repository working directory. If the team considers the historical operational data sensitive, rewrite history to purge the blob, since it remains publicly recoverable until then.

---

### Finding 6: Owner authorization replay window with no single use nonce

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Replay Protection |
| **Location** | `lib/agents/ownerAuth.ts` (`verifyOwnerSignature`) |
| **Status** | Open |

**Description:**

Agent owner authorization is signature based and well bound. The signed message includes the action, the agent id, the issued timestamp, the owner wallet address, the smart account address, and a canonical hash of the request body. The signature is verified against the owner wallet recorded on the agent. This correctly prevents a signature for one agent or one action from being reused on another.

Replay protection, however, relies only on a time window. A signed request is accepted if its `issuedAt` is within 5 minutes in the past and 30 seconds in the future. There is no single use nonce and no record of consumed signatures.

**Impact:**

Within a 5 minute window, an identical signed request can be replayed. The agent mutation actions are largely idempotent, which keeps the impact low. An attacker who captures a signed request, for example through access to logs that record request bodies, could replay it within the window.

**Recommendation:**

Add a single use nonce to the signed owner authorization message and reject any nonce that has already been consumed. This is the same pattern already used correctly for smart account session confirmation.

---

### Finding 7: Same origin enforcement derives the allowlist from request headers

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Cross Site Request Forgery Defense |
| **Location** | `lib/runtime/requestOrigin.ts` (`collectRequestOrigins`) |
| **Status** | Open |

**Description:**

`requireSameOriginRequest` compares the `Origin` header against a set of allowed origins. That set is built by `collectRequestOrigins` from request controlled headers: the `Forwarded` header, `X-Forwarded-Proto`, `X-Forwarded-Host`, and `Host`. There is no comparison against a fixed canonical origin configured for the deployment.

**Impact:**

For the standard browser cross site request forgery scenario the check still holds, because a browser sets the `Host` header to the real target host and the attacker cannot override it. The weakness is architectural and proxy dependent. If the application is ever placed behind a proxy that does not strip or normalize forwarding headers, an attacker able to influence `X-Forwarded-Host` or `Forwarded` could inject an origin into the allowed set.

**Recommendation:**

Pin the allowed origin to a configured canonical value, for example derived from `NEXT_PUBLIC_APP_URL`, and treat forwarded headers as untrusted input.

---

### Finding 8: Rate limiting is keyed on a spoofable client identifier and held in process memory

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Rate Limiting / Denial of Service Resistance |
| **Location** | `lib/runtime/requestSecurity.ts` (`readRateLimitClientKey`, `enforceInMemoryRateLimit`) |
| **Status** | Open |

**Description:**

`readRateLimitClientKey` builds the rate limit key from the first value of `X-Forwarded-For`, then `X-Real-IP`, then `Host`. The `X-Forwarded-For` header is client controlled. The limiter store is an in process map, so limits are per instance and reset on restart.

**Impact:**

An attacker can rotate the `X-Forwarded-For` header to obtain a fresh rate limit bucket on every request, defeating the limit. On a multi instance deployment the limit is also fragmented across instances.

**Recommendation:**

Derive the client identifier from a trusted source, for example the connecting address provided by the hosting platform. For production use, back the limiter with a shared store such as Redis so limits hold across instances and restarts.

---

### Finding 9: Executor private key auto generated when not provided, and reused as an HMAC key

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Key Management |
| **Location** | `lib/smart-account/serverExecutor.ts` (`readStore`, `readDatabaseStore`, `signBrowserSessionToken`) |
| **Status** | Open |

**Description:**

When `EXECUTOR_PRIVATE_KEY` is not set in the environment, `readStore` and `readDatabaseStore` call `generatePrivateKey()` and persist the generated key into the store, so the executor key is created silently rather than provisioned deliberately. Separately, `signBrowserSessionToken` uses the executor private key directly as the HMAC key for browser session tokens, so one secret is used for two distinct cryptographic purposes.

**Impact:**

A silently generated executor key can change unexpectedly between deployments if the store is not durable, which can orphan sessions. Reusing one secret for two cryptographic purposes is a key hygiene weakness. Neither issue is directly exploitable on its own.

**Recommendation:**

Require `EXECUTOR_PRIVATE_KEY` to be provisioned explicitly as a platform secret in production and fail closed if it is missing. Derive a separate, dedicated key for the browser session token HMAC through a key derivation function with a distinct label.

---

### Finding 10: Pieverse WSL execution path passes the binding API token as a process argument

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Sensitive Data Exposure |
| **Location** | `lib/pieverse/runtime.ts` (`resolvePurrCommand`) |
| **Status** | Open |

**Description:**

The Pieverse runtime executes the external `purr` CLI through `execFile` with array arguments, which is the safe pattern and avoids shell injection. In the standard path, the binding credentials are passed through the `env` option, which is correct.

In the Windows WSL path, however, `resolvePurrCommand` builds the argument array as `wsl.exe -d <distro> -- env INSTANCE_ID=<id> WALLET_API_TOKEN=<token> WALLET_API_URL=<url> <purr> ...`. The binding API token is placed on the command line as an argument to `wsl.exe`, rather than in the process environment.

**Impact:**

Command line arguments are visible to any local process and in process listings on the host. In the WSL path the Pieverse binding API token is therefore exposed to local observers. The standard path is not affected. The WSL path is a local development scenario, which keeps the severity low.

**Recommendation:**

Pass the binding credentials to the WSL process through the environment rather than as `env KEY=VALUE` arguments on the command line, consistent with the non WSL path.

---

### Finding 11: Windows gmgn CLI path does not escape command shell metacharacters

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Command Injection Surface |
| **Location** | `lib/gmgn/client.ts` (`executeCli`, `quoteWindowsArg`) |
| **Status** | Open |

**Description:**

The GMGN client runs the `gmgn-cli` tool. On non Windows platforms it uses `execFile` with an argument array, which is safe. On Windows it builds a single command string and runs it through `cmd.exe /d /s /c <command>`. The helper `quoteWindowsArg` only wraps an argument in quotes when the argument contains whitespace or a double quote. It does not neutralize command shell metacharacters such as `&`, `|`, `>`, `<`, and `^`. An argument that contains one of these characters and no whitespace would be passed to `cmd.exe` unquoted.

**Impact:**

If an attacker controlled value that contains command shell metacharacters reaches a `gmgn-cli` argument on a Windows host, it could inject a command into the `cmd.exe` invocation. Production runs on Linux, where the safe argument array form is used, which limits the practical impact. The Windows path is a local development scenario.

**Recommendation:**

Avoid constructing a `cmd.exe` command string. Invoke the CLI directly with `execFile` and an argument array on Windows as well, or use a quoting routine that escapes the full set of `cmd.exe` metacharacters.

---

### Finding 12: Domain has no DMARC policy and no CAA record

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | Email Security / Infrastructure |
| **Location** | DNS for `4lpha.tech` |
| **Status** | Open |

**Description:**

The domain `4lpha.tech` publishes an SPF record with a soft fail qualifier (`~all`) through Namecheap email forwarding. It publishes no DMARC record at `_dmarc.4lpha.tech` and no CAA record.

**Impact:**

Without a DMARC policy, receiving mail servers have no instruction to reject or quarantine messages that fail authentication, which makes it easier to spoof email from the domain for phishing. Without a CAA record, any certificate authority can issue certificates for the domain.

**Recommendation:**

Publish a DMARC record, starting at `p=none` for monitoring and moving to `p=quarantine` or `p=reject` once mail flows are confirmed. Tighten SPF from `~all` toward `-all`. Publish a CAA record that lists only the intended certificate authorities.

---

### Finding 13: Wildcard DNS record points to a private address

| Attribute | Value |
|-----------|-------|
| **Severity** | Low |
| **Type** | DNS Configuration |
| **Location** | DNS for `*.4lpha.tech` |
| **Status** | Open |

**Description:**

A wildcard DNS record `*.4lpha.tech` resolves to `192.168.1.1`, a private RFC 1918 address. Every undefined subdomain resolves to this private address.

**Impact:**

The impact is low. Pointing a public hostname at a private address is a configuration hygiene issue. It can produce confusing behavior and can direct clients on a network where `192.168.1.1` is the local gateway at their own router.

**Recommendation:**

Remove the wildcard record, or point it at a controlled host that returns a safe response. Define only the subdomains that are in use.

---

## Additional Observations

These items are Informational. They do not represent direct vulnerabilities but are recorded for awareness and defense in depth.

1. **Marketing and landing assets are inconsistent with the product.** The `landing/` and `4lpha-marketing/` pages use placeholder demo data, which is normal for a pre launch landing page. Several details are inconsistent and should be corrected before launch: the call to action references the domain `4alpha.ai`, while the live domain is `4lpha.tech`, the repository is `4alpha`, and the social handle is `4lpha_agent`; the Pulse feed examples are Solana meme tokens such as `$WIF` and `$MOODENG`, while the product operates on BNB Smart Chain and Four.Meme; the marketing positions table shows `SHORT` positions, while the implemented product only performs spot buy and sell; the boot sequence claims a `2.4B params` neural network, while the product uses an external large language model gateway (DGrid) and does not run its own model.

2. **Trade execution relies on the SameSite cookie attribute for cross site request forgery defense.** The route `app/api/chat/trade/execute/route.ts` authenticates with the `alpha_smart_account_session` cookie and performs no explicit origin check, unlike the JSON RPC proxy route. The cookie is set with `SameSite=Lax`, `HttpOnly`, and `Secure` in production, which does mitigate cross site request forgery. Adding an explicit origin check would provide defense in depth.

3. **The Claw control plane authorizes by a single shared token.** The `/api/claw/**` routes are protected by `requireClawRouteAuth`, which checks a shared `CLAW_API_TOKEN` or `CLAW_WEBHOOK_TOKEN`. These routes can arm, pause, and control any agent regardless of the per owner signature checks used on the standard agent routes. The shared token is effectively an operator master key over all agents. Treat it as a high privilege secret and rotate it independently, as the deployment guide already recommends.

4. **Pieverse binding secret references.** `resolveBindingToken` validates only that `apiTokenSecretRef` is a valid uppercase environment variable name and then reads that variable. Confirm that binding creation enforces the `CLAW_ALLOWED_PIEVERSE_SECRET_REFS` allowlist so a binding cannot be pointed at an unrelated server secret.

5. **Large language model prompt injection is bounded by the session policy.** The Copilot chat can request trade preparation and execution. A prompt injected model cannot exceed the signed session policy, since `executeSmartAccountPreparedTrade` independently validates the prepared trade, the session coverage, the spend limits, and the action allowlist. The risk is bounded but remains a defense in depth consideration as agent autonomy grows.

6. **Dead deprecated code.** `lib/agents/db.ts` exports a deprecated `sql` template helper that builds SQL by string concatenation. A repository wide scan found no remaining usage of it. All live queries use the parameterized `agentQuery` helper. Remove the deprecated helper so it cannot be reintroduced into a query path.

7. **Internal team documents and provenance.** `AGENTS.md`, `CLAUDE.md`, and `TEAM-WORKFLOW.md` were committed and later removed in commit `5071221`. They contain process guidance and no secrets, but they remain in history and disclose team member roles. The marketing pages also load third party scripts from public content delivery networks without Subresource Integrity attributes.

---

## Cross Channel Claim Verification

This section verifies the public and documentation claims of the project against the implemented code. Most implemented claims are consistent. The exceptions are recorded below.

| Claim | Code Reality | Verdict |
|-------|--------------|---------|
| AI agent platform for meme token analysis and trading execution | The repository implements a Pulse feed, token intelligence, autonomous agents, risk scoring, and three execution providers. The product matches the description. | Consistent |
| No token has been launched yet | No token contract, no token address in client configuration, and no on chain token were found for the project. | Consistent |
| No local private key is required, and private keys are not exposed to the client | No private key handling exists in client code, and no secret is placed in a `NEXT_PUBLIC_` variable. The model is non custodial. | Consistent |
| Agents can run in paper mode or live mode | The agent definition and execution mode support both. | Consistent |
| OnchainOS token scan is mandatory before automated buys | The scan function only returns `allow` or `warn`, never a blocking outcome. Its one call in the execution path discards the result, and the autonomous buy path does not call it. No execution path refuses a trade based on it. | Contradicted, see Finding 1 |
| Marketing claims and product domain | The marketing assets reference `4alpha.ai`, show Solana tokens and short positions, and claim a self hosted neural network. None of these match the deployed product. | Inconsistent, see Additional Observations item 1 |

The full Git history secret scan supports the security posture of the codebase. A scan with gitleaks across all branches produced 476 raw matches, and manual triage confirmed that all 476 are false positives: public BNB Chain contract addresses, agent cycle correlation identifiers, transaction hashes, and empty environment variable names in documentation. No private key, API key, RPC credential, or wallet seed was found anywhere in the history.

---

## Positive Observations

The following design and implementation choices were verified during the audit and represent good security practice:

- **The delegated session policy is tightly scoped.** A delegated session may only call buy and sell functions on the Four.Meme TokenManager and the PancakeSwap V2 router. The `approve` action is restricted by a parameter condition so the spender can only be one of those two routers. A session cannot perform an arbitrary transfer or an arbitrary contract call.
- **Fund egress endpoints are disabled.** The `withdraw`, `direct-send`, `export-key`, and `create-session` routes return controlled unavailable responses rather than moving funds.
- **Prepared trade validation is thorough.** `assertPreparedTradeIntegrity` decodes every transaction step and checks every argument against the quote, including the token, the amounts, the minimum receive floor, and the recipient, which must be the smart account.
- **Agent ownership is enforced on write routes.** Every state changing agent route verifies an owner wallet signature bound to the action, the agent id, and the request body.
- **Session confirmation is robust.** Session creation requires an owner signature over the full policy and consumes a single use nonce.
- **The command execution surface is mostly safe.** The Pieverse runtime and the Four.Meme client use `execFile` with argument arrays, and the Four.Meme Windows path quotes arguments correctly for PowerShell single quoted strings.
- **Database access is parameterized.** All live queries use the parameterized `pg` query helper.
- **Error output is sanitized.** `sanitizeSecretLikeText` redacts configured secret values and common key patterns before any error reaches a client.
- **Claw route authentication fails closed.** In production, claw routes reject requests when no token is configured.
- **A kill switch is implemented** for owners to disable execution.
- **A secret scanning workflow** runs on push, on pull request, and on a schedule.

---

## Findings Summary

| ID | Title | Severity | Status |
|----|-------|----------|--------|
| 1 | OnchainOS pre buy security scan is documented as mandatory but is not enforced | High | Open |
| 2 | Unauthenticated read access to all agents, positions, and wallet addresses | Medium | Open |
| 3 | Delegated automation sessions have no expiry and no cumulative spend ceiling | Medium | Open |
| 4 | Executor private key and session details stored unencrypted at rest by default | Medium | Open |
| 5 | Runtime state snapshot committed to public Git history | Low | Open |
| 6 | Owner authorization replay window with no single use nonce | Low | Open |
| 7 | Same origin enforcement derives the allowlist from request headers | Low | Open |
| 8 | Rate limiting keyed on a spoofable client identifier and held in process memory | Low | Open |
| 9 | Executor private key auto generated when not provided, and reused as an HMAC key | Low | Open |
| 10 | Pieverse WSL execution path passes the binding API token as a process argument | Low | Open |
| 11 | Windows gmgn CLI path does not escape command shell metacharacters | Low | Open |
| 12 | Domain has no DMARC policy and no CAA record | Low | Open |
| 13 | Wildcard DNS record points to a private address | Low | Open |

---

## Recommended Remediation Priority

1. Enforce the OnchainOS scan as a real blocking control across all execution paths (Finding 1).
2. Add owner authorization to the agent read routes (Finding 2).
3. Make store encryption mandatory and document `SMART_ACCOUNT_STORE_SECRET` (Finding 4).
4. Bound automation session expiry and add a cumulative spend ceiling (Finding 3).
5. Add a single use nonce to owner authorization, pin the same origin allowlist, and provision the executor key explicitly (Findings 6, 7, 9).
6. Harden the command execution paths, fix the DNS and mail records, and purge or ignore runtime artifacts (Findings 5, 10, 11, 12, 13).
7. Correct the marketing and landing inconsistencies before launch (Additional Observations item 1).

---

## Appendix A: Severity Classification

| Severity | Description |
|----------|-------------|
| **Critical** | Direct loss of funds, complete takeover, or irreversible systemic damage. Exploitation requires minimal effort. |
| **High** | Significant risk to user funds, integrity, or availability. Exploitation is feasible with moderate effort or under realistic conditions. |
| **Medium** | Conditional risk requiring specific circumstances or a combination of factors. Material impact if triggered. |
| **Low** | Minor issues, best practice deviations, or theoretical risks with low probability and limited impact. |
| **Informational** | Observations, hardening suggestions, and defense in depth notes with no direct security impact. |

---

## Appendix B: Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| gitleaks | 8.30.1 | Full Git history secret scanning |
| git | 2.51.2 | History review and deleted file recovery |
| Manual review | N/A | Line by line source code audit |
| DNS resolution | N/A | Infrastructure and mail security recon |

---

## Appendix C: Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | May 19, 2026 | Mefai Security Research | Initial report |
| 1.1 | May 19, 2026 | Mefai Security Research | Deepened review: added Finding 1 on OnchainOS scan enforcement, Findings 10 and 11 on the command execution surface, cross channel claim verification, and additional observations |

---

## Contact

**Mefai Security Research**
- Web: [mefai.io](https://mefai.io)
- GitHub: [github.com/mefai-dev](https://github.com/mefai-dev)

---

*This report was prepared by Mefai Security Research. Unauthorized distribution or modification of this document is prohibited without prior written consent.*
