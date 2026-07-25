# Day 18 – Replacing Router-on-a-Stick (ROAS) with a Multilayer Switch

## Objective

This lab demonstrates how to replace a traditional Router-on-a-Stick (ROAS) design with a Layer 3 Multilayer Switch. The multilayer switch performs inter-VLAN routing using Switch Virtual Interfaces (SVIs), while a point-to-point Layer 3 link connects the switch to the router.

---

## Topology

- 1 × Cisco 2911 Router (R1)
- 1 × Cisco 3560 Multilayer Switch (SW2)
- 1 × Cisco 2960 Layer 2 Switch (SW1)
- 7 × PCs

---

## Network Addressing

| VLAN | Network | Default Gateway (SVI) |
|------|---------|-----------------------|
| VLAN 10 | 10.0.0.0/26 | 10.0.0.62 |
| VLAN 20 | 10.0.0.64/26 | 10.0.0.126 |
| VLAN 30 | 10.0.0.128/26 | 10.0.0.190 |

### Layer 3 Link

| Device | Interface | IP Address |
|---------|-----------|------------|
| R1 | G0/0 | 10.0.0.194/30 |
| SW2 | G1/0/2 | 10.0.0.193/30 |

Network: **10.0.0.192/30**

---

## Tasks Performed

- Removed Router-on-a-Stick configuration from R1.
- Converted the R1–SW2 connection into a routed Layer 3 link.
- Configured SVIs for VLANs 10, 20, and 30.
- Assigned the last usable IP address of each subnet as the default gateway.
- Enabled IP routing on the multilayer switch.
- Configured a default route on SW2 pointing to R1.
- Verified inter-VLAN communication.
- Verified Internet connectivity through R1.

---

## Configuration Summary

### Router (R1)

- Removed subinterfaces.
- Configured G0/0 with:
  - **10.0.0.194/30**
- Existing routes to the Internet were retained.

### Multilayer Switch (SW2)

- Enabled Layer 3 routing.
- Configured SVIs:
  - VLAN 10 → 10.0.0.62/26
  - VLAN 20 → 10.0.0.126/26
  - VLAN 30 → 10.0.0.190/26
- Converted interface G1/0/2 into a routed port.
- Configured:
  - 10.0.0.193/30
- Added default route:
  ```
  ip route 0.0.0.0 0.0.0.0 10.0.0.194
  ```

### Layer 2 Switch (SW1)

- Maintained VLAN configuration.
- Maintained trunk link to SW2.
- Access ports remained assigned to their respective VLANs.

---

## Verification

Commands used:

```
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show running-config
ping
```

---

## Connectivity Tests

- ✅ VLAN 10 ↔ VLAN 20
- ✅ VLAN 10 ↔ VLAN 30
- ✅ VLAN 20 ↔ VLAN 30
- ✅ PCs successfully used their SVI as the default gateway.
- ✅ Internet connectivity verified by pinging **1.1.1.1**.

---

## Concepts Practiced

- Layer 3 Switching
- Switch Virtual Interfaces (SVIs)
- Inter-VLAN Routing
- Routed Ports
- Default Routes
- Static Routing
- Trunk Links
- Layer 2 vs Layer 3 Switching
- Replacing Router-on-a-Stick Architecture

---

## Outcome

Successfully migrated from a Router-on-a-Stick architecture to a Multilayer Switch-based design. Inter-VLAN routing is now handled directly by the Layer 3 switch, reducing router load and improving network performance while maintaining Internet connectivity through the router.