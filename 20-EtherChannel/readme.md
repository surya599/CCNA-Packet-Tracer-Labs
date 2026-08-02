
## 📖 Overview

This lab demonstrates the configuration and verification of Layer 2 and Layer 3 EtherChannels using Cisco Catalyst switches. It covers the implementation of **LACP**, **PAgP**, and **Static EtherChannel**, along with Layer 3 routing between two LANs. The lab also explores EtherChannel load-balancing methods and verifies end-to-end connectivity.

---

## 🎯 Objectives

- Configure a **Layer 2 EtherChannel** between ASW1 and DSW1 using **LACP**.
- Configure a **Layer 2 EtherChannel** between ASW2 and DSW2 using **PAgP**.
- Configure a **Layer 3 EtherChannel** between DSW1 and DSW2 using **Static EtherChannel**.
- Configure static routes to enable communication between both LANs.
- Verify the default EtherChannel load-balancing algorithm.
- Change the load-balancing method to use **source and destination IP addresses**.
- Verify successful connectivity between the PCs and the server.

---

## 🖥️ Network Topology

- **ASW1 ↔ DSW1**
  - Layer 2 EtherChannel
  - **LACP**
  - Trunk Link

- **ASW2 ↔ DSW2**
  - Layer 2 EtherChannel
  - **PAgP**
  - Trunk Link

- **DSW1 ↔ DSW2**
  - Layer 3 EtherChannel
  - **Static EtherChannel (Mode On)**
  - Routed Port-Channel

---

## 🌐 IP Addressing

| Device | IP Address |
|---------|------------|
| PC1 | 172.16.1.1/24 |
| PC2 | 172.16.1.2/24 |
| DSW1 VLAN 1 SVI | 172.16.1.254/24 |
| SRV1 | 172.16.2.1/24 |
| DSW2 VLAN 1 SVI | 172.16.2.254/24 |
| Layer 3 EtherChannel | 10.0.0.0/30 |

---

## 🛠️ Tasks Completed

### 1. Configure Layer 2 EtherChannel (LACP)

Configured an EtherChannel between **ASW1** and **DSW1** using **LACP** and configured the Port-Channel as an IEEE 802.1Q trunk.

---

### 2. Configure Layer 2 EtherChannel (PAgP)

Configured an EtherChannel between **ASW2** and **DSW2** using **PAgP** and configured the Port-Channel as an IEEE 802.1Q trunk.

---

### 3. Configure Layer 3 EtherChannel

Configured a routed EtherChannel between **DSW1** and **DSW2** using **Static EtherChannel (Mode On)**.

- Converted the Port-Channel into a Layer 3 interface.
- Assigned IP addresses from the **10.0.0.0/30** network.

---

### 4. Configure Static Routing

Configured static routes on both Layer 3 switches to allow communication between:

- **172.16.1.0/24**
- **172.16.2.0/24**

---

### 5. Verify Default Load Balancing

Verified the default EtherChannel load-balancing algorithm configured on each switch.

---

### 6. Configure IP-Based Load Balancing

Modified the EtherChannel hashing algorithm to use:

- Source IP Address
- Destination IP Address

Verified the updated load-balancing configuration.

---

## ✅ Verification Commands

```bash
show etherchannel summary
show etherchannel port-channel
show etherchannel load-balance
show interfaces trunk
show interfaces port-channel
show interfaces port-channel brief
show ip interface brief
show ip route
show running-config
ping
```

---

## 📚 Technologies Practiced

- EtherChannel
- LACP (IEEE 802.3ad)
- PAgP (Cisco Proprietary)
- Static EtherChannel
- Layer 2 Switching
- Layer 3 Switching
- Routed Port-Channel
- VLAN Trunking
- Static Routing
- EtherChannel Load Balancing

---

## 🎓 Skills Gained

By completing this lab, I learned how to:

- Configure Layer 2 EtherChannels using LACP.
- Configure Layer 2 EtherChannels using PAgP.
- Configure Layer 3 EtherChannels using Static EtherChannel.
- Configure and verify trunk links over Port-Channels.
- Configure routed Port-Channels between Layer 3 switches.
- Configure static routes for inter-network communication.
- Verify EtherChannel status and member interfaces.
- Understand EtherChannel load-balancing mechanisms.
- Modify the hashing algorithm for traffic distribution.
- Troubleshoot and verify end-to-end network connectivity.

---

## 📁 Lab File

**Day 23 Lab - EtherChannel.pkt**
