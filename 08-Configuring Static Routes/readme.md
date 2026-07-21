# CCNA Packet Tracer Lab 11 – Static Routing Configuration

## Overview

This lab focuses on configuring static routes to enable communication between remote networks. The objective is to configure router interfaces, assign IPv4 addresses to end devices, configure default gateways, and implement static routing to achieve end-to-end connectivity.

---

## Objective

- Configure router hostnames.
- Configure IPv4 addresses on router interfaces.
- Configure IPv4 addressing on PCs.
- Configure default gateways on end devices.
- Configure static routes on each router.
- Verify connectivity between remote networks.

---

## Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2911 Router | 3 |
| Cisco 2960 Switch | 2 |
| PCs | 2 |

---

## Network Addressing

| Network | Address |
|---------|----------|
| LAN 1 | 192.168.1.0/24 |
| R1 ↔ R2 | 192.168.12.0/24 |
| R2 ↔ R3 | 192.168.13.0/24 |
| LAN 2 | 192.168.3.0/24 |

---

## Tasks Performed

- Configured hostnames for R1, R2, and R3.
- Assigned IPv4 addresses to all router interfaces.
- Configured IPv4 addresses and default gateways on both PCs.
- Verified interface status and connectivity between directly connected networks.
- Configured static routes on each router to reach remote networks.
- Verified routing tables using Cisco IOS commands.
- Tested end-to-end connectivity between PC1 and PC2 using ICMP ping.

---

## IOS Commands Practiced

- `hostname`
- `interface`
- `ip address`
- `no shutdown`
- `ip route`
- `show ip interface brief`
- `show ip route`
- `show running-config`
- `copy running-config startup-config`

---

## Key Concepts Learned

- Static routing
- Remote network reachability
- Next-hop routing
- Router forwarding decisions
- Routing table verification
- Default gateway configuration
- End-to-end connectivity testing
- Cisco IOS route management

---

## Files Included

```
Day-11-Static-Routing/
│
├── Day 11 Lab - Configuring Static Routes (1).pkt
├── topology.png
└── readme.md
```

---

## Learning Outcome

After completing this lab, I can:

- Configure static routes between multiple routers.
- Configure IPv4 addressing on routers and end devices.
- Configure default gateways for hosts.
- Verify routing tables using Cisco IOS.
- Troubleshoot and validate end-to-end connectivity across multiple networks.
- Save and manage router configurations.

---

**Status:** ✅ Completed