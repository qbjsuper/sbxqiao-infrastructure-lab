# SBX Site Record

This document records the current implementation state of the SBX site.

---

## Site Identity

- Site Name: `SBX`
- Subnet: `172.16.50.0/24`
- Gateway: `172.16.50.1`

---

## Implemented Hosts

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

---

## Current Status

SBX is the active primary site and currently hosts the main domain controller and the Linux member infrastructure for Samba and authentication testing.