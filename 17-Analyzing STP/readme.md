# Day 20 - Analyzing STP (Spanning Tree Protocol)

## 📌 Objective
The objective of this lab is to analyze how **Spanning Tree Protocol (STP)** prevents Layer 2 switching loops by electing a **Root Bridge** and assigning port roles such as **Root Port (RP)**, **Designated Port (DP)**, and **Non-Designated (Blocking) Port (NDP)**.

---

## 🛠️ Lab Topology

- 4 Cisco 2960 Switches
- Multiple redundant links between switches
- STP enabled (default PVST+)
- Link lights disabled for easier analysis

---

## 🎯 Tasks Performed

- Identified the Root Bridge using Bridge ID (Priority + MAC Address)
- Compared Bridge IDs of all switches
- Calculated Root Path Cost for each switch
- Determined Root Ports on non-root switches
- Determined Designated Ports on each network segment
- Identified Non-Designated (Blocking) Ports to eliminate loops
- Verified STP calculations using Cisco CLI

---

## 📖 STP Concepts Covered

- Root Bridge Election
- Bridge ID (Priority + MAC Address)
- Root Path Cost
- Root Port (RP)
- Designated Port (DP)
- Non-Designated Port (NDP)
- STP Port States
- Loop Prevention
- Path Cost Comparison
- Tie-Breaking Rules

---

## 🔍 Verification Commands

```bash
show spanning-tree

show spanning-tree vlan 1

show spanning-tree root

show spanning-tree interface fa0/1

show spanning-tree summary
```

---

## 🧠 Key Learning Outcomes

- Learned how STP elects the Root Bridge.
- Understood how Bridge Priority affects Root Bridge selection.
- Learned how Root Path Cost is calculated.
- Identified Root Ports and Designated Ports on each switch.
- Determined which redundant ports are placed into the Blocking (Non-Designated) state.
- Understood how STP prevents Layer 2 switching loops without requiring manual intervention.
- Verified STP decisions using Cisco IOS commands.

---

## 📂 Files Included

- `Day 20 - Analyzing STP.pkt`
- `topology.png`
- `README.md`

---

## 🚀 Skills Practiced

- Cisco Packet Tracer
- STP Analysis
- Root Bridge Identification
- Port Role Identification
- CLI Verification
- Layer 2 Redundancy
- Switching Fundamentals
- Network Troubleshooting

---

## 📚 Lab Outcome

Successfully analyzed an STP topology by identifying the Root Bridge, determining the role of every switch port, understanding why specific ports were blocked, and verifying all results using Cisco IOS CLI commands.