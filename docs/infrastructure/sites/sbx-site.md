# SBX Site Record

This document records the current implementation state of the SBX site.

---

## Site Identity

- Site Name: `SBX`
- Subnet: `172.16.50.0/24`
- Gateway: `172.16.50.1`
- Site edge firewall/router: `pfSense-SBX`

---

## Physical Host Placement

- Site compute host: big PC

## Notes


SBX workloads currently run on the big PC in the homelab environment.
This physical host placement is an implementation detail and does not change the logical site role of SBX within the `sbxqiao.lab` design.

SBX uses its local pfSense instance as the site default gateway.
Cross-site traffic from SBX is intended to pass through `pfSense-SBX` over the IPsec tunnel toward SBY.

### sbx-dc1
- FQDN: `sbx-dc1.sbxqiao.lab`
- IP: `172.16.50.10`
- Roles:
  - Domain Controller
  - DNS Server
  - Global Catalog

### sbx-lx1
- FQDN: `sbx-lx1.sbxqiao.lab`
- IP: `172.16.50.30`
- Roles:
  - Linux member server
  - Samba file service

### sbx-cl2
- FQDN: `sbx-cl2.sbxqiao.lab`
- IP: `TBD`
- Roles:
  - Windows client workstation
  - Second-client validation endpoint
  - Samba access verification client

---

## Current Status

SBX is the active primary site and currently hosts the main domain controller, the Linux member infrastructure for Samba and authentication testing, and the Windows client validation layer.

`sbx-cl2` has been deployed, joined to the domain, and used to complete second-client Samba validation, including cross-user file modification testing.