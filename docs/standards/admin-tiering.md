# Administrative Tiering and Access Authorization Standard

## Purpose

This document defines the access authorization model for the SBXQIAO lab.

The design uses a tiered administration model to separate identity control, server administration, and workstation/user support.

---

## Tier Model

### Tier 0

Tier 0 controls the identity and security plane of the environment.

Scope:

- Domain Controllers
- Active Directory
- DNS on domain controllers
- Group Policy administration
- privileged identity objects

Administrative account example:

- `bojie.qiao.t0`

Administrative group:

- `GG-SBX-T0-Admins`

Allowed logon targets:

- Domain Controllers
- approved Tier 0 administration workstation if implemented later

---

### Tier 1

Tier 1 controls member servers and infrastructure servers that are not domain controllers.

Scope:

- Windows member servers
- Linux servers
- application or utility servers
- file servers

Administrative account example:

- `bojie.qiao.t1`

Administrative group:

- `GG-SBX-T1-ServerAdmins`

Allowed logon targets:

- member servers
- approved administration workstation if implemented later

---

### Tier 2

Tier 2 controls user workstations and support operations.

Scope:

- user workstations
- endpoint support
- limited helpdesk tasks

Primary user account example:

- `bojie.qiao`

Support group:

- `GG-SBX-T2-Helpdesk`

Allowed logon targets:

- workstations only

---

## Authorization Rule

Access must follow this pattern:

User account → role group → permission assignment → resource

Permissions should not be assigned directly to individual users unless there is a documented exception.

---

## Resource Classification

### Tier 0 Resources

Examples:

- `SBX-DC1`
- `SBX-DC2`
- Active Directory administration
- GPO administration
- DNS administration on DCs

Authorized administrative group:

- `GG-SBX-T0-Admins`

### Tier 1 Resources

Examples:

- `SBX-LX1`
- `SBX-FS1`
- member servers
- infrastructure service hosts

Authorized administrative group:

- `GG-SBX-T1-ServerAdmins`

### Tier 2 Resources

Examples:

- `SBX-CL1`
- `SBX-CL2`
- user workstation fleet

Authorized support group:

- `GG-SBX-T2-Helpdesk`

---

## Logon Restrictions

### T0 Accounts

T0 accounts must only be used for Tier 0 administration tasks and should not be used for standard user activity.

### T1 Accounts

T1 accounts must not administer domain controllers and should only be used on member servers.

### Standard User Accounts

Standard user accounts must not receive privileged administrative rights.

---

## Group Standard

Core role groups:

- `GG-SBX-T0-Admins`
- `GG-SBX-T1-ServerAdmins`
- `GG-SBX-T2-Helpdesk`

Additional access groups may be created for specific services, such as:

- `GG-SBX-Server-RDP`
- `GG-SBX-Workstation-LocalAdmins`
- `GG-SBX-Samba-Share-RW`

---

## Design Goal

The goal of this model is to enforce privilege separation, reduce administrative cross-contamination, and support a realistic enterprise-style operating model inside the lab.