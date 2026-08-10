# Day 31 – IPv4/IPv6 Dual-Stack Configuration

## Objective

Configure an IPv4/IPv6 dual-stack network by assigning IPv6 addresses to the router and PCs, enabling IPv6 routing, configuring default gateways, and verifying connectivity using both IPv4 and IPv6.

## Topology

- **R1:** Cisco 2911 Router
- **SW1, SW2, SW3:** Cisco 2960 Switches
- **PC1, PC2, PC3:** End Devices

## Addressing Table

| Device | Interface | IPv4 Address | IPv6 Address | Default Gateway |
|---|---|---|---|---|
| R1 | G0/0 | 192.168.1.1/24 | 2001:DB8:0:1::1/64 | — |
| R1 | G0/1 | 192.168.2.1/24 | 2001:DB8:0:2::1/64 | — |
| R1 | G0/2 | 192.168.3.1/24 | 2001:DB8:0:3::1/64 | — |
| PC1 | NIC | 192.168.1.2/24 | 2001:DB8:0:1::2/64 | 192.168.1.1 / 2001:DB8:0:1::1 |
| PC2 | NIC | 192.168.2.2/24 | 2001:DB8:0:2::2/64 | 192.168.2.1 / 2001:DB8:0:2::1 |
| PC3 | NIC | 192.168.3.2/24 | 2001:DB8:0:3::2/64 | 192.168.3.1 / 2001:DB8:0:3::1 |

## R1 Configuration

Enter the following commands on R1:

```bash
enable
configure terminal

ipv6 unicast-routing

interface gigabitEthernet 0/0
 ipv6 address 2001:DB8:0:1::1/64
 no shutdown
 exit

interface gigabitEthernet 0/1
 ipv6 address 2001:DB8:0:2::1/64
 no shutdown
 exit

interface gigabitEthernet 0/2
 ipv6 address 2001:DB8:0:3::1/64
 no shutdown
 exit

end
write memory