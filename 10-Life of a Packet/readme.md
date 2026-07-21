# CCNA Packet Tracer Lab 12 – MAC Address Analysis Across Routed Networks

## Overview

This lab focuses on analyzing how Ethernet frames are forwarded across multiple routed networks. Using Cisco Packet Tracer's Simulation Mode, the source and destination MAC addresses are examined at each hop while observing that the IP addresses remain unchanged from source to destination.

---

## Objective

- Analyze source and destination MAC addresses across multiple network segments.
- Observe how routers rewrite Layer 2 headers.
- Verify MAC addresses using Cisco IOS commands.
- Compare Layer 2 and Layer 3 addressing during packet forwarding.
- Use Simulation Mode to inspect packet flow.

---

## Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2911 Router | 3 |
| Cisco 2960 Switch | 2 |
| PCs | 6 |

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

- Built the topology and verified end-to-end connectivity.
- Generated ICMP traffic between hosts.
- Traced packets using Packet Tracer Simulation Mode.
- Identified the source and destination MAC addresses at each network segment.
- Verified router interface MAC addresses using Cisco IOS commands.
- Compared Layer 2 frame addressing with Layer 3 IP addressing throughout the communication path.

---

## IOS Commands Practiced

- `show interfaces`
- `show arp`
- `show ip arp`
- `show mac address-table`
- `ping`

---

## Key Concepts Learned

- Ethernet frame forwarding
- MAC address rewriting by routers
- Layer 2 encapsulation
- ARP operation
- Layer 2 vs. Layer 3 addressing
- Packet forwarding across routed networks
- Simulation Mode analysis

---

## Files Included

```
Day-12-MAC-Address-Analysis/
│
├── Day 12 Lab - Life of a Packet.pkt
├── topology.png
└── readme.md
```

---

## Learning Outcome

After completing this lab, I can:

- Explain how MAC addresses change at every router hop.
- Differentiate between Layer 2 and Layer 3 addressing.
- Verify MAC addresses using Cisco IOS commands.
- Analyze Ethernet frames using Packet Tracer Simulation Mode.
- Trace packets across multiple routed networks.

---

**Status:** ✅ Completed