# CCNA Packet Tracer Lab 16 – VLAN Configuration and Inter-VLAN Routing

## Overview

This lab focuses on configuring Virtual Local Area Networks (VLANs) and enabling communication between them using Router-on-a-Stick (ROAS). VLANs are created for different departments, switch ports are assigned to the appropriate VLANs, and router subinterfaces are configured to provide inter-VLAN routing.

---

## Objective

- Configure IPv4 addresses on all PCs.
- Configure the default gateway for each VLAN.
- Create VLANs on the switch.
- Assign switch ports to the appropriate VLANs.
- Configure Router-on-a-Stick using router subinterfaces.
- Configure an 802.1Q trunk between the router and switch.
- Verify connectivity within and between VLANs.
- Observe broadcast behavior using Simulation Mode.

---

## Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 1941 Router | 1 |
| Cisco Switch | 1 |
| PCs | 6 |

---

## VLAN Configuration

| VLAN | Department | Network |
|------|------------|---------|
| VLAN 10 | Engineering | 10.0.0.0/26 |
| VLAN 20 | HR | 10.0.0.64/26 |
| VLAN 30 | Sales | 10.0.0.128/26 |

---

## Tasks Performed

- Configured IPv4 addresses and subnet masks on all PCs.
- Assigned the last usable IP address of each subnet as the default gateway.
- Created VLANs 10, 20, and 30 on the switch.
- Assigned access ports to the appropriate VLANs.
- Named each VLAN based on its department.
- Configured router subinterfaces with IEEE 802.1Q encapsulation.
- Configured the switch port connected to the router as a trunk.
- Verified inter-VLAN connectivity using ICMP ping.
- Observed broadcast traffic within VLANs using Packet Tracer Simulation Mode.

---

## IOS Commands Practiced

- `vlan`
- `name`
- `switchport mode access`
- `switchport access vlan`
- `switchport mode trunk`
- `interface`
- `encapsulation dot1Q`
- `ip address`
- `no shutdown`
- `show vlan brief`
- `show interfaces trunk`
- `ping`

---

## Key Concepts Learned

- VLAN creation
- Access port configuration
- Trunk port configuration
- IEEE 802.1Q encapsulation
- Router-on-a-Stick (ROAS)
- Inter-VLAN routing
- Broadcast domains
- VLAN verification and troubleshooting

---

## Files Included

```
Day-16-VLAN-Configuration-and-Inter-VLAN-Routing/
│
├── Day 16 Lab - VLANs (Part 1).pkt
├── topology.png
└── readme.md
```

---

## Learning Outcome

After completing this lab, I can:

- Create and configure VLANs on a Cisco switch.
- Assign switch ports to specific VLANs.
- Configure an 802.1Q trunk between a router and switch.
- Implement Router-on-a-Stick for inter-VLAN routing.
- Verify VLAN and trunk configurations using Cisco IOS.
- Understand broadcast behavior within VLANs.

---

**Status:** ✅ Completed