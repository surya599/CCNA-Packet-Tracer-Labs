# CCNA Packet Tracer Lab 09 – Router Interface Configuration and Connectivity Verification

## Overview

This lab focuses on configuring IPv4 addresses on multiple router interfaces, enabling the interfaces, assigning interface descriptions, configuring end-device IP addresses, and verifying end-to-end connectivity between different networks.

---

## Objective

- Configure the router hostname.
- Examine interface information using Cisco IOS show commands.
- Configure IPv4 addresses on router interfaces.
- Enable router interfaces.
- Configure interface descriptions.
- Configure IPv4 addresses on end devices.
- Verify connectivity using ICMP.

---

## Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2911 Router | 1 |
| Cisco 2960 Switch | 3 |
| PCs | 3 |

---

## Network Addressing

| Network | Address |
|---------|----------|
| LAN 1 | 15.0.0.0/8 |
| LAN 2 | 182.98.0.0/16 |
| LAN 3 | 201.191.20.0/24 |

---

## Tasks Performed

- Configured the router hostname as **R1**.
- Used Cisco IOS show commands to inspect interface status and IP information.
- Assigned IPv4 addresses to all router interfaces.
- Enabled all active router interfaces using the `no shutdown` command.
- Configured interface descriptions for improved documentation.
- Verified interface configuration using Cisco IOS.
- Configured IPv4 addresses on all PCs.
- Tested end-to-end connectivity between the three LANs using ICMP ping.

---

## IOS Commands Practiced

- `hostname`
- `show ip interface brief`
- `interface`
- `ip address`
- `description`
- `no shutdown`
- `show running-config`
- `copy running-config startup-config`

---

## Key Concepts Learned

- Router interface configuration
- IPv4 addressing
- Interface administration
- Interface descriptions
- Cisco IOS verification commands
- End-to-end network connectivity
- ICMP testing
- Configuration management

---

## Files Included

```
Day-09-Router-Interface-Configuration/
│
├── Day 09 Lab - Interface Configuration.pkt
├── topology.png
└── readme.md
```

---

## Learning Outcome

After completing this lab, I can:

- Configure IPv4 addresses on multiple router interfaces.
- Enable and verify router interfaces using Cisco IOS.
- Document interfaces with meaningful descriptions.
- Configure IPv4 addresses on end devices.
- Verify network connectivity using ping.
- Save and validate router configurations.

---

**Status:** ✅ Completed