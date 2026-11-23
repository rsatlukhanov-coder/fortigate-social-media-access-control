# Technical notes — policy design and verification

These notes describe the implementation decisions in a **sanitized** form, with example object names and no production endpoints.

## Initial assessment

The firewall had existing identity-based access policies, but the required filtering controls were not present on those rules. FortiGuard web categorization was unavailable; therefore category-only configuration was unsuitable.

The device's application-signature database was reported as more current than the legacy IPS/AV data, so Application Control was used as a complementary control rather than treating outdated AV/IPS signatures as part of this project's assurance.

## Controls and responsibilities

### Allowlisted devices

Each approved endpoint was represented by a MAC-type firewall address object and grouped into a single exception address group. The exception policy was ordered above the QUIC deny and inspected staff policy.

The operator verified that a new test session selected the exception policy by inspecting session policy IDs.

MAC allowlisting remains spoofable and is affected by Wi-Fi address randomization. The relevant identifier is the source MAC actually presented on the protected path, not necessarily the hardware address printed on a device.

### Static URL filtering

A newly allocated URL filter table contained 49 block entries grouped into social, streaming, adult content, gaming and entertainment. The table was referenced by a new Web Filter profile, attached only to the restricted staff policy.

The original URL-filter tables and older profiles were intentionally left unchanged.

Some entries used wildcard matches. These require careful validation: a wildcard may unintentionally match more than the intended domain. Encrypted request paths are not generally visible with certificate-only inspection.

### Application Control

The restricted policy also used an Application Control profile intended to block the social-media category and log matched applications. Other and unknown application actions were set to pass.

**Category confirmed against Fortinet's FortiOS 7.0 documentation:** `category 23` corresponds to **Social.Media**. The actual profile has `action block`, `log enable`, and pass for other/unknown applications. This does not prove identification of every social media app.

### QUIC

A dedicated service for UDP destination port 443 was used with a DENY policy placed below the exception. The goal was to reduce QUIC/HTTP3 bypass of the intended inspection path, with fallback to TCP/443 where supported.

This is a transport-level deny for non-exempt devices, not a social-media category decision.

## Example read-only verification commands

Run on an **authorized FortiGate CLI**. Review output before sharing:

```text
get system status
show firewall policy
show firewall policy <POLICY_ID>
show firewall addrgrp
show webfilter profile
show webfilter urlfilter
show application list
diagnose sys session filter clear
diagnose sys session filter src <TEST_IP>
diagnose sys session list
diagnose sys session filter clear
```

The filter commands above change only the diagnostic display filter, not firewall policy state. Replace placeholders with a test endpoint from your authorized environment.

**Do not publish the raw output**: it may expose operational IPs, MAC addresses, user identities or encrypted secret values.

## Change sequencing

The profile and table were created before being attached to the live staff rule. The exception rule was created and tested first. After the filtering profile was enabled, only the test endpoint's existing sessions were cleared for a fresh policy evaluation.

Changes were limited to the intended staff Internet traffic path; unrelated server, VPN and other access policies were not intentionally edited.

## Rollback strategy

- Remove inspection profile references from the staff filtering policy if required.
- Disable or remove the new QUIC deny policy after checking dependencies.
- Keep a pre-change firewall backup in an approved secure location.
- Do not use broad session clears or policy deletions without confirming impact.

Because actual production policy identifiers are intentionally omitted, this public document does not provide a copy/paste production rollback script.

## Compiled policy validation

Read-only `diagnose firewall iprope list 100004` output confirmed:

- MAC exception rule compiled as ACCEPT with six source MAC entries.
- QUIC rule compiled as DROP for UDP destination port 443.
- Staff Internet rule compiled and present, with the relevant security profiles attached in its firewall configuration.
- Older identity-based ACCEPT policies are still installed and may warrant a hit-count review.

This output does not establish live policy-hit behavior for each client.
