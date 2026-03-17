# SBXQIAO Infrastructure Lab

Enterprise-style infrastructure lab built on Hyper-V to practise Windows Server, Linux administration, networking, and multi-site Active Directory operations.

## Current Design State

The lab currently uses a single forest and single domain design.

- Forest: `sbxqiao.lab`
- Domain: `sbxqiao.lab`

The environment is being developed as a multi-site lab.

### Sites
- SBX — Primary site
- SBY — Secondary site

### Core Services
- Active Directory Domain Services
- DNS
- pfSense routing and firewall
- Linux AD member integration
- Samba file services
- Time synchronisation validation
- Site-to-site IPsec VPN

## Current Infrastructure

### SBX Site
- Subnet: `172.16.50.0/24`
- Gateway: `172.16.50.1`
- Site edge firewall/router: `pfSense-SBX`
- WAN transit IP: `192.168.0.252/24`
- `sbx-dc1` — Domain Controller / DNS
- `sbx-lx1` — Linux member server / Samba
- `sbx-cl2` — Windows client workstation / Samba validation

### SBY Site
- Subnet: `172.16.51.0/24`
- Gateway: `172.16.51.1`
- Site edge firewall/router: `pfSense-SBY`
- WAN transit IP: `192.168.0.253/24`
- `sby-dc1` — Planned additional Domain Controller / DNS

## Connectivity Model

Each site uses its own pfSense firewall/router as the local default gateway.

- SBX local gateway: `172.16.50.1`
- SBY local gateway: `172.16.51.1`

The two physical Hyper-V hosts are connected through the home-router Ethernet network on `192.168.0.0/24`.

- Physical Hyper-V hosts remain in workgroup mode; only lab VMs join the `sbxqiao.lab` domain.

That home-router network is the underlay transport path between the two site edge firewalls.
Inter-site communication is carried through a site-to-site IPsec VPN between:
- `pfSense-SBX` WAN `192.168.0.252/24`
- `pfSense-SBY` WAN `192.168.0.253/24`

## DNS Model

pfSense provides routing and firewall functions.

Active Directory DNS remains on the domain controllers. Domain members must use AD DNS, not public DNS, for domain operations.

Before promotion, `sby-dc1` should use `172.16.50.10` as its preferred DNS server.

## Repository Structure
- `docs/architecture/` — design documents
- `docs/infrastructure/` — implemented sites, hosts, and services
- `docs/deployment/` — build order and deployment runbooks
- `docs/operations/` — validation, checkpoints, and troubleshooting
- `docs/standards/` — lab standards and workflow rules
- `templates/` — VM templates and documentation templates
- `journal/` — temporary working notes only

## Workflow

All meaningful lab actions follow:
- `docs/standards/lab-action-workflow.md`

Documentation placement and structure are defined in:
- `docs/standards/documentation-standard.md`

