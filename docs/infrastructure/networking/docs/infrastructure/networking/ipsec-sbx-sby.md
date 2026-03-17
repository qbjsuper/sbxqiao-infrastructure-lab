# SBX–SBY IPsec Site-to-Site Build

## Purpose

This document records the first-pass IPsec transport build between the SBX and SBY sites.

The objective of this stage was to establish secure routed connectivity between:

- `172.16.50.0/24` at SBX
- `172.16.51.0/24` at SBY

This stage was intentionally built as a **network transport first** milestone before expanding SBY into a fully configured AD site service node.

---

## Design Scope

### Site LANs protected by the tunnel

- **SBX LAN:** `172.16.50.0/24`
- **SBY LAN:** `172.16.51.0/24`

### Underlay / WAN transport

- **Underlay network:** `192.168.0.0/24`
- **Home router / gateway:** `192.168.0.1`

### WAN peer addresses

- **pfSense-SBX WAN:** `192.168.0.252`
- **pfSense-SBY WAN:** `192.168.0.253`

---

## Implemented State

### IPsec tunnel status

The first-pass site-to-site tunnel is operational.

#### Phase 1

- **Status:** Established
- **Type:** IKEv2
- **Authentication:** Mutual PSK
- **SBX peer:** `192.168.0.252`
- **SBY peer:** `192.168.0.253`

#### Phase 2

- **Status:** Installed
- **SBX local network:** `172.16.50.0/24`
- **SBY remote network:** `172.16.51.0/24`

On the reverse side:

- **SBY local network:** `172.16.51.0/24`
- **SBX remote network:** `172.16.50.0/24`

### Cryptographic baseline

#### Phase 1 baseline

- Key Exchange version: `IKEv2`
- Internet Protocol: `IPv4`
- Authentication method: `Mutual PSK`
- My identifier: `My IP address`
- Peer identifier: `Peer IP address`
- Encryption: `AES-256`
- Hash: `SHA-256`
- Diffie-Hellman group: `14`

#### Phase 2 baseline

- Mode: `Tunnel IPv4`
- Protocol: `ESP`
- Encryption: `AES-256`
- Hash: `SHA-256`
- PFS group: `14`

---

## Validation Outcome

### Successful validations

The following validations were completed successfully:

- WAN reachability from `192.168.0.252` to `192.168.0.1`
- WAN reachability from `192.168.0.253` to `192.168.0.1`
- WAN peer reachability between:
  - `192.168.0.252`
  - `192.168.0.253`
- Phase 1 establishment between SBX and SBY
- Phase 2 installation for:
  - `172.16.50.0/24`
  - `172.16.51.0/24`
- ICMP traffic successfully passing through the IPsec tunnel after firewall rule correction

### Current confirmed transport result

Secure routed transport is now working between:

- `172.16.50.0/24`
- `172.16.51.0/24`

This confirms that the first-pass encrypted site-to-site path is available for later multi-site service validation.

---

## Troubleshooting Record

### 1. Initial WAN peer reachability failure

#### Symptom

The two pfSense WAN addresses could both reach the home router at `192.168.0.1`, but could not ping each other.

- `192.168.0.252` → `192.168.0.1` worked
- `192.168.0.253` → `192.168.0.1` worked
- `192.168.0.252` ↔ `192.168.0.253` failed initially

#### Verified non-issues

The following were checked and confirmed correct:

- WAN IP addressing
- subnet mask
- default gateway
- Hyper-V external vSwitch attachment

#### Root cause

The immediate cause was inbound ICMP being blocked on the WAN interfaces.

#### Fix applied

Temporary WAN ICMP allow rules were added on both pfSense firewalls for peer testing between:

- `192.168.0.252`
- `192.168.0.253`

#### Result

WAN peer-to-peer ping then succeeded, confirming that underlay transport was working.

---

### 2. IPsec status confusion during build

#### Symptom

During the build process, the IPsec GUI behaviour appeared inconsistent, including moments where the entry looked disabled or the status page appeared misleading.

#### Operational conclusion

The live tunnel state must be verified from the actual IPsec status entries and Security Association state, not only from a banner message or a row toggle interpretation.

#### Final confirmed state

The final operational state on both sides showed:

- Phase 1 established
- Phase 2 installed

This confirmed that the tunnel configuration itself was active.

---

### 3. Tunnel established but traffic initially failed

#### Symptom

Phase 1 and Phase 2 came up, but initial ping across the tunnel failed.

#### Observed behaviour from counters

The IPsec counters showed an asymmetric pattern:

- SBY was sending traffic
- SBX was receiving traffic
- SBX was not sending matching return traffic

#### Root cause

The **IPsec firewall rule direction** was incorrect.

This was the key troubleshooting finding.

On pfSense, rules on the **IPsec** tab apply to traffic as it **enters from the remote side**.  
That means the rule source must reference the **remote subnet**, not the local subnet.

#### Incorrect logic initially used

The earlier rule logic treated IPsec rules like local outbound policy, which reversed source and destination expectations.

#### Corrected rule direction

##### On SBX

- **Source:** `172.16.51.0/24`
- **Destination:** `172.16.50.0/24`

##### On SBY

- **Source:** `172.16.50.0/24`
- **Destination:** `172.16.51.0/24`

#### Result

After correcting the IPsec rule direction, ping across the tunnel succeeded.

This confirmed that:

- the tunnel itself was healthy
- the issue was traffic filtering logic, not Phase 1 or Phase 2 establishment

---

## Operational Notes

- This is a **transport milestone**, not a full multi-site AD completion point
- `sby-dc1` exists but is not yet treated as a fully configured site service node
- WAN ICMP rules were temporary troubleshooting controls, not the long-term intended WAN policy
- IPsec rules should remain narrow during early validation
- additional application traffic should only be opened deliberately after service requirements are defined

---

## Current State Summary

### Established

- working WAN underlay between both pfSense WAN peers
- working site-to-site IPsec tunnel between SBX and SBY
- protected subnet path between:
  - `172.16.50.0/24`
  - `172.16.51.0/24`
- successful ICMP validation across the tunnel

### Troubleshooted and resolved

- WAN peer ping failure caused by WAN-side ICMP filtering
- misleading or confusing GUI state during IPsec build
- tunnel-up-but-no-traffic condition caused by reversed IPsec firewall rule direction

### Not yet completed

- `sby-dc1` configuration
- cross-site DNS service validation
- AD Sites and Services mapping
- cross-site AD replication validation

---

## Next Stage

The next planned stage after transport validation is:

1. configure `sby-dc1`
2. validate DNS reachability across the tunnel
3. define site/subnet mapping in AD Sites and Services
4. validate replication only after site services are ready