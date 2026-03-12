# Active Directory Design

This document describes the Active Directory design of the SBXQIAO Infrastructure Lab.

---

## Domain Model

The environment uses a single forest and single domain design.

- Forest: `sbxqiao.lab`
- Domain: `sbxqiao.lab`

This keeps the directory model simple while still supporting enterprise-style operational practices through site separation and role distribution.

---

## Domain Controllers

### SBX Site

- `sbx-dc1.sbxqiao.lab`
- Roles:
  - Domain Controller
  - DNS Server
  - Global Catalog

### SBY Site

- `sbx-dc2.sbxqiao.lab` *(planned)*
- Roles:
  - Domain Controller
  - DNS Server
  - Global Catalog

---

## Site Design

### SBX

- Subnet: `172.16.50.0/24`

### SBY

- Subnet: `172.16.51.0/24`

This allows Active Directory to support:

- site-aware authentication
- realistic replication behaviour
- branch-style deployment practice
- site and subnet mapping

---

## Replication Intent

The multi-site design should support:

- directory replication between DCs
- DNS-integrated zone replication
- SYSVOL replication
- validation through `repadmin`, `dcdiag`, and DNS tests

---

## Design Objectives

This AD design supports the following learning goals:

- deploy additional domain controllers
- manage multi-site directory design
- validate replication health
- understand DNS dependency in AD
- practise staged infrastructure rollout