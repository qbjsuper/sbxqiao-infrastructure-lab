# sbx-cl2

## Host Identity

| Item | Value |
|---|---|
| Hostname | `sbx-cl2` |
| FQDN | `sbx-cl2.sbxqiao.lab` |
| Site | `SBX` |
| Domain | `SBXQIAO.LAB` |
| Host Type | Windows client workstation |
| Status | Implemented |

## Operating System

| Item | Value |
|---|---|
| Operating System | Windows 11 |
| Edition | To be confirmed |
| Deployment Model | Virtual machine |
| Hypervisor | Hyper-V |

## Role

`sbx-cl2` is the second Windows client workstation for the SBX site.

Current intended functions:
- second-client Samba validation
- cross-user file access testing
- Group Policy validation
- client-side authentication testing
- workstation administration practice

## Network Configuration

| Item | Value |
|---|---|
| Network | `172.16.50.0/24` |
| IP Address | `TBD` |
| Default Gateway | `172.16.50.1` |
| DNS Server | `172.16.50.10` |
| Domain Controller | `sbx-dc1.sbxqiao.lab` |

## Template Source

| Item | Value |
|---|---|
| Template Type | Windows client template or clean installation |
| Template State | Baseline system before domain join |
| Required Customisation | hostname assignment, network configuration, domain join, validation testing |

## Deployment Purpose

`sbx-cl2` exists to provide a clean second workstation in the SBX site.

Its deployment purpose is to:
- validate access to `\\sbx-lx1\Shared` from a second client
- test a separate domain-user context
- verify cross-user share behaviour
- support later workstation and policy testing

## Validation Checks

The following checks should be valid for `sbx-cl2`:
- hostname resolves correctly
- IP configuration is correct
- DNS points to `sbx-dc1`
- domain join succeeds
- domain logon succeeds
- domain controller discovery succeeds
- `sbx-lx1` resolves correctly
- `\\sbx-lx1\Shared` opens successfully
- second-client file creation succeeds
- cross-user modification behaviour is recorded

## Notes

`sbx-cl2` should be used as a normal workstation validation point and not as an infrastructure server.

This host is preferred over using `sbx-dc1` for Samba verification because it provides a cleaner client context and better represents ordinary workstation behaviour.