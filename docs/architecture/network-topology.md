# Network Topology

This document describes the logical network topology of the SBXQIAO Infrastructure Lab.

---

## Overview

The lab is structured as a multi-site environment.

Current sites:

- **SBX** — Primary site
- **SBY** — Secondary site

Each site uses its own subnet to support routing, Active Directory site mapping, and future replication testing.

---

## Site Networks

### SBX Site

- Subnet: `172.16.50.0/24`
- Gateway: `172.16.50.1`
- Domain Controller: `sbx-dc1.sbxqiao.lab`
- Linux Member Server: `sbx-lx1.sbxqiao.lab`

### SBY Site

- Subnet: `172.16.51.0/24`
- Gateway: `172.16.51.1`
- Planned Domain Controller: `sbx-dc2.sbxqiao.lab`

---

## Logical Topology

```text
                 ┌─────────────────────┐
                 │      pfSense        │
                 │ Routing / Gateway   │
                 └─────────┬───────────┘
                           │
         ┌─────────────────┴─────────────────┐
         │                                   │
 ┌───────▼────────┐                 ┌────────▼───────┐
 │   SBX Site     │                 │   SBY Site     │
 │ 172.16.50.0/24 │                 │ 172.16.51.0/24 │
 └───────┬────────┘                 └────────┬───────┘
         │                                   │
 ┌───────▼──────┐                     ┌──────▼───────┐
 │   sbx-dc1    │                     │   sbx-dc2    │
 │ 172.16.50.10 │                     │ 172.16.51.10 │
 └──────────────┘                     └──────────────┘
         │
 ┌───────▼──────┐
 │   sbx-lx1    │
 │ 172.16.50.30 │
 └──────────────┘
```

## Design Goals

The topology is intended to support:

- multi-site Active Directory
- DNS resolution across sites
- site-aware authentication
- replication testing
- future WAN simulation
- future VPN experiments if required

## Notes

- each site must use its own subnet
- DNS must remain reliable before domain join and domain controller promotion
- subnet-to-site mapping will later be configured in Active Directory Sites and Services

## Physical Host Topology

The current homelab physical hosting arrangement is:

- SBX site workloads run on the big PC
- SBY site workloads run on the mini PC

This mapping describes where the site workloads are hosted physically.
It is separate from the logical Active Directory site and subnet design.