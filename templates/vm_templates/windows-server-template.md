# Windows Server VM Template Standard

## Purpose

Base template for Windows Server virtual machines used in the SBXQIAO Infrastructure Lab.

---

## Baseline

- Hyper-V virtual machine
- Generation 2
- updates applied
- local administrator access verified
- suitable for infrastructure role deployment
- checkpoint created before role-specific configuration

---

## Intended Uses

- Domain controllers
- utility servers
- management servers

---

## Post-Clone Tasks

1. rename computer
2. configure static IP
3. verify DNS settings
4. verify time source
5. join domain or promote as required
6. create host record in `docs/infrastructure/hosts/`