# Time Synchronisation Service Record

## Service Identity

| Item | Value |
|---|---|
| Service Name | Time Synchronisation |
| Primary Time Authority | `sbx-dc1` |
| Current Scope | SBX site systems |
| Status | Implemented and under validation |

## Purpose

Time synchronisation supports authentication and service stability across the lab.

Its purpose is to:
- keep domain members within acceptable time skew
- support Kerberos authentication
- support reliable domain operations
- support repeatable troubleshooting and validation

## Current Implementation

The current time-synchronisation model is centred on the domain controller.

At the current stage:
- `sbx-dc1` acts as the primary time source within the SBX domain environment
- domain members should align with the domain controller
- Linux member-server time behaviour has been part of Active Directory integration validation
- time-service correctness remains an operationally important dependency for Samba and domain authentication

## Dependent Systems

Time synchronisation is important for:
- `sbx-dc1`
- `sbx-lx1`
- domain-joined Windows clients
- Kerberos authentication workflows
- Samba AD-integrated access

## Validation State

The time-synchronisation layer should be considered healthy when:
- domain members report correct system time
- client time does not drift beyond Kerberos tolerance
- Linux member systems can follow the domain time source correctly
- authentication failures are not caused by time skew
- troubleshooting confirms expected source behaviour

## Operational Notes

Time-source design and troubleshooting should be documented carefully because time errors can appear as:
- domain join failures
- Kerberos errors
- Samba authentication issues
- inconsistent client behaviour

Time validation should be part of any serious AD integration troubleshooting workflow.

## Notes

This document records the implemented time-synchronisation role in the lab.

Detailed step-by-step troubleshooting and validation procedures should be stored in operations documentation.