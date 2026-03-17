# Network Topology

This document describes the logical network topology of the SBXQIAO Infrastructure Lab.

---

## Overview

The lab is built as a multi-site environment within a single forest and single domain design.

- **Forest:** `sbxqiao.lab`
- **Domain:** `sbxqiao.lab`

Current sites:
- **SBX** — Primary site
- **SBY** — Secondary site

Each site uses its own local subnet, local pfSense firewall/router, and local default gateway.  
Inter-site communication is intended to pass through a site-to-site IPsec VPN carried across the home-router underlay network.

---

## Design Principles

The topology is designed to support:

- multi-site Active Directory
- DNS resolution across sites
- routed separation between site subnets
- site-aware authentication and service placement
- replication testing between domain controllers
- controlled inter-site communication through VPN
- enterprise-style staged infrastructure growth

---

## Site Network Summary

### SBX Site
- **Subnet:** `172.16.50.0/24`
- **Default gateway:** `172.16.50.1`
- **Site edge firewall/router:** `pfSense-SBX`
- **WAN transit IP:** `192.168.0.252/24`

Current hosts:
- `sbx-dc1.sbxqiao.lab` — `172.16.50.10`
- `sbx-lx1.sbxqiao.lab` — `172.16.50.30`

### SBY Site
- **Subnet:** `172.16.51.0/24`
- **Default gateway:** `172.16.51.1`
- **Site edge firewall/router:** `pfSense-SBY`
- **WAN transit IP:** `192.168.0.253/24`

Planned host:
- `sby-dc1.sbxqiao.lab` — `172.16.51.10`

---

## Underlay Transport Network

The two Hyper-V host systems are physically connected through a home-router Ethernet network.

- **Underlay network:** `192.168.0.0/24`

This network is used as the transport path between:
- `pfSense-SBX` WAN `192.168.0.252`
- `pfSense-SBY` WAN `192.168.0.253`

The underlay network is not the internal LAN for either site.  
It only provides transport between the two site edge firewalls.

---

## Inter-Site Connectivity Model

Inter-site communication is intended to use a **site-to-site IPsec VPN** between the two pfSense firewalls.

### VPN Endpoints
- **SBX peer:** `192.168.0.252`
- **SBY peer:** `192.168.0.253`

### Protected Internal Networks
- **SBX protected subnet:** `172.16.50.0/24`
- **SBY protected subnet:** `172.16.51.0/24`

This VPN will carry traffic between the two site LANs while preserving routed separation between the subnets.

---

## Logical Topology

```text
                         Home-router underlay network
                               192.168.0.0/24
         ┌────────────────────────────────────────────────────────────┐
         │ Physical Ethernet transport between the two host systems   │
         └───────────────────────┬───────────────────────┬────────────┘
                                 │                       │
                     ┌───────────▼───────────┐   ┌───────▼───────────┐
                     │     pfSense-SBX       │   │    pfSense-SBY    │
                     │ WAN: 192.168.0.252    │===│ WAN: 192.168.0.253│
                     │ LAN: 172.16.50.1      │IPsec│ LAN: 172.16.51.1 │
                     └───────────┬───────────┘   └───────┬───────────┘
                                 │                       │
                    ┌────────────▼────────────┐  ┌───────▼────────────┐
                    │        SBX Site         │  │      SBY Site      │
                    │      172.16.50.0/24     │  │    172.16.51.0/24  │
                    └────────────┬────────────┘  └───────┬────────────┘
                                 │                       │
                 ┌───────────────▼──────────────┐   ┌────▼─────────────┐
                 │  sbx-dc1.sbxqiao.lab         │   │ sby-dc1.sbxqiao.lab
                 │  172.16.50.10                │   │ 172.16.51.10
                 └───────────────┬──────────────┘   └──────────────────┘
                                 │
                 ┌───────────────▼──────────────┐
                 │  sbx-lx1.sbxqiao.lab         │
                 │  172.16.50.30                │
                 └──────────────────────────────┘