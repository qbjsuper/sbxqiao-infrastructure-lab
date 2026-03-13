# sbx-cl2

## Host Identity

| Item | Value |
|---|---|
| Hostname | `sbx-cl2` |
| Site | `SBX` |
| Domain | `SBXQIAO.LAB` |
| Host Type | Windows client workstation |
| Build Status | Planned |

## Operating System

| Item | Value |
|---|---|
| Operating System | Windows 11 |
| Edition | To be confirmed |
| Deployment Model | Virtual machine |
| Hypervisor | Hyper-V |

## Role

`sbx-cl2` is the second Windows client workstation for the SBX site.

This host is intended to provide a clean domain-joined workstation for:
- second-client Samba validation
- cross-user file access testing
- Group Policy testing
- client-side authentication testing
- client DNS and name-resolution testing
- later workstation administration practice

## Network Configuration

| Item | Value |
|---|---|
| Network | `172.16.50.0/24` |
| IP Assignment | DHCP or reserved address |
| Default Gateway | `172.16.50.1` |
| DNS Server | `172.16.50.10` |
| Domain Controller | `sbx-dc1.sbxqiao.lab` |

## Template Source

| Item | Value |
|---|---|
| Template Type | Windows client template |
| Template State | Clean baseline before domain join |
| Required Customisation | Rename to `sbx-cl2`, configure network, join `SBXQIAO.LAB` |

## Deployment Purpose

The immediate purpose of `sbx-cl2` is to validate Samba from a second SBX workstation using a separate client context.

Primary objectives:
- verify access to `\\sbx-lx1\Shared` from a second workstation
- test a separate domain user logon session
- confirm read access to existing shared content
- confirm file and folder creation from a second client
- confirm whether another authorised user can modify existing shared content

Secondary future objectives:
- Group Policy validation
- mapped drive testing
- client troubleshooting practice
- standard user workflow simulation
- later site and service validation tasks

## Validation Checks

Planned validation for `sbx-cl2`:
- hostname is `sbx-cl2`
- client is on the `172.16.50.0/24` network
- DNS points to `172.16.50.10`
- domain join to `SBXQIAO.LAB` succeeds
- domain logon succeeds
- domain controller discovery succeeds
- `sbx-dc1` name resolution succeeds
- `sbx-lx1` name resolution succeeds
- `\\sbx-lx1\Shared` opens successfully
- existing shared content is visible
- second-client file creation succeeds
- cross-user modification test is recorded

## Notes

`sbx-cl2` should be used as a normal workstation validation point and not as an infrastructure server.

This host is preferred over using `sbx-dc1` for Samba verification because it provides a cleaner client context and better represents ordinary workstation behaviour.