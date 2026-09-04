````markdown
# GRE Tunnel with OSPF Lab

## Objective

1. Configure a GRE tunnel to connect R1 and R2.
2. Configure OSPF on the tunnel interfaces of R1 and R2 to allow PC1 and PC2 to communicate.

## Addressing

| Device | Interface | IP Address |
|---|---|---|
| R1 | G0/0 | 10.0.1.1/24 |
| R1 | G0/0/0 | 100.0.0.2/30 |
| R1 | Tunnel0 | 192.168.1.1/30 |
| R2 | G0/0 | 10.0.2.1/24 |
| R2 | G0/0/0 | 200.0.0.2/30 |
| R2 | Tunnel0 | 192.168.1.2/30 |
| PC1 | NIC | 10.0.1.100/24 |
| PC2 | NIC | 10.0.2.100/24 |

## Tasks

### 1. Configure GRE Tunnel

Configure a GRE tunnel between **R1 and R2** using:

```text
Tunnel Network: 192.168.1.0/30

R1 Tunnel0: 192.168.1.1
R2 Tunnel0: 192.168.1.2
````

The tunnel should use the WAN interfaces as the tunnel source and destination.

### 2. Configure OSPF

Configure **OSPF** on the tunnel interfaces of R1 and R2.

Use:

```text
OSPF Process ID: 1
Area: 0
```

Advertise:

```text
R1: 10.0.1.0/24
R2: 10.0.2.0/24
GRE: 192.168.1.0/30
```

### 3. Verify Connectivity

Verify that:

* GRE Tunnel is up.
* OSPF neighbor relationship is established.
* R1 learns the `10.0.2.0/24` network.
* R2 learns the `10.0.1.0/24` network.
* PC1 can ping PC2.
* PC2 can ping PC1.

## Verification Commands

```cisco
show ip interface brief
show interfaces tunnel 0
show ip ospf neighbor
show ip route
ping 192.168.1.2
ping 10.0.2.100
```

## Expected Result

PC1 and PC2 should successfully communicate through the **GRE tunnel**, with **OSPF** providing the routing between the two office networks.

```
```
