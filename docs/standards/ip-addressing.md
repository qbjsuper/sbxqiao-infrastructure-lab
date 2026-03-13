# IP Addressing Standard

## Purpose

This document defines the IP addressing plan for the SBXQIAO infrastructure lab.

The goal is to maintain a simple, consistent, and scalable addressing structure.

---

## Primary Network

Infrastructure LAN:


172.16.50.0 /24


This network hosts the SBX site infrastructure.

Gateway:


172.16.50.1


Provided by:


SBX-RTR1 (pfSense)


---

## Infrastructure Address Allocation

### Router


SBX-RTR1
172.16.50.1


---

### Domain Controllers


SBX-DC1
172.16.50.10

SBX-DC2
172.16.50.11


---

### Workstations


SBX-CL1
172.16.50.20
SBX-CL2
172.16.50.21


---

### Linux Servers


SBX-LX1
172.16.50.30


---

### Future Infrastructure Servers

Examples:


SBX-FS1
SBX-JMP1
SBX-MON1


Suggested address range:


172.16.50.40 – 172.16.50.80


---

## Addressing Rules

All infrastructure machines must use **static IP addresses**.

The following rules apply:

- gateway: `172.16.50.1`
- DNS server: `172.16.50.10`
- subnet mask: `/24`

Clients must **never use public DNS directly**.

All DNS queries must go through the domain controller.

---

## Future Expansion

Additional sites may use separate subnets.

Example:


SBY Site
172.16.51.0 /24


Routing between sites will occur through controlled infrastructure gateways.

---

## Design Goal

The addressing structure must remain simple enough for manual management