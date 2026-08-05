# EIGRP Configuration Lab

## Objective
Configure and verify Enhanced Interior Gateway Routing Protocol (EIGRP) on a four-router topology. The lab covers:

- Basic router configuration
- Loopback interface configuration
- EIGRP AS 100 configuration
- Passive interfaces
- Disabling automatic summarization
- Unequal-cost load balancing using the `variance` command
- Route verification and troubleshooting

---

## Topology

```
        10.0.12.0/30
      R1 ------------ R2
      |               |
      |               |
10.0.13.0/30     10.0.24.0/30
      |               |
      |               |
      R3 ------------ R4 ----- SW1 ----- PC1
         10.0.34.0/30        192.168.4.0/24
```

---

## IP Addressing

| Device | Interface | IP Address |
|----------|-----------|------------|
| R1 | G0/0 | 10.0.12.1/30 |
| R1 | F1/0 | 10.0.13.1/30 |
| R1 | Lo0 | 1.1.1.1/32 |
| R2 | G0/0 | 10.0.12.2/30 |
| R2 | F1/0 | 10.0.24.1/30 |
| R2 | Lo0 | 2.2.2.2/32 |
| R3 | F1/0 | 10.0.13.2/30 |
| R3 | F2/0 | 10.0.34.1/30 |
| R3 | Lo0 | 3.3.3.3/32 |
| R4 | F1/0 | 10.0.24.2/30 |
| R4 | F2/0 | 10.0.34.2/30 |
| R4 | G0/0 | 192.168.4.254/24 |
| R4 | Lo0 | 4.4.4.4/32 |
| PC1 | NIC | 192.168.4.1/24 |

---

## Configuration Tasks

### 1. Configure Basic Settings

- Assign hostnames.
- Configure IP addresses.
- Enable all interfaces.
- Save the configuration.

---

### 2. Configure Loopback Interfaces

Create Loopback0 on every router.

| Router | Loopback |
|---------|----------|
| R1 | 1.1.1.1/32 |
| R2 | 2.2.2.2/32 |
| R3 | 3.3.3.3/32 |
| R4 | 4.4.4.4/32 |

---

### 3. Configure EIGRP

- Autonomous System: **100**
- Disable automatic summarization.
- Advertise all connected networks.
- Advertise Loopback interfaces.
- Configure passive interfaces where appropriate.

---

### 4. Configure Unequal-Cost Load Balancing

Configure **R1** to use unequal-cost load balancing toward the destination network:

```
192.168.4.0/24
```

using the **variance** command.

---

## Verification Commands

Verify neighbor relationships:

```bash
show ip eigrp neighbors
```

View the EIGRP topology table:

```bash
show ip eigrp topology
```

Display the routing table:

```bash
show ip route eigrp
```

View EIGRP interfaces:

```bash
show ip protocols
```

Verify load balancing:

```bash
show ip route
```

---

## Expected Results

- All routers form EIGRP neighbor adjacencies.
- Loopback interfaces are reachable from every router.
- The 192.168.4.0/24 network is learned by all routers.
- Auto-summary is disabled.
- Passive interfaces suppress unnecessary EIGRP hello packets.
- R1 installs multiple feasible paths toward 192.168.4.0/24 using unequal-cost load balancing.

---

## Concepts Practiced

- EIGRP Neighbor Discovery
- Autonomous System Configuration
- Loopback Advertisement
- Passive Interfaces
- Auto-Summary
- Feasible Successor
- Feasible Distance (FD)
- Reported Distance (RD)
- DUAL Algorithm
- Unequal-Cost Load Balancing
- Route Verification and Troubleshooting

---

## Outcome

Successfully configured an EIGRP network with four routers, advertised loopback interfaces, disabled automatic summarization, implemented passive interfaces, and verified unequal-cost load balancing using the **variance** command.