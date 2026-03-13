# Active Directory Service Record

## Service Identity

| Item | Value |
|---|---|
| Service Name | Active Directory Domain Services |
| Domain | `SBXQIAO.LAB` |
| Current Scope | Single forest, single domain |
| Primary Site | `SBX` |
| Current Primary Host | `sbx-dc1` |

## Purpose

Active Directory provides the identity, authentication, and directory foundation for the lab.

Its current purpose is to:
- host the `SBXQIAO.LAB` domain
- authenticate users and computers
- provide directory-based administration
- support domain join for servers and clients
- support future multi-site expansion

## Current Implementation

The current implementation state is:
- one forest
- one domain
- one active domain controller in SBX
- SBY identified as a future secondary site
- additional domain-controller work for SBY still pending confirmed build planning

## Service Components

Current known service components:
- domain controller: `sbx-dc1`
- Global Catalog: `sbx-dc1`
- domain authentication source: `sbx-dc1`
- directory database host: `sbx-dc1`

## Dependent Systems

The following current systems depend on Active Directory:
- `sbx-dc1`
- `sbx-lx1`
- current and planned SBX client workstations
- Samba authentication workflows on `sbx-lx1`

## Validation State

The Active Directory service should be considered healthy when:
- domain controller discovery succeeds
- domain logon succeeds
- domain join succeeds for authorised systems
- directory lookups succeed
- Kerberos and NTLM authentication function as expected
- dependent systems can resolve and reach the domain controller

## Operational Notes

At the current stage, Active Directory is centred on the SBX site.

Because the environment is intended to grow into a multi-site lab, future documentation should expand this record when:
- additional domain controllers are deployed
- SBY design is confirmed
- site and subnet records are implemented
- replication planning becomes active

## Notes

This document records the implemented Active Directory service, not the full future design.

High-level future design decisions should be recorded separately in architecture documentation.