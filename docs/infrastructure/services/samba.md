# Samba Service Record

## Service Identity

| Item | Value |
|---|---|
| Service Name | Samba |
| Service Host | `sbx-lx1.sbxqiao.lab` |
| Primary Share | `Shared` |
| Share Path | `/srv/samba/shared` |
| Authentication Context | `SBXQIAO.LAB` |
| Status | Implemented and validated |

## Purpose

Samba provides SMB file-sharing services from the Linux member server.

Its purpose is to:
- provide a domain-authenticated shared folder
- validate Linux integration with Active Directory
- support Windows client access to Linux-hosted file shares
- support learning for Samba, permissions, and identity integration

## Current Implementation

The current Samba implementation is hosted on `sbx-lx1`.

Current characteristics:
- server role is member server
- AD integration path is based on winbind
- normal domain-user authentication has been validated
- Windows client access has been validated
- SSH is used for Linux-side administration and troubleshooting

## Current Share Model

The currently documented primary share is:

- Share name: `Shared`
- Backing path: `/srv/samba/shared`
- Access model: authorised domain users
- intended use: share access testing and file-service validation

The current backing directory model aligns Linux permissions and ACLs with domain-authenticated access.

## Dependent Services

The Samba service depends on:
- Active Directory authentication
- DNS resolution
- healthy domain join for `sbx-lx1`
- winbind identity resolution
- Linux filesystem permissions on the backing directory

## Validation State

The Samba service should be considered healthy when:
- the Linux server remains joined to the domain
- winbind resolves users and groups correctly
- the `Shared` share is visible to authorised users
- share listing succeeds
- file and folder creation succeeds
- Windows client access succeeds
- access remains stable after service restart or reboot

## Current Validation Summary

The currently validated behaviour includes:
- successful domain-authenticated share access
- successful server-side Samba testing
- successful Windows client access from SBX
- successful file and folder creation through the share
- successful second-client access from `sbx-cl2`
- successful cross-user file modification

Samba for the current SBX lab stage is considered operational and validated.

## Operational Notes

Samba access failures in this lab should be analysed in layers:
1. domain join and trust
2. DNS and name resolution
3. winbind identity resolution
4. Samba share configuration
5. Linux backing-directory permissions and ACLs

This layered approach is important because authentication success does not automatically mean filesystem authorisation is correct.

## Notes

This document records the implemented Samba service state.

Detailed troubleshooting history and per-test outcomes should be stored in operations records and checkpoints.