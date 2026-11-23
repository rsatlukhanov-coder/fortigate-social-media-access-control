# Test and validation checklist

Tests below are **not** marked successful without evidence.

| Test | Expected | Status |
|---|---|---|
| Approved laptop accesses social media | Allowed by MAC exception rule | Reported working; policy-hit log pending |
| Other laptop attempts UDP/443 | Denied by QUIC rule | Not verified |
| Other laptop uses social-media website over TCP/443 | Blocked by filter profile if correctly classified and policy reached | Not verified |
| Other laptop accesses permitted business website | Allowed | Not verified |
| Other phone opens social-media app over protected path | Blocked if app detected and path covered | Not verified |
| Approved phone on wireless | Allowed if MAC recognized and correct Wi-Fi policy exists | Not verified |
| Earlier identity policy bypass | No bypass of filtered policy | Needs review |
| Separate wireless ingress path | Correct security policies hit | Needs review |

### Verification method
1. Test approved and non-approved devices, both desktop and phone.
2. Verify policy IDs using appropriate traffic logs.
3. Test browser and native applications, TCP/443 and UDP/443 separately.
4. Check LAN and Wi-Fi paths independently.
5. Record pass/fail and anonymized results; do not expose personal or production identifiers.
