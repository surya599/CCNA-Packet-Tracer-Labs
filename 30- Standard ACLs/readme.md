# Day 34 – OSPF and ACL Configuration Lab

## Objective

Configure OSPF on R1 and R2 to provide full connectivity between all PCs and servers, then implement standard numbered and named ACLs according to the required network security policies.

## Topology

### R1 Networks

* **G0/0:** 172.16.1.254/24
* **G0/1:** 172.16.2.254/24
* **S0/0/0:** 203.0.113.1/30

### R2 Networks

* **S0/0/0:** 203.0.113.2/30
* **G0/0:** 192.168.1.254/24
* **G0/1:** 192.168.2.254/24

### End Devices

* PC1: 172.16.1.1/24
* PC2: 172.16.1.2/24
* PC3: 172.16.2.1/24
* PC4: 172.16.2.2/24
* SRV1: 192.168.1.100/24
* SRV2: 192.168.2.100/24

## Tasks

### 1. Configure OSPF

Configure OSPF on **R1 and R2** so that all networks are advertised and full connectivity is established between:

* 172.16.1.0/24
* 172.16.2.0/24
* 192.168.1.0/24
* 192.168.2.0/24
* 203.0.113.0/30

Verify using:

```bash
show ip ospf neighbor
show ip route
show ip ospf interface brief
```

Test connectivity using `ping` between the PCs and servers.

### 2. Configure Standard Numbered ACL on R1

Configure a **standard numbered ACL** on R1 to enforce:

* Only **PC1 (172.16.1.1)** and **PC3 (172.16.2.1)** can access the **192.168.1.0/24** network.
* Hosts in **172.16.2.0/24** cannot access **192.168.2.0/24**.
* Hosts in **172.16.1.0/24** cannot access **172.16.2.0/24**.
* Hosts in **172.16.2.0/24** cannot access **172.16.1.0/24**.

Use appropriate `permit`, `deny`, and `ip access-group` commands.

Verify using:

```bash
show access-lists
show ip interface
```

### 3. Configure Standard Named ACLs on R2

Configure **standard named ACLs** on R2 to implement the required access restrictions.

Verify using:

```bash
show access-lists
show ip interface
```

## Verification

Test the ACL policies with `ping`:

* PC1 → SRV1
* PC3 → SRV1
* PC2 → SRV1
* PC4 → SRV1
* 172.16.2.0/24 → 192.168.2.0/24
* 172.16.1.0/24 → 172.16.2.0/24
* 172.16.2.0/24 → 172.16.1.0/24

Expected behavior should match the configured security policies.

## Important Commands

```bash
show ip route
show ip ospf neighbor
show ip ospf interface brief
show access-lists
show ip interface
ping <destination-ip>
```

## Key Concepts Learned

* OSPF network advertisement
* OSPF neighbor relationships
* Dynamic routing between multiple LANs
* Standard numbered ACLs
* Standard named ACLs
* ACL wildcard masks
* ACL placement and direction
* `permit` and `deny` statements
* Verifying ACL operation
* Using ACLs to control inter-network communication

## Day 34 Summary

Completed a combined **OSPF + Standard ACL** Packet Tracer lab. OSPF was used to establish dynamic routing between R1 and R2, while standard ACLs were configured to restrict traffic between specific LANs and servers according to the required network security policies.
