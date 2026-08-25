# VLAN & Router-on-a-Stick (ROAS) Lab

## Objective

The objective of this lab is to configure VLANs on a Cisco Layer 2 switch and implement **Router-on-a-Stick (ROAS)** to enable communication between different VLANs.

The lab also demonstrates how VLAN tagging works for both data and voice traffic in Cisco Packet Tracer.

---

## Topology

### Devices Used

* 1 × Cisco 3650-24PS Switch — **SW1**
* 1 × Cisco 2811 Router — **R1**
* 2 × PCs — **PC1, PC2**
* 2 × IP Phones — **PH1, PH2**

### Connections

| Device | Interface   | Connected To | Interface |
| ------ | ----------- | ------------ | --------- |
| PC1    | NIC         | PH1          | PC Port   |
| PH1    | Switch Port | SW1          | G1/0/2    |
| PC2    | NIC         | PH2          | PC Port   |
| PH2    | Switch Port | SW1          | G1/0/3    |
| SW1    | G1/0/1      | R1           | F0/0      |

---

## VLAN Information

| VLAN    | Network         | Purpose    |
| ------- | --------------- | ---------- |
| VLAN 10 | 192.168.10.0/24 | Data VLAN  |
| VLAN 20 | 192.168.20.0/24 | Voice VLAN |

### IP Addressing

| Device             | IP Address    |
| ------------------ | ------------- |
| PC1                | 192.168.10.11 |
| PC2                | 192.168.20.12 |
| R1 VLAN 10 Gateway | 192.168.10.1  |
| R1 VLAN 20 Gateway | 192.168.20.1  |

---

## Tasks

### 1. Configure SW1 Interfaces

Configure the switch interfaces in the appropriate VLANs.

* **G1/0/2** → VLAN 10
* **G1/0/3** → VLAN 20
* **G1/0/1** → Trunk link to R1

Verify the VLAN configuration using:

```bash
show vlan brief
```

Verify the trunk using:

```bash
show interfaces trunk
```

---

## 2. Configure Router-on-a-Stick

Configure **R1 F0/0** as the physical interface connected to SW1.

Create subinterfaces for each VLAN:

```bash
interface fastethernet 0/0
 no shutdown

interface fastethernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface fastethernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

These subinterfaces act as the default gateways for the respective VLANs.

Verify using:

```bash
show ip interface brief
```

---

## 3. Test Connectivity

Switch to **Simulation Mode** in Packet Tracer.

From **PC1**, ping **PC2**:

```bash
ping 192.168.20.12
```

Observe the packet as it travels:

**PC1 → PH1 → SW1 → R1 → SW1 → PH2 → PC2**

### Question

**Is the traffic tagged with a VLAN ID?**

On the trunk link between SW1 and R1, the Ethernet frames are tagged using **802.1Q VLAN tagging**.

The router receives the tagged frame on F0/0 and uses the appropriate subinterface to route the packet between VLANs.

---

## 4. Test Voice Traffic

In Simulation Mode, initiate a call from **PH2 to PH1**.

Observe the packet as it crosses the network.

### Question

**Is the traffic tagged with a VLAN ID?**

Voice traffic is associated with the configured **voice VLAN** and is carried across the trunk using VLAN tagging.

> Note: The telephone configurations on R1 are pre-configured and are not the main focus of this CCNA lab.

---

## Verification Commands

### Check VLANs

```bash
show vlan brief
```

### Check Trunk

```bash
show interfaces trunk
```

### Check Router Interfaces

```bash
show ip interface brief
```

### Check Routing Table

```bash
show ip route
```

### Test Connectivity

```bash
ping 192.168.20.12
```

---

## Expected Results

* VLAN 10 and VLAN 20 are successfully created.
* SW1 interfaces are assigned to the appropriate VLANs.
* The SW1–R1 link operates as a trunk.
* R1 uses subinterfaces for VLAN 10 and VLAN 20.
* PC1 can communicate with PC2 through the router.
* Inter-VLAN routing works successfully.
* VLAN tagging can be observed on the trunk link in Simulation Mode.
* Voice traffic from PH2 to PH1 can be observed using the configured voice VLAN.

---

## Key Concepts Learned

* VLAN configuration
* Access ports
* Trunk ports
* 802.1Q VLAN tagging
* Inter-VLAN routing
* Router-on-a-Stick (ROAS)
* Router subinterfaces
* Data VLAN vs Voice VLAN
* Packet Tracer Simulation Mode
* VLAN tag identification

---

## Conclusion

This lab demonstrates how a **Layer 2 switch and router can work together to provide communication between separate VLANs**. Router-on-a-Stick allows a single physical router interface to handle multiple VLANs through 802.1Q-tagged trunk traffic and router subinterfaces.

The lab also provides practical observation of **VLAN tagging** during both data and voice communication.
