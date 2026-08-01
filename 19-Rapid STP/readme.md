# Day 21 Lab - Analyzing STP and RSTP Port Roles

## Objective
This lab focuses on analyzing **Spanning Tree Protocol (STP)** and **Rapid Spanning Tree Protocol (RSTP)** by identifying the root bridge, determining the port roles and states of each switch, and manually configuring the appropriate RSTP link types.

---

## Topology
- **Switches**
  - SW1 (Root Bridge)
  - SW2
  - SW3
  - SW4

- **End Devices**
  - PC1
  - PC2
  - PC3
  - PC4
  - PC5
  - PC6

- **Additional Devices**
  - Hub0
  - Hub1

---

## Tasks Completed

### 1. Identify the Root Bridge
- Verified the Root Bridge using the CLI.
- Confirmed that **SW1** is the Root Bridge.
- Examined the role and state of every interface on the root bridge.

---

### 2. Analyze Port Roles
Determined the port role and state of every interface on all switches.

#### Port Roles
- Root Port (RP)
- Designated Port (DP)
- Alternate Port (AP)
- Backup Port (BP)

#### Port States
- Forwarding
- Discarding

Verified all results using:

```bash
show spanning-tree
```

---

### 3. Configure RSTP Link Types

Configured the appropriate link type for each interface.

Examples:
- Point-to-Point
- Shared Link

Special attention was given to:

- SW1 F0/24 (connected to Hub0)

Since the interface connects to a **Hub**, the correct RSTP link type is:

**Shared**

---

## Verification Commands

Display spanning-tree information

```bash
show spanning-tree
```

Display interface status

```bash
show interfaces status
```

Display spanning-tree details

```bash
show spanning-tree detail
```

Display spanning-tree interface information

```bash
show spanning-tree interface f0/24 detail
```

Display running configuration

```bash
show running-config
```

---

## Key Concepts Learned

- Root Bridge election process
- Bridge ID comparison
- Port Roles in RSTP
- Port States in RSTP
- Difference between Root, Designated, Alternate, and Backup ports
- Shared vs Point-to-Point link types
- Effect of hubs on STP/RSTP topology
- CLI verification of spanning-tree information

---

## Outcome

Successfully:
- Identified the Root Bridge.
- Determined the role and state of every switch interface.
- Verified the topology using Cisco IOS commands.
- Configured the correct RSTP link types.
- Understood how hubs influence RSTP by creating shared links and backup ports.

---

## Technologies Used

- Cisco Packet Tracer
- Cisco Catalyst 2960 Switches
- Spanning Tree Protocol (STP)
- Rapid Spanning Tree Protocol (RSTP)
- Cisco IOS CLI

---

## Skills Practiced

- STP Troubleshooting
- RSTP Analysis
- Root Bridge Identification
- Port Role Verification
- CLI Verification
- Layer 2 Switching
- Network Redundancy
- Loop Prevention