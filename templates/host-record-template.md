# Host Record Template

Use this template when documenting a host under `docs/infrastructure/hosts/`.

---

## Host Identity

| Item | Value |
|---|---|
| Hostname | |
| FQDN | |
| Site | |
| Domain | |
| Host Type | |
| Status | |

## Operating System

| Item | Value |
|---|---|
| Operating System | |
| Version / Edition | |
| Deployment Model | |
| Hypervisor | |

## Role

Describe the host role and current functions.

Example points:
- domain controller
- DNS server
- Linux member server
- client workstation
- file service host

## Network Configuration

| Item | Value |
|---|---|
| Network | |
| IP Address | |
| Default Gateway | |
| DNS Server | |
| Domain Controller | |

## Template Source

| Item | Value |
|---|---|
| Template Type | |
| Template State | |
| Required Customisation | |

## Deployment Purpose

Describe why this host exists in the environment.

## Validation Checks

List the checks that confirm the host is healthy and usable.

Example points:
- hostname resolves correctly
- network configuration is correct
- service role is operational
- domain join succeeds
- client access succeeds

## Notes

Record host-specific operational notes, dependencies, and follow-up items.