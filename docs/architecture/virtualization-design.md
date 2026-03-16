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