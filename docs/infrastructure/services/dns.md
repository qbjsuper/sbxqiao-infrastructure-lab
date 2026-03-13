# DNS Service Record

## Service Identity

| Item | Value |
|---|---|
| Service Name | Domain Name System |
| Domain Namespace | `sbxqiao.lab` |
| Primary DNS Host | `sbx-dc1.sbxqiao.lab` |
| Primary Site | `SBX` |
| Status | Implemented |

## Purpose

DNS provides the name-resolution foundation for the lab.

Its purpose is to:
- resolve hosts in `sbxqiao.lab`
- support Active Directory service location
- support client and server domain operations
- support Linux member-server integration
- support future multi-site expansion

## Current Implementation

The current DNS implementation is hosted on `sbx-dc1`.

Current operational use includes:
- host record resolution for SBX systems
- AD-integrated service discovery
- SRV record resolution for domain operations
- name resolution required by Samba and Linux AD integration

## Current Clients

The following systems should use the domain controller for DNS:
- `sbx-dc1`
- `sbx-lx1`
- `sbx-cl1`
- `sbx-cl2` when deployed
- other domain-joined systems in the SBX site

## Validation State

The DNS service should be considered healthy when:
- `sbx-dc1.sbxqiao.lab` resolves correctly
- `sbx-lx1.sbxqiao.lab` resolves correctly
- `_ldap._tcp` and other required SRV records resolve correctly
- clients can locate the domain controller
- Linux member systems can resolve required AD records
- domain join and domain logon succeed

## Operational Notes

DNS should point to the domain controller for domain-joined systems.

Incorrect DNS configuration is one of the highest-risk causes of AD and Samba integration failure in this lab.

Future DNS expansion for additional sites should be recorded after secondary-site planning is confirmed.

## Notes

This document records the implemented DNS service only.

Future DNS architecture, delegation, or multi-site service design should be documented separately in architecture records.