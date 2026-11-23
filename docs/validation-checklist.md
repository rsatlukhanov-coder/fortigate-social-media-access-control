# Validation record and remaining work

The table distinguishes observed evidence from outstanding tests.

| Scenario | Expected outcome | Evidence/status |
| --- | --- | --- |
| Test endpoint included in MAC exception group | Matches exception rule, Internet access allowed | **Observed:** session policy ID pointed to exception rule |
| Non-exempt device visits a domain in static block list | Blocked by Web Filter if session reaches inspected policy | Not fully documented |
| Non-exempt device uses a social-media app | Blocked when recognized by Application Control | Not fully documented |
| Non-exempt device attempts UDP/443 | Denied by QUIC policy | Rule configured; live hit evidence pending |
| Non-exempt device visits permitted business website | Allowed | Not fully documented |
| Phone with randomized Wi-Fi address | Match behavior confirmed against actual network MAC | Needs targeted test |
| Phone using mobile data or external VPN | Outside assumed protected path | Scope limitation |
| Earlier user/group policies | No unintended bypass of inspected rule | Needs policy-hit review |
| Other Wi-Fi interface or network segment | Protected by corresponding policy where intended | Needs path verification |

## Suggested evidence to add

For each test, note only:
- Device class: Windows laptop / iOS / Android (no username or MAC)
- Network path: protected Wi-Fi / other
- Target category: social / video / game / ordinary business site
- Result: pass or block
- Policy role hit: exception / QUIC deny / inspected staff
- Inspection log category if safely sanitized

Use redacted or fictional examples rather than pasting live firewall logs.

## Questions not yet answered

1. Does the intended Application Control category correspond to the actual social-media signatures?
2. Are older identity-based policies matching any current sessions?
3. Does testing confirm blocking in mobile native apps, not just browsers?
4. Are wireless clients on any other ingress interface outside this policy scope?
5. Are any legitimate business sites impacted by overbroad wildcard matches?
