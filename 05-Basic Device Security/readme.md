# CCNA Packet Tracer Lab 06 – Ethernet Switching and MAC Address Learning

## Overview

This lab explores how Ethernet switches learn MAC addresses and forward frames within a LAN. Using Cisco Packet Tracer's Simulation Mode, the behavior of ARP requests, ICMP messages, and MAC address table learning is analyzed.

---

## Objective

- Observe frame forwarding when switch MAC address tables are empty.
- Analyze ARP and ICMP traffic using Simulation Mode.
- Understand how switches dynamically learn MAC addresses.
- Verify learned MAC addresses using IOS commands.
- Clear dynamically learned MAC addresses from the switches.

---

## Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2960 Switch | 2 |
| PCs | 4 |

---

## Network Addressing

| Network | Address |
|---------|----------|
| LAN | 192.168.1.0/24 |

---

## Tasks Performed

- Built the topology with two interconnected switches and four PCs.
- Verified that both switches had empty MAC address tables.
- Verified that all PCs had empty ARP tables.
- Sent ICMP traffic between hosts and analyzed packet flow in Simulation Mode.
- Observed ARP broadcasts and ICMP echo request/reply messages.
- Allowed the switches to dynamically learn the MAC addresses of connected devices.
- Verified learned MAC addresses using switch IOS commands.
- Cleared the dynamic MAC address table entries.

---

## IOS Commands Practiced

- `show mac address-table`
- `clear mac address-table dynamic`

---

## Protocols Observed

- Ethernet
- ARP
- IPv4
- ICMP

---

## Key Concepts Learned

- Ethernet frame forwarding
- MAC address learning
- Unknown unicast flooding
- Broadcast domains
- ARP request and ARP reply
- Dynamic MAC address table
- Switch forwarding behavior
- Packet analysis using Simulation Mode

---

## Files Included

```
Day-06-Ethernet-Switching-and-MAC-Learning/
│
├── Day 06 Lab - Ethernet LAN Switching.pkt
├── topology.png
└── readme.md
```

---

## Learning Outcome

After completing this lab, I can:

- Explain how Ethernet switches build MAC address tables.
- Analyze ARP and ICMP traffic using Packet Tracer's Simulation Mode.
- Verify learned MAC addresses using Cisco IOS commands.
- Understand how switches handle unknown destinations and broadcasts.
- Clear and rebuild dynamic MAC address tables for troubleshooting.

---

**Status:** ✅ Completed