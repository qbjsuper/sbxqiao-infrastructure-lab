# DNS Design

This document describes the DNS design of the SBXQIAO Infrastructure Lab.

---

## Role of DNS

DNS is a core dependency of Active Directory and must function correctly before:

- domain join
- AD authentication
- domain controller promotion
- service discovery
- replication validation

---

## Current DNS Design

DNS is Active Directory integrated.

### Current DNS Server

- `sbx-dc1.sbxqiao.lab`
- IP: `172.16.50.10`

### Planned Additional DNS Server

- `sbx-dc2.sbxqiao.lab`
- IP: `172.16.51.10`

---

## DNS Client Rules

Design principles:

- domain members should use domain DNS servers only
- clients should not use public DNS directly for AD operations
- name resolution must work across sites
- DNS should be validated before domain join and before promotion

---

## SBY Initial Deployment Sequence

When building `sbx-dc2`:

1. configure static IP
2. set preferred DNS to `172.16.50.10`
3. verify name resolution
4. join domain
5. promote to domain controller
6. verify DNS zones and records after promotion

---

## Validation Targets

- resolve `sbxqiao.lab`
- resolve `sbx-dc1.sbxqiao.lab`
- verify SRV record discovery
- verify forward and reverse lookup behaviour