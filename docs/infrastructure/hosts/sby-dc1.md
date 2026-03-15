# sby-dc1

## Identity
- Hostname: `sby-dc1`
- Hyper-V VM name: `SBY-DC1`
- FQDN: `sby-dc1.sbxqiao.lab`
- Site: `SBY`

## Intended Role
- Additional Domain Controller
- DNS Server
- Secondary site infrastructure host for `sbxqiao.lab`

## Planned Network Configuration
- Subnet: `172.16.51.0/24`
- Planned IP: `172.16.51.10`
- Gateway: `172.16.51.1`
- Preferred DNS before promotion: `172.16.50.10`
- DNS suffix: `sbxqiao.lab`

## Directory Services Target
- Forest: `sbxqiao.lab`
- Domain: `sbxqiao.lab`
- Promotion type: additional domain controller in existing domain

## Build Intent
`sby-dc1` will be deployed as the first SBY host and promoted into the existing domain to establish the secondary AD site.

## Validation Goals
- Host uses the planned static network configuration
- Name resolution to `sbx-dc1.sbxqiao.lab` works
- Domain join path is healthy
- Promotion completes successfully
- AD replication succeeds between `sbx-dc1` and `sby-dc1`
- DNS zone data is present and healthy
- SBY subnet is associated with site `SBY`
- `sby-dc1` is mapped to the correct site

## Pre-Promotion Checkpoint
Create a VM checkpoint only after:
- OS install is complete
- hostname is set
- static IP is configured
- updates are applied
- connectivity to `sbx-dc1` is verified
- local admin access is confirmed

## Notes
This host is part of the single-forest, single-domain, multi-site design for the lab.