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
- `sbx-dc2`
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