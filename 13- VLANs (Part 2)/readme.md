# Router-on-a-Stick Inter-VLAN Routing Lab

## Objective
Configure inter-VLAN routing using the Router-on-a-Stick method, allowing devices in multiple VLANs to communicate through a single router interface.

## Topics Covered
- VLAN Creation
- Access Port Configuration
- 802.1Q Trunk Configuration
- Router Subinterfaces
- Inter-VLAN Routing
- Troubleshooting Router-on-a-Stick

## Network Topology
- 2 Cisco Switches
- 1 Cisco Router
- 7 PCs
- VLANs:
  - VLAN 10
  - VLAN 20
  - VLAN 30

## Configuration Steps

### 1. Created VLANs
- VLAN 10
- VLAN 20
- VLAN 30

### 2. Configured Access Ports
Assigned each PC-facing switch port to its respective VLAN.

### 3. Configured Trunk Links
- SW1 ↔ SW2
- SW2 ↔ R1

Allowed VLANs:
- 10
- 20
- 30

Native VLAN:
- 1001 (Unused)

### 4. Configured Router Subinterfaces

Configured one subinterface for each VLAN using IEEE 802.1Q encapsulation.

Example:
- G0/0.10
- G0/0.20
- G0/0.30

Each subinterface was assigned the default gateway IP for its VLAN.

### 5. Verification
Used the following commands:
- `show vlan brief`
- `show interfaces trunk`
- `show ip interface brief`
- `ping`

## Issue Encountered

VLAN 20 and VLAN 30 communicated successfully, but VLAN 10 could not communicate.

### Root Cause

The router subinterface for VLAN 10 was configured with an incorrect IP address:

Incorrect:
```

10.10.10.62

```

Correct:
```

10.0.0.62

```

Because the default gateway was in the wrong network, hosts in VLAN 10 could not reach the router or other VLANs.

## Resolution

Updated the router subinterface with the correct IP address:

```

interface GigabitEthernet0/0.10
ip address 10.0.0.62 255.255.255.192

```

After correcting the IP address, VLAN 10 communication and inter-VLAN routing worked successfully.

## Key Learnings

- Router subinterface IP addresses must belong to the correct subnet.
- Verify VLAN membership before troubleshooting routing.
- Use `show interfaces trunk` to confirm allowed VLANs.
- Use `show ip interface brief` to verify router interface status.
- A single incorrect gateway IP can prevent an entire VLAN from communicating.

## Outcome

✅ Successfully configured Router-on-a-Stick inter-VLAN routing.

✅ Verified communication between VLAN 10, VLAN 20, and VLAN 30.

✅ Identified and resolved a router subinterface IP addressing issue through systematic troubleshooting.