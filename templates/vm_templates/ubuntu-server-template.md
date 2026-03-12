# Ubuntu Server VM Template Standard

## Purpose

Base template for Ubuntu Server virtual machines used in the SBXQIAO Infrastructure Lab.

---

## Baseline

- OpenSSH installed
- package baseline updated
- suitable for infrastructure role deployment
- checkpoint created before domain integration or major service changes

---

## Intended Uses

- Linux member servers
- Samba servers
- utility servers

---

## Post-Clone Tasks

1. set hostname
2. configure static IP
3. verify DNS resolution
4. verify time synchronisation
5. join AD if required
6. create host record in `docs/infrastructure/hosts/`