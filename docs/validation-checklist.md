# Validation record and remaining work

The table distinguishes administrator-reported functional tests, observed session data, and checks not yet supported by published evidence.

| Scenario | Expected outcome | Evidence/status |
| --- | --- | --- |
| Test endpoint included in MAC exception group | Matches exception rule, social-media access allowed | **Observed:** exception policy ID in session table; administrator confirms social-media access |
| Non-exempt device attempts a restricted social-media site | Blocked on the protected network | **Administrator confirms social-media blocking works for devices outside approved MAC group** |
| Non-exempt device uses a social-media app | Blocked when recognized by Application Control | Not fully documented |
| Non-exempt device attempts UDP/443 | Denied by QUIC policy | Rule configured; live hit evidence pending |
| Non-exempt device visits permitted business website | Allowed | Not fully documented |
| Phone with randomized Wi-Fi address | Match behavior confirmed against actual network MAC | Needs targeted test |
| Phone using mobile data or external VPN | Outside assumed protected path | Scope limitation |
| Earlier user/group policies | No unintended bypass of inspected rule | Needs policy-hit review |
| Other Wi-Fi interface or network segment | Protected by corresponding policy where intended | Needs path verification |

## Verified configuration details

- Category `23` is `Social.Media` according to Fortinet's FortiOS 7.0 documentation; configured action is block, with logging enabled.
- The compiled rule table contains the six-device MAC exception, UDP/443 drop rule, and inspected staff policy.
- These are configuration facts, not proof that every possible bypass has been tested.

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

1. Are older identity-based policies matching any current sessions?
2. Does testing confirm blocking in mobile native apps, not just browsers?
3. Are wireless clients on any other ingress interface outside this policy scope?
4. Are any legitimate business sites impacted by overbroad wildcard matches?
