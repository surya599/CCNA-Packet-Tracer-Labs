# Day 18 – VTP (VLAN Trunking Protocol) Lab

## 📌 Objective
This lab demonstrates the configuration and verification of **VTP Server, Client, and Transparent modes** along with **VLAN propagation**, **802.1Q trunking**, **DTP disablement**, and **access port configuration**.

---

## 🛠️ Topology

- **Switches:** 3 × Cisco 2960-24TT
- **PCs:** 9
- **VTP Domain:** `CCNA`

### VLANs

| VLAN | Network | Devices |
|------|---------|---------|
| VLAN 10 | 10.0.0.0/26 | PC1, PC2 |
| VLAN 20 | 10.0.0.64/26 | PC8, PC9 |
| VLAN 30 | 10.0.0.128/26 | PC6, PC7 |
| VLAN 40 | 10.0.0.192/26 | PC3, PC4 |
| VLAN 50 | Test VLAN | Used for VTP Client verification |

---

## 🎯 Lab Tasks

### 1. Configure Trunk Links

- Configure all switch-to-switch links as **802.1Q trunk ports**.
- Disable **Dynamic Trunking Protocol (DTP)** using:

```bash
switchport nonegotiate
```

- Verify:
  - Administrative Mode
  - Operational Mode
  - Encapsulation
  - Trunk Status

Commands:

```bash
show interfaces trunk
show interfaces switchport
```

---

### 2. Configure SW1 as VTP Server

- Set VTP Domain:

```bash
CCNA
```

- Configure SW1 in **Server Mode**.
- Create:
  - VLAN 10
  - VLAN 20
  - VLAN 30

Verify that VLANs are automatically learned by SW2 and SW3.

Commands:

```bash
show vtp status
show vlan brief
```

---

### 3. Configure SW2 as VTP Transparent

- Change SW2 to **Transparent Mode**.
- Create VLAN 40 on SW2.

Verify:

- VLAN40 exists only on SW2.
- VLAN40 is **NOT** added to SW1 or SW3.

Commands:

```bash
show vlan brief
show vtp status
```

---

### 4. Configure SW3 as VTP Client

- Configure SW3 in **Client Mode**.
- Attempt to create VLAN50.

Expected Result:

- VLAN creation should fail because VTP Clients cannot create, modify, or delete VLANs.

Verify:

```bash
show vtp status
show vlan brief
```

---

### 5. Configure Access Ports

Assign host-facing ports to their respective VLANs.

| Device | VLAN |
|--------|------|
| PC1, PC2 | VLAN10 |
| PC8, PC9 | VLAN20 |
| PC6, PC7 | VLAN30 |
| PC3, PC4 | VLAN40 |

Configure ports as:

```bash
switchport mode access
switchport access vlan <VLAN-ID>
```

Verify:

```bash
show vlan brief
show interfaces switchport
```

---

## ✅ Verification Checklist

- [ ] All inter-switch links are trunks.
- [ ] DTP is disabled on trunk links.
- [ ] SW1 operates as VTP Server.
- [ ] SW2 operates as VTP Transparent.
- [ ] SW3 operates as VTP Client.
- [ ] VLANs 10, 20, and 30 propagate from SW1.
- [ ] VLAN40 exists only on SW2.
- [ ] VLAN50 cannot be created on SW3.
- [ ] All access ports belong to the correct VLAN.

---

## 📚 Concepts Practiced

- VLAN Configuration
- IEEE 802.1Q Trunking
- Dynamic Trunking Protocol (DTP)
- VTP Server Mode
- VTP Client Mode
- VTP Transparent Mode
- VLAN Database Synchronization
- Access Port Configuration
- VLAN Verification Commands

---

## 📂 Files Included

- `Day 19 Lab - DTP & VTP.pkt` – Cisco Packet Tracer file
- `topology.png` – Cisco Packet Tracer topology
- `readme.md` – Lab documentation

---

## 🎓 Learning Outcome

After completing this lab, you will be able to:

- Configure and verify VTP Server, Client, and Transparent modes.
- Understand VLAN propagation using VTP.
- Configure secure trunk links by disabling DTP.
- Configure access ports for end devices.
- Verify VLAN and trunk configurations using Cisco IOS show commands.
- Understand the differences between VTP operating modes and their effects on VLAN management.