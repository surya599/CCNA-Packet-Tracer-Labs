# CCNA Packet Tracer Lab 11 – Static Route Troubleshooting

## Overview

This lab focuses on troubleshooting static routing in a multi-router network. Each router contains a single configuration error that prevents end-to-end communication between two LANs. The objective is to identify the misconfigurations, correct them, and restore full network connectivity.

---

## Objective

- Verify network connectivity.
- Identify routing and interface misconfigurations.
- Troubleshoot static routes using Cisco IOS verification commands.
- Correct configuration errors on each router.
- Restore successful communication between remote hosts.

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

- Verified the existing network configuration.
- Tested connectivity between PC1 and PC2.
- Used Cisco IOS verification commands to identify configuration errors.
- Located one misconfiguration on each router.
- Corrected the routing and interface configuration issues.
- Verified routing tables after applying the fixes.
- Confirmed successful end-to-end connectivity using ICMP ping.

---

## IOS Commands Practiced

- `show ip interface brief`
- `show ip route`
- `show running-config`
- `show ip protocols`
- `ping`
- `traceroute`
- `ip route`

---

## Key Concepts Learned

- Static route troubleshooting
- Routing table analysis
- Interface verification
- Network path verification
- ICMP-based connectivity testing
- Cisco IOS troubleshooting methodology
- Identifying and correcting routing misconfigurations

---

## Files Included

```
Day-12-Static-Route-Troubleshooting/
│
├── Day 11 Lab - Troubleshooting Static Routes.pkt
├── topology.png
└── readme.md
```

---

## Learning Outcome

After completing this lab, I can:

- Troubleshoot static routing issues using Cisco IOS.
- Analyze routing tables to identify configuration problems.
- Verify interface status and network reachability.
- Correct routing misconfigurations on Cisco routers.
- Validate successful end-to-end connectivity after troubleshooting.

---

**Status:** ✅ Completed