

## Objective

The objective of this lab is to configure and verify **Floating Static Routes** as backup routes in an OSPF network. When the primary OSPF path fails, the floating static routes should automatically enter the routing table and maintain connectivity between Enterprise networks through the ISP.

---

## Topology

- **Enterprise A**
  - R1
  - R2
  - SW1
  - SW2
  - PC1
  - SRV1

- **ISP Network**
  - SPR1
  - SPR2
  - ISPBR1
  - ISPBR2

---

## IP Addressing

| Device | Interface | IP Address |
|---------|-----------|------------|
| R1 | G0/0/0 | 203.0.113.2/30 |
| R1 | G0/1 | 10.0.1.254/24 |
| R1 | G0/2/0 | 10.0.0.1/30 |
| R2 | G0/0/0 | 203.0.113.6/30 |
| R2 | G0/1 | 10.0.2.254/24 |
| R2 | G0/2/0 | 10.0.0.2/30 |
| SPR1 | G0/0/0 | 203.0.113.1/30 |
| SPR1 | G0/1/0 | 192.168.1.1/30 |
| SPR2 | G0/0/0 | 203.0.113.5/30 |
| SPR2 | G0/1/0 | 192.168.1.2/30 |
| ISPBR1 | - | 203.0.113.9/30 |
| ISPBR2 | - | 203.0.113.13/30 |
| PC1 | NIC | 10.0.1.1/24 |
| SRV1 | NIC | 10.0.2.1/24 |

---

## Tasks Performed

### 1. Verified Existing Routing

- Checked routing tables on R1 and R2.
- Verified that OSPF was the active routing protocol inside Enterprise A.
- Confirmed:
  - PC1 reached SRV1 using the OSPF route.
  - Internet traffic used the default static route.

---

### 2. Configured Floating Static Routes

Configured backup static routes with an administrative distance higher than OSPF (110).

**On R1**

```bash
ip route 10.0.2.0 255.255.255.0 203.0.113.1 111
```

**On R2**

```bash
ip route 10.0.1.0 255.255.255.0 203.0.113.5 111
```

Since OSPF has an Administrative Distance of **110**, these routes remained inactive while OSPF was operational.

---

### 3. Simulated Link Failure

Disabled the OSPF link between R1 and R2.

```bash
interface g0/2/0
shutdown
```

Observed:

- OSPF neighbor adjacency went down.
- OSPF routes were removed.
- Floating static routes automatically entered the routing table.

---

### 4. Configured ISP Reachability

Added static routes on the ISP routers to ensure return traffic could reach the Enterprise LANs.

**SPR1**

```bash
ip route 10.0.2.0 255.255.255.0 192.168.1.2
```

**SPR2**

```bash
ip route 10.0.1.0 255.255.255.0 192.168.1.1
```

---

### 5. Verified Failover

Verified that:

- Floating static routes replaced OSPF routes after the failure.
- PC1 successfully communicated with SRV1 through the ISP.
- End-to-end connectivity was maintained despite the OSPF link failure.

---

## Verification Commands

### Check Routing Table

```bash
show ip route
```

### Verify OSPF Neighbors

```bash
show ip ospf neighbor
```

### Test Connectivity

```bash
ping 10.0.2.1
```

### Trace Packet Path

```bash
traceroute 10.0.2.1
```

---

## Key Concepts Learned

- Dynamic Routing
- OSPF Administrative Distance
- Static Routes
- Floating Static Routes
- Route Failover
- Routing Table Selection
- Default Routes
- Network Redundancy
- Backup Routing
- High Availability

---

## Result

Successfully configured **Floating Static Routes** as backup paths for OSPF. During normal operation, OSPF selected the primary route. After simulating a link failure, the floating static routes automatically became active, allowing uninterrupted communication between Enterprise networks through the ISP infrastructure.

---

## Skills Practiced

- Configuring Floating Static Routes
- Understanding Administrative Distance
- OSPF Route Verification
- Static Route Configuration
- Simulating Network Failures
- Troubleshooting Routing Issues
- Implementing Network Redundancy
- Verifying Routing Table Changes
- End-to-End Connectivity Testing