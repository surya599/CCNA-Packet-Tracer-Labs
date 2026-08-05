# Day 26 - OSPF Single Area Configuration and Troubleshooting

## Overview

This lab focused on configuring a Single Area OSPF network across four routers and troubleshooting OSPF routing issues. The network consisted of four routers interconnected using point-to-point links, with loopback interfaces configured as Router IDs. During the lab, OSPF neighbor relationships were established, LSAs were exchanged, and routing problems were diagnosed using various verification commands. The issue was ultimately resolved by reconfiguring Router R3.

---

## Objectives

- Configure IPv4 addressing on router interfaces.
- Configure Loopback interfaces for Router IDs.
- Configure OSPF Process 1 in Area 0.
- Configure passive interfaces for Loopback and LAN interfaces.
- Verify OSPF neighbor adjacencies.
- Analyze the OSPF Link-State Database (LSDB).
- Troubleshoot missing OSPF routes.
- Verify successful route propagation after troubleshooting.

---

## Network Topology

```
                 10.0.12.0/30
          +-----------------------+
          |                       |
       R1 |                       | R2
     1.1.1.1                  2.2.2.2
          |                       |
10.0.13.0 |                       | 10.0.24.0
          |                       |
       R3 |-----------------------| R4
     3.3.3.3      10.0.34.0     4.4.4.4
```

---

## IP Addressing

| Device | Interface | IP Address | Subnet Mask |
|---------|-----------|------------|-------------|
| R1 | Fa1/0 | 10.0.13.1 | 255.255.255.252 |
| R1 | Fa2/0 | 10.0.12.1 | 255.255.255.252 |
| R1 | Lo0 | 1.1.1.1 | 255.255.255.255 |
| R2 | Fa1/0 | 10.0.12.2 | 255.255.255.252 |
| R2 | Fa2/0 | 10.0.24.1 | 255.255.255.252 |
| R2 | Lo0 | 2.2.2.2 | 255.255.255.255 |
| R3 | Fa1/0 | 10.0.13.2 | 255.255.255.252 |
| R3 | Fa2/0 | 10.0.34.1 | 255.255.255.252 |
| R3 | Lo0 | 3.3.3.3 | 255.255.255.255 |
| R4 | Fa1/0 | 10.0.24.2 | 255.255.255.252 |
| R4 | Fa2/0 | 10.0.34.2 | 255.255.255.252 |
| R4 | Lo0 | 4.4.4.4 | 255.255.255.255 |

---

## Configuration Summary

### Router Configuration

- Assigned IP addresses to all FastEthernet interfaces.
- Configured Loopback0 interfaces.
- Enabled interfaces using `no shutdown`.
- Configured OSPF Process ID 1.
- Advertised all required networks into Area 0.
- Configured passive interfaces for Loopback and LAN interfaces.

---

## Verification Commands

```bash
show ip interface brief
show ip route
show ip ospf neighbor
show ip ospf database
show ip ospf interface
show ip protocols
show running-config | section router ospf
```

---

## Troubleshooting Performed

During verification, Router R3 formed FULL OSPF neighbor adjacencies with both R1 and R4, but the routing table contained only connected routes.

The following troubleshooting steps were performed:

- Verified interface status.
- Verified OSPF neighbor adjacency.
- Verified OSPF network statements.
- Verified passive interface configuration.
- Examined the OSPF Link-State Database.
- Compared Router LSAs from all routers.
- Verified routing information sources.
- Reconfigured OSPF network statements.
- Verified Router IDs and Loopback advertisements.
- Reconfigured Router R3.

After reconfiguring Router R3, OSPF recalculated the topology successfully and all expected OSPF routes were installed in the routing table.

---

## Key OSPF Concepts Practiced

- Single Area OSPF
- Router ID selection
- Loopback Interfaces
- OSPF Neighbor States
- DR/BDR Election
- Link-State Advertisements (LSAs)
- Link-State Database (LSDB)
- SPF Calculation
- Passive Interfaces
- OSPF Route Verification
- OSPF Troubleshooting

---

## Commands Learned

### Configure OSPF

```bash
router ospf 1
network <network> <wildcard-mask> area 0
passive-interface Loopback0
```

### Verification

```bash
show ip ospf neighbor
show ip ospf database
show ip ospf interface
show ip protocols
show ip route
```

### Troubleshooting

```bash
clear ip ospf process
show running-config | section router ospf
```

---

## Skills Gained

- Configured Single Area OSPF.
- Configured Router IDs using Loopback interfaces.
- Established OSPF neighbor adjacencies.
- Verified OSPF operation.
- Interpreted the OSPF Link-State Database.
- Diagnosed routing issues using Cisco IOS commands.
- Resolved an OSPF routing issue through systematic troubleshooting and router reconfiguration.

---

## Outcome

Successfully configured and verified a four-router Single Area OSPF network. All routers established OSPF neighbor relationships, exchanged routing information, and learned remote routes successfully after troubleshooting and reconfiguring Router R3.