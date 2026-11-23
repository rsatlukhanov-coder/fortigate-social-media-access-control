# Policy design

## Approved device objects
Create individual MAC address objects for authorized endpoints and group them into an address group, for example `APPROVED_DEVICE_MACS`. Wired and wireless adapters have distinct MAC addresses; phone private Wi-Fi addresses can differ from hardware MACs.

## Exception policy
Incoming: staff LAN. Outgoing: WAN. Source: approved MAC group. Destination: all. Service: ALL. Action: ACCEPT. NAT: enabled if the path needs it. No social-media Web Filter or Application Control profiles are attached. Place above other staff filtering rules.

## QUIC deny policy
Incoming: staff LAN. Outgoing: WAN. Service: UDP/443. Action: DENY. Place below the approved MAC exception and before the filtered staff Internet policy. Other clients may fall back to TCP/443; this deny alone is not social-media filtering.

## Staff Internet policy
Source: staff subnet. Action: ACCEPT, with social-media Web Filter and Application Control profiles attached, plus appropriate SSL inspection and logging. Actual enforcement depends on profile categories, signatures and traffic visibility.

## Critical policy-order check
Earlier identity-based ACCEPT policies without filtering can bypass this staff rule. Review them carefully, preserve required functions and have an authorized rollback plan.

## Wireless traffic
A separate Wi-Fi ingress interface will not match a LAN-only policy. Confirm the path and MAC visibility before adding corresponding wireless policies.

## Evidence
Use anonymized policy-hit IDs and endpoint tests, not production config exports or unredacted packet logs.
