# CCNA Packet Tracer Lab 08 – Basic Device Configuration and Interface Settings

## Overview

This lab focuses on performing the initial configuration of Cisco routers and switches. The objective is to configure hostnames, assign IP addresses, manually configure interface speed and duplex settings, add interface descriptions, and administratively disable unused interfaces following common networking best practices.

---

## Objective

- Configure device hostnames.
- Assign IPv4 addresses to the router and PCs.
- Manually configure speed and duplex on inter-device links.
- Add descriptions to network interfaces.
- Disable unused switch and router interfaces.

---

## Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2911 Router | 1 |
| Cisco 2960 Switch | 2 |
| PCs | 4 |

---

## Network Addressing

| Network | Address |
|---------|----------|
| LAN | 172.16.0.0/16 |

---

## Tasks Performed

- Configured hostnames for R1, SW1, and SW2.
- Assigned the appropriate IPv4 addresses to the router and all PCs.
- Configured speed and duplex settings on interfaces connecting networking devices.
- Added meaningful descriptions to all active interfaces.
- Disabled all unused interfaces to improve network security and management.

---

## IOS Commands Practiced

- `hostname`
- `interface`
- `ip address`
- `no shutdown`
- `speed`
- `duplex`
- `description`
- `shutdown`
- `show ip interface brief`
- `show interfaces description`

---

## Key Concepts Learned

- Initial device configuration
- IPv4 interface configuration
- Manual speed and duplex configuration
- Interface documentation using descriptions
- Administrative interface shutdown
- Network hardening best practices
- Interface verification using Cisco IOS

---

## Files Included

```
Day-08-Basic-Device-Configuration/
│
├── Day 08 Lab - IPv4 Addresses.pkt
├── topology.png
└── readme.md
```

---

## Learning Outcome

After completing this lab, I can:

- Perform the initial configuration of Cisco routers and switches.
- Configure IPv4 addresses on network devices.
- Manually configure interface speed and duplex settings.
- Document interfaces using descriptions.
- Disable unused interfaces as a security best practice.
- Verify interface status and configuration using Cisco IOS commands.

---

**Status:** ✅ Completed