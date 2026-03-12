# SBY Site Record

This document defines the planned implementation state of the SBY site.

---

## Site Identity

- Site Name: `SBY`
- Subnet: `172.16.51.0/24`
- Gateway: `172.16.51.1`

---

## Planned Host

### sbx-dc2

- FQDN: `sbx-dc2.sbxqiao.lab`
- IP: `172.16.51.10`
- Planned Roles:
  - Domain Controller
  - DNS Server
  - Global Catalog

---

## Deployment Intent

SBY is the secondary site and is intended to support:

- multi-site Active Directory
- additional DNS service
- replication validation
- site-aware authentication testing
- branch-style infrastructure design practice