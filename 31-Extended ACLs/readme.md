# Day 35 – Extended ACLs

## Overview

This lab focuses on configuring and applying **Extended Access Control Lists (ACLs)** on Cisco routers to control traffic based on:

* Source IP address
* Destination IP address
* Protocol
* TCP/UDP port numbers

The objective is to implement specific network security policies without blocking unrelated traffic.

## Lab Objectives

Configure extended ACLs to enforce the following policies:

1. Hosts in **172.16.2.0/24** must not be able to communicate with **PC1 (172.16.1.1)**.
2. Hosts in **172.16.1.0/24** must not be able to access the **DNS service on SRV1 (192.168.1.100)**.
3. Hosts in **172.16.1.0/24** must not be able to access the **HTTP or HTTPS services on SRV2 (192.168.2.100)**.
4. All other permitted traffic should continue to function normally.

## Network Addressing

| Device / Network  | Address          |
| ----------------- | ---------------- |
| PC1               | 172.16.1.1/24    |
| PC2               | 172.16.1.2/24    |
| PC3               | 172.16.2.1/24    |
| PC4               | 172.16.2.2/24    |
| R1 – LAN Gateway  | 172.16.1.254/24  |
| R1 – LAN Gateway  | 172.16.2.254/24  |
| R1 – R2           | 203.0.113.0/30   |
| R1 Serial         | 203.0.113.1/30   |
| R2 Serial         | 203.0.113.2/30   |
| SRV1              | 192.168.1.100/24 |
| SRV2              | 192.168.2.100/24 |
| R2 – SRV1 Gateway | 192.168.1.254/24 |
| R2 – SRV2 Gateway | 192.168.2.254/24 |

## ACL Requirements

### Policy 1 – Block 172.16.2.0/24 → PC1

Traffic from the entire **172.16.2.0/24** network to **172.16.1.1** must be denied.

```cisco
access-list <ACL_NUMBER> deny ip 172.16.2.0 0.0.0.255 host 172.16.1.1
```

### Policy 2 – Block DNS Access to SRV1

Hosts in **172.16.1.0/24** must not be able to access the DNS service on **192.168.1.100**.

DNS normally uses:

```text
UDP 53
TCP 53
```

Example:

```cisco
access-list <ACL_NUMBER> deny udp 172.16.1.0 0.0.0.255 host 192.168.1.100 eq 53
access-list <ACL_NUMBER> deny tcp 172.16.1.0 0.0.0.255 host 192.168.1.100 eq 53
```

### Policy 3 – Block HTTP/HTTPS Access to SRV2

Hosts in **172.16.1.0/24** must not access the web services on **SRV2 (192.168.2.100)**.

HTTP uses TCP port **80** and HTTPS uses TCP port **443**.

```cisco
access-list <ACL_NUMBER> deny tcp 172.16.1.0 0.0.0.255 host 192.168.2.100 eq 80
access-list <ACL_NUMBER> deny tcp 172.16.1.0 0.0.0.255 host 192.168.2.100 eq 443
```

## Important ACL Concept

Extended ACLs should generally be placed **close to the source of the traffic** because they can make decisions based on source, destination, protocol, and port.

Remember that ACLs are processed **top-to-bottom**.

```text
First matching statement → Action is taken
No match → Implicit deny
```

Therefore, if other traffic should remain allowed, an explicit permit statement may be required:

```cisco
access-list <ACL_NUMBER> permit ip any any
```

## Verification Commands

Check the configured ACLs:

```cisco
show access-lists
```

Check the ACL configuration:

```cisco
show running-config
```

Check interface ACL application:

```cisco
show ip interface
```

Test connectivity using:

```text
ping
```

Test application-specific services using:

```text
DNS lookup
HTTP
HTTPS
```

## Expected Results

| Test                       | Expected Result |
| -------------------------- | --------------- |
| 172.16.2.0/24 → PC1        | ❌ Blocked       |
| 172.16.1.0/24 → SRV1 DNS   | ❌ Blocked       |
| 172.16.1.0/24 → SRV2 HTTP  | ❌ Blocked       |
| 172.16.1.0/24 → SRV2 HTTPS | ❌ Blocked       |
| Other permitted traffic    | ✅ Allowed       |

## Key Concepts Learned

* Extended ACLs
* Standard vs Extended ACLs
* ACL wildcard masks
* Source and destination matching
* TCP/UDP port filtering
* DNS port 53
* HTTP port 80
* HTTPS port 443
* ACL processing order
* Implicit deny
* Applying ACLs to interfaces
* Verifying ACL hit counters
* Network traffic filtering and security

## Day 35 Progress

**Topic:** Extended Access Control Lists (Extended ACLs)

**Lab:** Network traffic filtering using source, destination, protocol, and port-based rules

**Status:** ✅ Completed

**CCNA Progress:** Day 35 completed
