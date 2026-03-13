# sbx-lx1

## Host Identity

| Item | Value |
|---|---|
| Hostname | `sbx-lx1` |
| FQDN | `sbx-lx1.sbxqiao.lab` |
| Site | `SBX` |
| Domain | `SBXQIAO.LAB` |
| Host Type | Ubuntu member server |
| Status | Implemented |

## Operating System

| Item | Value |
|---|---|
| Operating System | Ubuntu Server |
| Version | To be confirmed |
| Deployment Model | Virtual machine |
| Hypervisor | Hyper-V |

## Role

`sbx-lx1` is the Linux member server for the SBX site.

Current functions:
- domain-joined Linux member server
- Samba file service host
- SSH administration endpoint
- winbind-based Active Directory identity integration platform

This host is used to practise Linux administration inside a Windows Active Directory environment.

## Network Configuration

| Item | Value |
|---|---|
| Network | `172.16.50.0/24` |
| IP Address | `172.16.50.30` |
| Default Gateway | `172.16.50.1` |
| DNS Server | `172.16.50.10` |
| Domain Controller | `sbx-dc1.sbxqiao.lab` |

## Template Source

| Item | Value |
|---|---|
| Template Type | Ubuntu Server template or clean installation |
| Template State | Baseline system before domain join |
| Required Customisation | hostname assignment, network configuration, AD join, SSH enablement, Samba configuration |

## Deployment Purpose

`sbx-lx1` exists to provide Linux member-server capability within the SBX site.

Its deployment purpose is to:
- validate Linux integration with Active Directory
- provide a Samba file-sharing platform
- support domain-authenticated file access testing
- provide a Linux administration practice target
- support multi-platform infrastructure learning

## Validation Checks

The following checks should be valid for `sbx-lx1`:
- hostname resolves correctly
- IP configuration is correct
- DNS points to `sbx-dc1`
- domain join is healthy
- winbind identity resolution succeeds
- SSH access succeeds
- Samba service is running
- `Shared` share is accessible to authorised domain users
- read and write operations succeed through Samba
- service behaviour remains stable after reboot

## Notes

`sbx-lx1` is currently operating as an Ubuntu AD member server.

The current AD integration path for Samba access uses winbind.

The primary shared directory currently used for validation is:
- Share name: `Shared`
- Path: `/srv/samba/shared`

This host should be documented together with the Samba service record because both are closely linked operationally.