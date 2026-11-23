# Responsible documentation and publication

This is a sanitized portfolio project, not a production configuration repository.

**Do not publish any of the following:**
- Organization or employee names, identifying labels and user accounts
- Internal or public IP addresses, subnets, network topology, access-point counts or device locations
- MAC addresses, hostnames, hardware serial numbers, policy UUIDs and raw device inventories
- WAN management settings, VPN architecture, authentication methods, secrets, encrypted password blobs or configuration backups
- Raw DHCP leases, session records, traffic logs or unredacted GUI/CLI screenshots
- Configuration files that could be imported into the organization's live firewall

Information derived from a workplace deployment may require company approval even after sanitization.

If evidence is later added, use a staged review:
1. Prepare screenshots or logs offline.
2. Redact sensitive details and metadata.
3. Confirm the remaining material does not reveal the internal environment.
4. Publish only with appropriate authorization.

MAC address allowlisting is not strong authentication; device identity can be spoofed.
