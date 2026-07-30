# Day 21 - STP Path Cost, Port Priority, PortFast & BPDU Guard

## 📌 Objective
This lab explores how to influence the Spanning Tree Protocol (STP) topology by modifying interface path cost and port priority. It also demonstrates the use of PortFast and BPDU Guard on access ports.

---

## 🛠️ Technologies Used
- Cisco Packet Tracer
- Cisco Catalyst 2960 Switches
- Spanning Tree Protocol (PVST+)
- VLANs
- PortFast
- BPDU Guard

---

## 🖥️ Devices Used
- 4 × Cisco 2960-24TT Switches (SW1, SW2, SW3, SW4)
- 2 × PCs
- VLAN 1
- VLAN 2

---

## 📚 Lab Tasks

### 1. Verify the Existing STP Topology
- Identify the current root bridge.
- Check the STP role and state of every port on each switch.

Commands used:
```bash
show spanning-tree
show spanning-tree vlan 1
show spanning-tree vlan 2
```

---

### 2. Configure Root Bridges

Configure:

- SW1 as the Primary Root Bridge for VLAN 1
- SW1 as the Secondary Root Bridge for VLAN 2
- SW2 as the Primary Root Bridge for VLAN 2
- SW2 as the Secondary Root Bridge for VLAN 1

Commands:

```bash
spanning-tree vlan 1 root primary
spanning-tree vlan 2 root secondary

spanning-tree vlan 2 root primary
spanning-tree vlan 1 root secondary
```

Verify:

```bash
show spanning-tree
```

---

### 3. Modify STP Path Cost

Increase the VLAN 1 path cost on SW4 FastEthernet0/2.

Configuration:

```bash
interface f0/2
spanning-tree vlan 1 cost 100
```

Verify:

```bash
show spanning-tree vlan 1
show spanning-tree interface f0/2
```

**Observation**

- The root port changes only if the new path becomes less preferred than another available path.
- If the alternate path still has a higher total root path cost, the root port remains unchanged.

---

### 4. Modify Port Priority

Increase the VLAN 1 port priority of SW1 FastEthernet0/1.

Configuration:

```bash
interface f0/1
spanning-tree vlan 1 port-priority 240
```

Verify:

```bash
show spanning-tree vlan 1
```

**Observation**

- Port priority is used only when STP must choose between equal-cost paths.
- If no tie exists, changing the port priority does not affect the topology.

---

### 5. Configure PortFast and BPDU Guard

Enable PortFast and BPDU Guard on the access ports connected to PCs.

Configuration:

```bash
interface f0/3
spanning-tree portfast
spanning-tree bpduguard enable
```

Verify:

```bash
show spanning-tree interface f0/3 detail
show spanning-tree summary
```

---

## ✅ Key Concepts Learned

- Root Bridge Election
- Root Port Selection
- Designated Port Election
- STP Path Cost
- Port Priority
- PortFast
- BPDU Guard
- STP Topology Verification
- PVST+ Operation

---

## 📂 Files Included

- `Day 21 Lab - Configuring Spanning Tree.pkt`
- `topology.png`
- `readme.md`

---

## 🎯 Outcome

Successfully manipulated the STP topology by configuring different root bridges, modifying path costs and port priorities, and securing access ports using PortFast and BPDU Guard while verifying all changes through STP show commands.