# Virtualization Design

This document describes the Hyper-V virtualisation approach used in the SBXQIAO Infrastructure Lab.

---

## Platform

The lab runs on Hyper-V.

Virtual machines are deployed in a staged and documented way rather than being created ad hoc.

---

## Design Principles

- use repeatable VM templates
- create checkpoints at stable milestones
- apply static addressing for infrastructure servers
- document each host before major role changes
- validate time, DNS, and connectivity before domain operations

---

## VM Template Strategy

The lab uses prebuilt VM templates as the baseline for new systems.

### Windows Server Template

Used for:

- `sbx-dc1`
- `sby-dc1`
- future Windows infrastructure servers

### Ubuntu Server Template

Used for:

- `sbx-lx1`
- future Linux infrastructure servers

---

## Operational Rule

For every new infrastructure VM:

1. deploy from template
2. rename host
3. configure network
4. validate DNS and time
5. install or join role
6. validate service
7. document host state
8. create checkpoint at stable milestone

---

## Hyper-V Host Management Model

### Physical host operating model

The two physical Hyper-V host PCs remain in a **workgroup configuration**.

They are **not joined** to the `sbxqiao.lab` Active Directory domain.

This design keeps the host operating systems separate from the lab domain so that the base platform remains stable, simple to recover, and independent from lab-side directory, DNS, or Group Policy issues.

### Domain membership boundary

Domain membership applies to **lab virtual machines only**.

Examples:

- Domain controllers
- Member servers
- Lab client virtual machines

The physical Windows host PCs act as the **virtualisation platform layer** and remain outside the domain boundary.

### Multi-host Hyper-V management approach

The lab uses **two independent Hyper-V hosts**:

- SBX host PC
- SBY host PC

These hosts are managed from a **single Hyper-V Manager console** for convenience.

This is a **remote management model only** and does not imply:

- failover clustering
- live migration
- shared storage
- automatic high availability

Each Hyper-V host remains a separate compute and storage boundary.

### Management rationale

This approach was selected for the following reasons:

1. Keeps the physical host PCs isolated from lab Active Directory dependency
2. Reduces risk to the daily-use host operating systems
3. Preserves a cleaner separation between the platform layer and the lab workload layer
4. Still allows centralised administration of both Hyper-V hosts from one console

### Operational note

Because the Hyper-V hosts are not domain-joined, remote management between them requires additional workgroup-compatible configuration, such as:

- remote management enablement
- firewall allowance
- WinRM trust configuration where required
- local administrative permission alignment

This configuration is accepted as a trade-off for keeping the host layer outside the lab domain.

### Design outcome

The final model is:

- **Physical hosts:** workgroup
- **Lab VMs:** domain-joined where required
- **Hyper-V management:** centralised through one Hyper-V Manager console
- **Cluster status:** not clustered

## Workgroup Multi-Host Hyper-V Manager Access

### Objective

Manage both physical Hyper-V host PCs from a single Hyper-V Manager console while keeping the physical hosts outside the `sbxqiao.lab` domain.

### Host model

The physical Hyper-V hosts remain in a **workgroup** configuration.

Current hosts:

- `QIAO-AU` — primary Hyper-V host
- `BOJIE_MS_A2` — secondary Hyper-V host

This provides **centralised management only**. It does **not** provide:

- failover clustering
- live migration
- shared storage
- automatic high availability

### Authentication design

Because the physical hosts are not domain-joined, standard domain Kerberos-based remote management is not available.

A dedicated local administrative account is used for remote Hyper-V management:

- account name: `hvadmin`
- account scope: local account on each physical Hyper-V host
- password: same password on both host PCs

On the remote Hyper-V host, the `hvadmin` account must be a member of:

- `Administrators`
- `Hyper-V Administrators`
- `Remote Management Users`

### Remote management prerequisites

The following prerequisites were required on the remote Hyper-V host:

- network reachability between both physical hosts
- WinRM enabled
- PowerShell remoting enabled
- firewall rules allowing remote management traffic
- local administrative rights
- Hyper-V administrative rights

### Working launch method

Direct connection from a normal Hyper-V Manager session produced workgroup authentication and authorisation issues.

The working method is to launch Hyper-V Manager from the primary host using `runas /netonly` with the remote host local administrative identity.

Command used from `QIAO-AU`:

```cmd
runas /netonly /user:BOJIE_MS_A2\hvadmin "C:\Windows\System32\mmc.exe C:\Windows\System32\virtmgmt.msc"

This starts Hyper-V Manager locally on QIAO-AU but uses the remote host account BOJIE_MS_A2\hvadmin for network authentication.

Operational workflow

Launch Hyper-V Manager with the runas /netonly command

Connect to remote Hyper-V host BOJIE_MS_A2

In the same console, connect to the local host QIAO-AU

Use the left pane to switch between hosts:

select BOJIE_MS_A2 to manage mini PC VMs

select QIAO-AU to manage big PC VMs

Result

A single Hyper-V Manager console now manages both physical Hyper-V hosts:

BOJIE_MS_A2

QIAO-AU

This meets the design goal of managing multiple Hyper-V hosts in one Hyper-V Manager while preserving the workgroup boundary on the physical hosts.

Notes

This is a management-plane solution, not a cluster design.

Each host retains its own separate compute, storage, and VM inventory.

VM visibility depends on which host is selected in the Hyper-V Manager left pane.