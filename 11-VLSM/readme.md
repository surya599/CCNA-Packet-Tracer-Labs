# CCNA Packet Tracer Lab 15 – IPv4 Subnetting and Static Routing

## Overview

This lab focuses on subnetting a single IPv4 network using Variable Length Subnet Masking (VLSM) to accommodate multiple LANs with different host requirements. After designing the subnetting scheme, static routes are configured to enable communication between all LANs.

---

## Objective

- Subnet the 192.168.5.0/24 network using VLSM.
- Allocate appropriate subnets based on host requirements.
- Assign the first usable IP address to each PC.
- Assign the last usable IP address to each router LAN interface.
- Configure the point-to-point link between the routers.
- Configure static routes to enable communication between all networks.
- Verify end-to-end connectivity.

---

## Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2911 Router | 2 |
| Cisco 2960 Switch | 4 |
| PCs | 4 |

---

## Base Network

| Network | Address |
|---------|----------|
| Available Address Space | 192.168.5.0/24 |

---

## Tasks Performed

- Designed a VLSM addressing scheme for four LANs and one point-to-point network.
- Assigned IPv4 addresses to all router interfaces and PCs.
- Configured the first usable IP address on each PC.
- Configured the last usable IP address on each router LAN interface.
- Configured the point-to-point connection between R1 and R2.
- Configured static routes on both routers.
- Verified connectivity between all PCs using ICMP ping.

---

## IOS Commands Practiced

- `ip address`
- `no shutdown`
- `ip route`
- `show ip interface brief`
- `show ip route`
- `ping`
- `copy running-config startup-config`

---

## Key Concepts Learned

- Variable Length Subnet Masking (VLSM)
- IPv4 subnet planning
- Host address allocation
- Point-to-point addressing
- Static routing
- Route verification
- End-to-end connectivity testing

---

## Files Included

```
Day-15-IPv4-Subnetting-and-Static-Routing/
│
├── Day 15 Lab - VLSM.pkt
├── topology.png
└── readme.md
```

---

## Learning Outcome

After completing this lab, I can:

- Design an efficient VLSM addressing scheme for multiple LANs.
- Configure IPv4 addressing on routers and end devices.
- Configure static routes between multiple networks.
- Verify routing tables and interface configurations using Cisco IOS.
- Validate end-to-end communication across all subnets.

---

**Status:** ✅ Completed