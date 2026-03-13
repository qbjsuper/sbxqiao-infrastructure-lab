# SBX Build Record

This document records the deployment sequence and current build state of the SBX site.

---

## Purpose

The SBX site is the primary implementation site for the current lab environment.

This document exists to:
- record the order of deployment for SBX infrastructure
- show what has already been built
- identify the next planned build step
- provide a site-level deployment reference

---

## Site Scope

- Site Name: `SBX`
- Subnet: `172.16.50.0/24`
- Gateway: `172.16.50.1`
- Domain: `SBXQIAO.LAB`

---

## Build Phases

### Phase 1 — Core Directory Services
Completed:
- deployed `sbx-dc1`
- configured Active Directory Domain Services
- configured DNS
- established the SBX domain foundation

### Phase 2 — Linux Member Services
Completed:
- deployed `sbx-lx1`
- joined the server to `SBXQIAO.LAB`
- stabilised winbind-based AD integration
- enabled SSH
- configured Samba file sharing
- validated authenticated access to the `Shared` share

### Phase 3 — Second Client Validation
Planned:
- deploy `sbx-cl2`
- join `sbx-cl2` to `SBXQIAO.LAB`
- validate second-client access to `\\sbx-lx1\Shared`
- verify cross-user Samba access behaviour

---

## Current Build State

### Implemented
- `sbx-dc1`
- `sbx-lx1`

### Planned
- `sbx-cl2`

---

## Next Build Target

### sbx-cl2

`sbx-cl2` is planned as the second Windows client workstation in the SBX site.

Its immediate purpose is to:
- provide a clean second client for Samba validation
- verify access from a separate workstation context
- test cross-user file access behaviour

Detailed host information should be recorded in:
- `docs/infrastructure/hosts/sbx-cl2.md`

Validation results should be recorded in:
- `docs/operations/sbx-cl2-samba-validation.md`

---

## Notes

SBX remains the active primary site.

The next logical build step is `sbx-cl2`, which will complete the second-client Samba validation stage before the lab focus moves further into documentation refinement and second-site planning.