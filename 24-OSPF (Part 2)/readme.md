# OSPF Default Route Advertisement Lab

## Lab Overview

This lab demonstrates how to configure a multi-router OSPF network, assign loopback interfaces for stable Router IDs, adjust OSPF reference bandwidth, and advertise a default route from an Autonomous System Boundary Router (ASBR).

---

## Objectives

- Configure hostnames and IP addresses on all routers.
- Configure Loopback interfaces.
- Configure single-area OSPF using interface configuration.
- Configure passive interfaces where appropriate.
- Modify the OSPF reference bandwidth.
- Configure R1 as an ASBR.
- Advertise a default route into the OSPF domain.
- Verify routing tables and OSPF operation.
- Observe OSPF Hello packets using Simulation Mode.

---

## Network Topology

- **R1** connects to ISP1 and the internal OSPF network.
- **R2, R3, and R4** participate in OSPF Area 0.
- **R1** injects the default route into OSPF.
- **R4** provides LAN connectivity to PC1.

---

## Addressing

| Device | Interface | IP Address |
|---------|-----------|------------|
| R1 | G3/0 | 203.0.113.1/30 |
| ISP1 | Interface | 203.0.113.2/30 |
| R1 | G0/0 | 10.0.12.1/30 |
| R2 | G0/0 | 10.0.12.2/30 |
| R1 | F1/0 | 10.0.13.1/30 |
| R3 | F1/0 | 10.0.13.2/30 |
| R2 | F1/0 | 10.0.24.1/30 |
| R4 | F1/0 | 10.0.24.2/30 |
| R3 | F2/0 | 10.0.34.1/30 |
| R4 | F2/0 | 10.0.34.2/30 |
| R4 | G0/0 | 192.168.4.254/24 |
| PC1 | NIC | 192.168.4.1/24 |

---

## Loopback Interfaces

| Router | Loopback |
|---------|----------|
| R1 | 1.1.1.1/32 |
| R2 | 2.2.2.2/32 |
| R3 | 3.3.3.3/32 |
| R4 | 4.4.4.4/32 |

---

## Configuration Tasks

### 1. Configure Hostnames

Assign the appropriate hostname to each router.

---

### 2. Configure Interfaces

- Assign all IP addresses.
- Enable interfaces using:

```
no shutdown
```

---

### 3. Configure Loopback Interfaces

Example (R1):

```
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
```

Repeat for the remaining routers.

---

### 4. Configure OSPF

Enable OSPF directly on each interface.

Example:

```
ip ospf 1 area 0
```

Configure LAN interfaces as passive.

Example:

```
router ospf 1
 passive-interface GigabitEthernet0/0
```

(Configure according to your topology.)

---

### 5. Configure OSPF Reference Bandwidth

On every router:

```
router ospf 1
 auto-cost reference-bandwidth 100
```

This makes FastEthernet interfaces have a cost of 100.

---

### 6. Configure R1 as an ASBR

Configure the default route:

```
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

Advertise it into OSPF:

```
router ospf 1
 default-information originate
```

---

## Verification

### Verify OSPF Neighbors

```
show ip ospf neighbor
```

---

### Verify OSPF Interfaces

```
show ip ospf interface brief
```

---

### Verify Routing Table

```
show ip route
```

Expected observations:

- All routers learn every network.
- R4 receives an OSPF external default route.

Example:

```
O*E2 0.0.0.0/0
```

---

### Verify Loopbacks

```
show ip interface brief
```

Loopback interfaces should be in the **up/up** state.

---

### Verify Default Route Advertisement

```
show ip route
```

On R4, verify:

```
O*E2 0.0.0.0/0
```

---

## OSPF Hello Packet Fields

Using Packet Tracer Simulation Mode, observe the Hello packets.

Typical fields include:

- Router ID
- Area ID
- Hello Interval
- Dead Interval
- Network Mask
- Designated Router (DR)
- Backup Designated Router (BDR)
- Neighbor List
- Authentication Type
- Options
- Priority

---

## Commands Used

```
hostname
interface
ip address
no shutdown
interface loopback0
ip ospf 1 area 0
router ospf 1
passive-interface
auto-cost reference-bandwidth 100
ip route
default-information originate
show ip ospf neighbor
show ip ospf interface brief
show ip route
show ip interface brief
show running-config
```

---

## Skills Practiced

- Loopback configuration
- Single-area OSPF
- Interface-based OSPF configuration
- Passive interfaces
- OSPF reference bandwidth
- ASBR configuration
- Default route origination
- Route verification
- OSPF Hello packet analysis
- Routing table interpretation

---

## Result

Successfully configured a multi-router OSPF Area 0 topology with loopback interfaces, customized OSPF reference bandwidth, configured R1 as an ASBR, advertised a default route throughout the OSPF domain, verified routing information, and analyzed OSPF Hello messages using Packet Tracer Simulation Mode.