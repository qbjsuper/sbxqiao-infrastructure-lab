[2026-03-17 17:10 AEST]

## Current AD Design State

### Overview

The Active Directory environment is now operating as a **single-forest, single-domain, multi-site** design.

* **Forest:** `sbxqiao.lab`
* **Domain:** `sbxqiao.lab`

This remains one domain across both sites. The design does **not** currently use child domains or multiple forests.

### Site and subnet design

The directory now contains active site-aware configuration for the two intended locations:

* **SBX** mapped to `172.16.50.0/24`
* **SBY** mapped to `172.16.51.0/24`

The default site object still exists in Active Directory:

* `Default-First-Site-Name`

This is currently leftover scaffolding rather than the intended production-style layout. The real operational site model is now **SBX + SBY**.

### Domain controller layout

The domain currently has two writable domain controllers, one in each site:

* **SBX-DC1.sbxqiao.lab**

  * Site: `SBX`
  * IPv4: `172.16.50.10`
  * Global Catalog: `True`

* **SBY-DC1.sbxqiao.lab**

  * Site: `SBY`
  * IPv4: `172.16.51.10`
  * Global Catalog: `True`

Both domain controllers are functioning as:

* writable DCs
* Global Catalog servers
* DNS-integrated AD participants

Cross-site replication and DNS replication have been validated successfully.

### Replication and service state

Validation work confirms that the two-site domain is operational.

Confirmed working:

* Active Directory replication between `SBX-DC1` and `SBY-DC1`
* DNS application partition replication
* `SYSVOL` and `NETLOGON` shares on both DCs
* DC advertising
* KDC, LDAP, GC, and time service roles
* site-aware DC placement

This means the environment has moved beyond simple domain join status and is now functioning as a proper multi-site AD deployment.

---

## OU Design

### Tiering model

The OU design shows an enterprise-style **tiered administration structure**.

Main tier roots currently present:

* `SBX-T0`
* `SBX-T1`
* `SBX-T2`

This indicates the environment is being organised around three main administrative/security tiers:

* **T0** — domain and identity control plane
* **T1** — server and infrastructure administration
* **T2** — user, workstation, and general access tier

### Key OU branches

The exported structure shows the following notable branches and role separations:

Under **SBX-T0**:

* `PrivilegedAccounts`
* `PrivilegedGroups`
* `DomainControllers` (custom OU path currently used by `SBX-DC1`)

Under **SBX-T1**:

* `PrivilegedAccounts`
* `Servers`
* `JumpHosts`

Under **SBX-T2**:

* `Accounts`
* `Workstations`

Additional top-level supporting OUs also exist, including:

* `SBX-AdminWorkstations`
* `SBX-Disabled`
* `SBX-Groups`

### Interpretation

The OU design is clearly intended to separate:

* privileged identities
* privileged groups
* servers
* workstations
* standard user identities
* disabled or administrative support objects

This is a sound structure for a lab that is meant to practise tiering, role separation, and policy scoping.

---

## Current object placement

### Computer objects

Current computer object placement reflects the tier model, but also reveals one design inconsistency.

Known placements:

* **SBX-DC1**

  * `OU=DomainControllers,OU=SBX-T0,...`

* **SBY-DC1**

  * `OU=Domain Controllers,...`

* **SBX-LX1**

  * `OU=Servers,OU=SBX-T1,...`

* **SBX-CL1**

  * `OU=Workstations,OU=SBX-T2,...`

* **SBX-CL2**

  * `OU=Workstations,OU=SBX-T2,...`

### Assessment

Most computer placement aligns with the intended tiering design.

The main exception is domain controller placement:

* `SBX-DC1` is in a **custom T0 DomainControllers OU**
* `SBY-DC1` is in the **standard built-in Domain Controllers OU**

This makes the DC design inconsistent.

### Recommended target state

Because Active Directory already provides a standard and expected OU for domain controllers, the cleaner design is:

* keep **all DCs** in the built-in **Domain Controllers** OU

That aligns better with:

* standard AD behaviour
* default domain controller policy processing
* built-in health checks such as `dcdiag`

---

## User design

### User placement

The exported directory data shows that user objects are also being separated by role and privilege tier.

Examples include:

* standard user account:

  * `Bojie Qiao` in `OU=Accounts,OU=SBX-T2,...`

* privileged admin accounts:

  * `bojie.qiao.t0` in `OU=PrivilegedAccounts,OU=SBX-T0,...`
  * `bojie.qiao.t1` in `OU=PrivilegedAccounts,OU=SBX-T1,...`

### Interpretation

This confirms that the directory is already modelling:

* standard identity
* T0 administrative identity
* T1 administrative identity

That is a strong and useful lab pattern for practising privilege separation and administrative tiering.

---

## Group Policy Design

### GPO inventory

The environment currently contains the following GPOs:

* `Default Domain Policy`
* `Default Domain Controllers Policy`
* `SBX-Baseline-Domain`
* `SBX-T0-Baseline`
* `SBX-T0-Enforcement`
* `SBX-T1-Baseline`
* `SBX-T1-Enforcement`
* `SBX-T2-Baseline`
* `SBX-T2-Enforcement`

### Policy model

The GPO structure is organised in a clear layered pattern.

At the **domain level**:

* `Default Domain Policy`
* `SBX-Baseline-Domain`

At the **Domain Controllers OU**:

* `Default Domain Controllers Policy`

At each **tier root OU**:

* one **Baseline** GPO
* one **Enforcement** GPO

So the intended design pattern is:

* domain-wide baseline policy
* default Microsoft baseline policies retained
* separate baseline and enforcement policy pairs for each administrative tier

### Interpretation

This is a clean and scalable Group Policy layout.

It gives you:

* consistent naming
* predictable scope boundaries
* room to add settings gradually without losing structure

### Current maturity

At least some custom GPOs appear to exist mainly as framework objects and may still contain little or no actual policy content.

That means the **GPO architecture is established**, but the **policy payload** is still developing.

This is normal at the current stage of the lab.

---

## Architecture summary

The environment can now be described as follows:

The lab is a **single-domain Active Directory deployment spanning two sites**, `SBX` and `SBY`, with one writable Global Catalog domain controller in each site. Site awareness is implemented through subnet-to-site mapping for `172.16.50.0/24` and `172.16.51.0/24`. The logical directory structure uses a **tiered OU design** based on `SBX-T0`, `SBX-T1`, and `SBX-T2`, separating privileged administration, servers, workstations, and standard user identities. Group Policy is organised by tier using a **Baseline + Enforcement** model, while the Microsoft default domain and domain controller policies remain in place.

---

## What is already strong

The current AD design is strong in these areas:

* site-aware multi-site structure
* one DC per site
* successful replication across sites
* DNS-integrated multi-DC design
* tiered OU concept
* privileged account separation
* structured GPO naming and linking pattern

These elements mean the environment now has a real architectural shape, not just a collection of joined machines.

---

## What is still unfinished or inconsistent

### 1. Domain controller OU placement

This is the most important directory design inconsistency right now.

Current state:

* `SBX-DC1` in custom T0 DC OU
* `SBY-DC1` in standard Domain Controllers OU

Recommended end state:

* both DCs in the built-in **Domain Controllers** OU

### 2. Default site object remains

`Default-First-Site-Name` still exists.

This is not currently harmful, but it is a leftover object rather than part of the intended target design.

### 3. Custom GPO content still needs maturity

The GPO framework exists, but some custom policies may still be placeholder objects with limited settings.

### 4. OU naming is SBX-centric

The tier roots are named `SBX-T0`, `SBX-T1`, and `SBX-T2`.

Now that the domain spans both `SBX` and `SBY`, this raises a future naming/design question:

* are these tiers meant to be **domain-global administrative tiers**
* or specifically **SBX-local tiers**

This is not an immediate problem, but it is worth deciding later for consistency.

---

## Recommended next design actions

1. Move `SBX-DC1` into the standard **Domain Controllers** OU.
2. Keep the current site/subnet model as-is.
3. Leave `Default-First-Site-Name` alone until the environment is stable and documented.
4. Continue populating the custom tier GPOs with actual policy settings.
5. Later review whether the `SBX-` prefix on tier OUs still matches the long-term design intent.

---

## Final assessment

The AD environment is now a functioning **two-site, single-domain, tier-structured lab**.

It has:

* a valid site model
* a valid domain controller topology
* a clear OU design intent
* a clear Group Policy framework
* working replication and core DC services

The environment is no longer at the “basic build” stage. It now has a defined architecture and only needs directory hygiene and policy maturity to become cleaner and more consistent.
