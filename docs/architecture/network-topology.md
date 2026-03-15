# Network Topology

This document describes the logical network topology of the SBXQIAO Infrastructure Lab.

---

## Overview

The lab is structured as a multi-site environment.

Current sites:
- **SBX** — Primary site
- **SBY** — Secondary site

Each site uses its own subnet to support routing, Active Directory site mapping, and replication testing.

Each site also uses its own pfSense firewall/router as the local default gateway.

---

## Site Networks

### SBX Site
- Subnet: `172.16.50.0/24`
- Gateway: `172.16.50.1`
- Site edge firewall/router: `pfSense-SBX`
- Domain Controller: `sbx-dc1.sbxqiao.lab`
- Linux Member Server: `sbx-lx1.sbxqiao.lab`

### SBY Site
- Subnet: `172.16.51.0/24`
- Gateway: `172.16.51.1`
- Site edge firewall/router: `pfSense-SBY`
- Planned Domain Controller: `sby-dc1.sbxqiao.lab`

---

## Logical Topology

```text
                  Home-router Ethernet transport network
        ┌──────────────────────────────────────────────────────┐
        │ Physical link between the two Hyper-V host systems   │
        └───────────────┬───────────────────────┬──────────────┘
                        │                       │
                ┌───────▼────────┐      ┌───────▼───────┐
                │  pfSense-SBX   │      │ pfSense-SBY   │
                │  172.16.50.1   │      │ 172.16.51.1   │
                │  Site gateway  │      │ Site gateway  │
                └───────┬────────┘      └──────┬────────┘
                        │                      │
              ┌─────────▼─────────┐   ┌────────▼─────────┐
              │ SBX Site          │   │ SBY Site         │
              │ 172.16.50.0/24    │   │ 172.16.51.0/24   │
              └─────────┬─────────┘   └────────┬─────────┘
                        │                      │
          ┌─────────────▼────────────┐  ┌──────▼────────────┐
          │ sbx-dc1.sbxqiao.lab      │  │sby-dc1.sbxqiao.lab│
          │ 172.16.50.10             │  │ 172.16.51.10      │
          └─────────────┬────────────┘  └───────────────────┘
                        │
          ┌─────────────▼────────────┐
          │ sbx-lx1.sbxqiao.lab      │
          │ 172.16.50.30             │
          └──────────────────────────┘