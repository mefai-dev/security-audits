# ICE/ION: Whitepaper Claims vs Code Reality

**Score: 34/100 - HIGH RISK**

**Date:** 2026-04-12
**Chain:** ION (TON Fork)
**Website:** ice.io

---

## Severity Summary

| Severity | Count |
|----------|-------|
| Critical | 5 |
| High | 6 |
| Medium | 2 |
| Low | 0 |
| Informational | 0 |

---

## Why This Report Exists

We are frustrated by the lies told to users. Projects make bold security claims in their whitepapers while their actual code tells a completely different story. This report documents every claim ICE Open Network makes and compares it against what we found in their public source code.

We are not making accusations. We are not spreading FUD. We are reading the source code and showing what is actually implemented.

---

## The Foundation: A Rebranded Fork

**CLAIM:** ICE Open Network is an original blockchain.

**REALITY:** It is a TON (The Open Network) fork with cosmetic changes.

**EVIDENCE:**
```cpp
// ion/tddb/test/io-bench.cpp:2
This file is part of TON Blockchain source code.

// ion/storage/db.h:2
This file is part of TON Blockchain Library.
```

```
$ git log --oneline -3
205a86e rebranding ton to ion
2e8e0e0 Merge pull request #6 from ice-blockchain/upstream-sync
dda011d Merge branch 'master' into upstream-sync
```

**IMPACT:** You cannot change the tires of a car and claim you built the car yourself.

---

## Claim 1: Quantum-Resistant Encryption

**CLAIM:**
> "strong quantum resistant encryption to secure personal data"

**LIE:** There is no quantum-resistant cryptography. Standard Ed25519 is used.

**EVIDENCE:**
```yaml
# heimdall/application.yaml:163-165
identityKeypairs:
  - 97da572f...HIDDEN BY MEFAI...dccab44e
  - 0ba58ed6...HIDDEN BY MEFAI...67ced4a4
```

**IMPACT:** Ed25519 is a standard elliptic curve algorithm, not quantum-resistant. The keys are also exposed in a public repository.

---

## Claim 2: Non-Exportable Private Keys

**CLAIM:**
> "Private keys stored as non-exportable within device secure elements"
> "private keys cannot be duplicated or cloned"

**LIE:** Private keys are stored in plaintext YAML files on GitHub.

**EVIDENCE:**
```yaml
# heimdall/application.yaml:163-165
identityKeypairs:
  - FULL_64_BYTE_KEY_IN_PLAINTEXT
```

**IMPACT:** Anyone can copy these keys. They are not in secure elements. They are not non-exportable.

---

## Claim 3: Biometric Security

**CLAIM:**
> "Biometric linking prevents unauthorized access even with device access"
> "Multi-factor authentication and biometric verification"

**LIE:** Device identification is disabled in production code.

**EVIDENCE:**
```go
// heimdall/accounts/internal/device-identification/contract.go:17
const deviceIdentificationDisabled = true
```

**IMPACT:** Biometric protection does not work. The feature is turned off.

---

## Claim 4: KYC Verification

**CLAIM:**
> "Assurance levels: none, low, substantial, high"
> "Minimum dataset (name, surname, birthdate) for verification"

**LIE:** KYC verification is bypassed with hardcoded `if true` statements.

**EVIDENCE:**
```go
// eskimo/kyc/quiz/quiz.go:243
func (r *repositoryImpl) CheckQuizStatus(ctx context.Context, userID UserID) (*QuizStatus, error) {
    if true {
        return &QuizStatus{
            KYCQuizCompleted: false,
        }, nil
    }
    // Actual implementation never reached
}
```

**IMPACT:** The KYC system does not verify anyone. 26+ similar bypass flags exist across the codebase.

---

## Claim 5: Multi-Party Computation

**CLAIM:**
> "Multi-Party Computation splits private keys into five shares"
> "Three shares needed for recovery"

**LIE:** No MPC implementation exists. Keys are stored whole.

**EVIDENCE:**
```yaml
identityKeypairs:
  - FULL_64_BYTE_PRIVATE_KEY  # Not split into shares
```

**IMPACT:** Keys are single points of failure. The MPC claim is false.

---

## Claim 6: User Data Ownership

**CLAIM:**
> "Users retain complete ownership of their data"
> "Granular disclosure controls"

**LIE:** Any user can access any other user's data through IDOR vulnerability.

**EVIDENCE:**
```go
// eskimo/cmd/eskimo/referrals.go:45
func (s *service) GetReferralAcquisitionHistory(
    ctx context.Context,
    req *server.Request[GetReferralAcquisitionHistoryArg, []*users.ReferralAcquisition],
) (*server.Response[[]*users.ReferralAcquisition], *server.Response[server.ErrorResponse]) {
    // NO ownership check
    res, err := s.usersRepository.GetReferralAcquisitionHistory(ctx, req.Data.UserID)
    return server.OK(&res), nil
}
```

**IMPACT:** User data is not protected. Privacy is compromised.

---

## Claim 7: Decentralized Validators

**CLAIM:**
> "Validators elected by community"
> "Prevents power concentration"

**LIE:** Over 50% of tokens are controlled by Team, DAO, and Treasury.

**EVIDENCE:**
```
Token Distribution:
- Team Pool: 25%
- DAO Pool: 15%
- Treasury: 10%
Total insider control: 50%
```

**IMPACT:** Validators are not community-elected when insiders control majority of tokens.

---

## Claim 8: Non-Custodial Wallets

**CLAIM:** Users own their keys. Decentralized. Non-custodial.

**LIE:** ICE uses DFNS custody service with admin access to all wallets.

**EVIDENCE:**
```go
// heimdall/accounts/internal/dfns/dfns.go
ServiceAccountPrivateKey   // ICE admin access
ServiceAccountCredentialID // Admin credential
```

**IMPACT:** Users do NOT own their private keys. ICE has administrative access to all wallets through DFNS.

---

## Claim 9: Secure Authentication

**CLAIM:** Enterprise-grade security.

**LIE:** Login bypass exists with hardcoded credentials.

**EVIDENCE:**
```yaml
# eskimo/application.yaml:146-147
confirmationCode:
  constCodes:
    bogus@example.com: 111
```

**IMPACT:** Authentication can be bypassed with hardcoded values.

---

## Claim 10: Data Integrity

**CLAIM:** Hash encryption mechanism validates data.

**LIE:** Checksum validation is bypassed.

**EVIDENCE:**
```go
// eskimo/users/users.go:177
if true || checksum == "" {
    // Validation bypassed
}
```

**IMPACT:** Data integrity checks do not work.

---

## Claim 11: Secure Infrastructure

**CLAIM:** Enterprise security for network infrastructure.

**LIE:** Internal relay IPs are exposed in public code.

**EVIDENCE:**
```go
// heimdall/token-analytics/dummy_inserts.go:1812
ionConnectRelays := []string{
    "wss://141.95.59.70:4443",
    "wss://181.41.142.217:4443",  // Verified open
    "wss://94.100.16.233:4443",
}
```

**IMPACT:** Internal infrastructure is exposed to the public.

---

## Claim 12: Production-Ready Code

**CLAIM:** Professional development practices.

**LIE:** Development credentials are in production code.

**EVIDENCE:**
```yaml
# Multiple application.yaml files
jwtSecret: bogus
communityTokenAPIKey: dev-api-key
identityServiceApiKey: dev-api-key
postgresql://root:pass@localhost:5433/eskimo
```

**IMPACT:** Test and development credentials should never be in production.

---

## Claim 13: Engagement Rewards

**CLAIM:**
> "Deflationary mechanism through fee burning"
> "Users earn through engagement"

**LIE:** Tokenomics and task rewards are bypassed.

**EVIDENCE:**
```go
// freezer/tokenomics/tokenomics.go:222
func (r *repository) isAdvancedTeamEnabled(device string) bool {
    if true {
        return false
    }
}

// freezer/tokenomics/balance.go:282
func (s *completedTasksSource) Process(ctx context.Context, message *messagebroker.Message) (err error) {
    if true {
        return nil  // Rewards bypassed
    }
}
```

**IMPACT:** The reward system can be manipulated.

---

## Summary

| Claim | Reality |
|-------|---------|
| Quantum-resistant encryption | Standard Ed25519, keys public |
| Non-exportable keys | Plaintext YAML files |
| Biometric security | Disabled in code |
| KYC verification | Bypassed with if true |
| MPC key splitting | Keys stored whole |
| User data ownership | IDOR vulnerability |
| Decentralized validators | 50% insider token control |
| Non-custodial wallets | DFNS custody with admin access |
| Secure authentication | Hardcoded bypass |
| Data integrity | Checksum validation bypassed |
| Secure infrastructure | Internal IPs exposed |
| Production-ready | Dev credentials in production |
| Engagement rewards | Reward system bypassed |

---

## References

- ICE Documentation: https://docs.ice.io
- ICE Whitepaper: https://ice.io/whitepaper
- GitHub Repository: https://github.com/ice-blockchain
- Mainnet Explorer: https://explorer.ice.io

---

## Disclaimer

This report documents discrepancies between whitepaper claims and source code implementation. All findings are based on publicly available source code and APIs.

**Report Date:** 2026-04-12
**Website:** https://mefai.io
