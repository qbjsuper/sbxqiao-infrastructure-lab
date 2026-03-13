# sbx-dc1

## Host Identity

| Item | Value |
|---|---|
| Hostname | `sbx-dc1` |
| FQDN | `sbx-dc1.sbxqiao.lab` |
| Site | `SBX` |
| Domain | `SBXQIAO.LAB` |
| Host Type | Windows Server domain controller |
| Status | Implemented |

## Operating System

| Item | Value |
|---|---|
| Operating System | Windows Server |
| Edition | To be confirmed |
| Deployment Model | Virtual machine |
| Hypervisor | Hyper-V |

## Role

`sbx-dc1` is the primary domain controller for the SBX site.

Current service roles:
- Active Directory Domain Services
- DNS Server
- Global Catalog
- domain authentication source for SBX systems

This host is the core identity and name-resolution platform for the current lab.

## Network Configuration

| Item | Value |
|---|---|
| Network | `172.16.50.0/24` |
| IP Address | `172.16.50.10` |
| Default Gateway | `172.16.50.1` |
| Preferred DNS | `127.0.0.1` or local DNS service |
| Site Gateway Device | pfSense |

## Template Source

| Item | Value |
|---|---|
| Template Type | Windows Server template or clean installation |
| Template State | Baseline system before promotion |
| Required Customisation | static IP, hostname assignment, AD DS installation, DNS installation, domain controller promotion |

## Deployment Purpose

`sbx-dc1` exists to provide the first working domain controller for the SBX site.

Its deployment purpose is to:
- establish the `SBXQIAO.LAB` domain
- provide authentication and directory services
- provide DNS services for domain-joined systems
- support future site and replication expansion
- act as the primary infrastructure dependency for the current lab stage

## Validation Checks

The following checks should be valid for `sbx-dc1`:
- hostname resolves correctly
- static IP configuration is correct
- AD DS is installed and operational
- DNS is installed and operational
- domain logon succeeds from clients
- SRV records resolve correctly
- domain controller discovery succeeds
- replication status is normal for the current single-DC stage
- time service behaviour is acceptable for the current lab stage

## Notes

`sbx-dc1` is currently the primary infrastructure host for the SBX site.

All SBX domain-joined systems should use the domain controller for DNS.

This host should not be used as a normal client-validation platform unless no other option exists.