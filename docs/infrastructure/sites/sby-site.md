# SBY Site

## Site Purpose

SBY is the secondary Active Directory site in the `sbxqiao.lab` lab design.

## Site Network
- Site name: `SBY`
- Subnet: `172.16.51.0/24`
- Gateway: `172.16.51.1`
- Site edge firewall/router: `pfSense-SBY`
- WAN transit IP: `192.168.0.253/24`

## Planned Infrastructure
- `sby-dc1` — Additional Domain Controller / DNS

## Physical Host Placement
- Site compute host: mini PC

## Site Role in Lab

SBY extends the lab from a single active site into a documented multi-site AD design.

It is intended to host directory and name resolution services for the secondary subnet.

SBY uses its own local pfSense instance as the site default gateway.
Cross-site traffic from SBY is intended to pass through `pfSense-SBY` over the IPsec tunnel toward SBX.

## Design Expectations
- The SBY subnet must be mapped to the `SBY` AD site
- `sby-dc1` must be associated with the `SBY` site after promotion
- Replication between SBX and SBY must validate cleanly
- DNS must remain healthy across both domain controllers
- Inter-site IPsec connectivity to SBX must be working before domain join and promotion

## Current State

SBY is in build-preparation state.
The first planned SBY infrastructure host is `sby-dc1`.