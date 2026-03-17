# SBY Site

## Purpose

SBY is the secondary site in the `sbxqiao.lab` multi-site lab design.

It represents a remote site connected to SBX by site-to-site IPsec.

## Network

- Site name: `SBY`
- Subnet: `172.16.51.0/24`
- Default gateway: `172.16.51.1`
- pfSense LAN IP: `172.16.51.1`
- pfSense WAN IP: `192.168.0.253`

## Current systems

### sby-dc1

- Hostname: `sby-dc1`
- IP address: `172.16.51.10`
- Role: domain controller / DNS / Global Catalog
- Domain: `sbxqiao.lab`
- AD site: `SBY`

## Current service state

SBY now has:

- site-to-site connectivity to SBX
- internet access through pfSense-SBY
- an additional writable domain controller
- local DNS service on `sby-dc1`
- a Global Catalog in-site

## Build progress

### Completed

- pfSense-SBY deployed
- SBY subnet established
- IPsec tunnel to SBX established
- inter-site DNS resolution validated
- `sby-dc1` joined to `sbxqiao.lab`
- AD Sites and Services prepared with site `SBY`
- `sby-dc1` promoted as additional DC
- DNS installed on `sby-dc1`
- GC enabled on `sby-dc1`
- replication validated between `SBX-DC1` and `SBY-DC1`

### Issues encountered and resolved

#### 1. AD DNS discovery initially failed across IPsec

- basic ping worked
- simple DNS lookup worked
- AD SRV lookup failed initially

Cause:
- filtering/ruleset scope problem on the inter-site path

Fix:
- broadened firewall rules during bring-up

Result:
- AD SRV lookup succeeded
- domain join and later promotion could proceed

#### 2. No internet access from SBY

Cause:
- `pfSense-SBY` WAN lacked a valid upstream gateway

Fix:
- assigned upstream WAN gateway on pfSense-SBY

Result:
- internet access restored for SBY hosts

## Current AD site design

- `SBX` = `172.16.50.0/24`
- `SBY` = `172.16.51.0/24`

`SBY-DC1` now correctly identifies as site `SBY`.

## Current operational importance

SBY is no longer only a remote member-server site.

It now contains:

- a local writable DC
- a local DNS server
- a local Global Catalog

This means the lab has reached a functional two-site AD state.

## Next likely tasks

- run broader `dcdiag` validation
- validate DNS health from both DCs
- review replication schedule and site link settings
- decide DHCP service design for SBY
- continue documenting site-specific standards and recovery approach