# DHCP Snooping Lab – Cisco Packet Tracer

## 📌 Lab Overview

This lab demonstrates how to configure a Cisco router as a **DHCP server** and enable **DHCP Snooping** on two Layer 2 switches.

The lab also demonstrates why DHCP requests may fail when the switch incorrectly treats the uplink toward the DHCP server as an **untrusted port**, and how to fix the problem by configuring the appropriate uplink interfaces as trusted.

---

## 🖥️ Network Topology

```text
                    192.168.1.0/24
                           |
                     G0/0 | .1
                         R1
                    Cisco 2911
                           |
                        G0/2
                         SW1
                    2960-24TT
                        G0/1
                           |
                        G0/1
                         SW2
                    2960-24TT
                     /     |     \
                  F0/1   F0/2   F0/3
                   |       |       |
                  PC1     PC2     PC3
```

### Device Connections

| Device | Interface | Connected To |
| ------ | --------- | ------------ |
| R1     | G0/0      | SW1 G0/2     |
| SW1    | G0/2      | R1 G0/0      |
| SW1    | G0/1      | SW2 G0/1     |
| SW2    | G0/1      | SW1 G0/1     |
| SW2    | F0/1      | PC1          |
| SW2    | F0/2      | PC2          |
| SW2    | F0/3      | PC3          |

---

# 🎯 Objectives

1. Configure **R1 as a DHCP server**.
2. Exclude IP addresses `192.168.1.1` through `192.168.1.9`.
3. Configure the default gateway as `192.168.1.1`.
4. Enable **DHCP Snooping** on SW1 and SW2.
5. Configure the uplink interfaces as **trusted DHCP Snooping ports**.
6. Test DHCP address assignment from PC1.
7. Understand why DHCP may fail when the uplink is not trusted.
8. Make the necessary configuration changes to restore DHCP functionality.

---

# 🌐 IP Addressing

| Parameter             | Value                       |
| --------------------- | --------------------------- |
| Network               | `192.168.1.0/24`            |
| Subnet Mask           | `255.255.255.0`             |
| Router IP             | `192.168.1.1`               |
| DHCP Excluded Range   | `192.168.1.1 - 192.168.1.9` |
| DHCP Starting Address | `192.168.1.10`              |
| Default Gateway       | `192.168.1.1`               |
| DHCP Pool             | `192.168.1.0/24`            |

Therefore, DHCP clients should receive addresses beginning from:

```text
192.168.1.10
```

---

# 1. Configure R1 as a DHCP Server

Enter privileged EXEC mode:

```cisco
enable
configure terminal
```

Exclude the addresses from `.1` to `.9`:

```cisco
ip dhcp excluded-address 192.168.1.1 192.168.1.9
```

Create the DHCP pool:

```cisco
ip dhcp pool LAN_POOL
```

Configure the network:

```cisco
network 192.168.1.0 255.255.255.0
```

Configure the default gateway:

```cisco
default-router 192.168.1.1
```

Exit configuration mode:

```cisco
exit
```

Configure the router interface:

```cisco
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

Save the configuration:

```cisco
end
write memory
```

### Verify DHCP Configuration

```cisco
show ip dhcp pool
```

Check DHCP bindings:

```cisco
show ip dhcp binding
```

Check the router interface:

```cisco
show ip interface brief
```

The G0/0 interface should be:

```text
GigabitEthernet0/0    192.168.1.1    up    up
```

---

# 2. Configure DHCP Snooping on SW1

Access SW1:

```cisco
enable
configure terminal
```

Enable DHCP Snooping globally:

```cisco
ip dhcp snooping
```

Enable DHCP Snooping for VLAN 1:

```cisco
ip dhcp snooping vlan 1
```

Configure the interface connected to R1 as a trusted port:

```cisco
interface gigabitEthernet 0/2
ip dhcp snooping trust
exit
```

The connection from R1 to SW1 is the path through which legitimate DHCP server responses arrive, so it must be trusted.

---

# 3. Configure DHCP Snooping on SW2

Access SW2:

```cisco
enable
configure terminal
```

Enable DHCP Snooping:

```cisco
ip dhcp snooping
```

Enable it for VLAN 1:

```cisco
ip dhcp snooping vlan 1
```

Configure the uplink toward SW1 as trusted:

```cisco
interface gigabitEthernet 0/1
ip dhcp snooping trust
exit
```

The PC-facing ports should remain **untrusted**.

Therefore, do **not** configure:

```cisco
ip dhcp snooping trust
```

on:

```text
F0/1
F0/2
F0/3
```

---

# 4. Verify DHCP Snooping

On SW1:

```cisco
show ip dhcp snooping
```

You should see DHCP Snooping enabled and VLAN 1 enabled.

Check trusted interfaces:

```cisco
show ip dhcp snooping
```

You can also verify the interface configuration:

```cisco
show running-config interface gigabitEthernet 0/2
```

On SW2:

```cisco
show running-config interface gigabitEthernet 0/1
```

The interface should contain:

```cisco
ip dhcp snooping trust
```

---

# 5. Test DHCP on PC1

On PC1, open:

```text
Desktop → Command Prompt
```

First check the current IP configuration:

```text
ipconfig
```

Then request a new DHCP address:

```text
ipconfig /renew
```

The PC should eventually receive an address similar to:

```text
IP Address:      192.168.1.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.1.1
```

The exact address may differ depending on the DHCP leases already assigned.

---

# 6. What Happens If DHCP Snooping Is Misconfigured?

If DHCP Snooping is enabled but the uplink interface is left **untrusted**, DHCP server messages can be dropped by the switch.

For example, if SW1's connection toward R1 is not trusted:

```text
R1 → SW1
```

DHCP OFFER and DHCP ACK messages from R1 can be discarded because they are received through an untrusted interface.

Similarly, if SW2's uplink toward SW1 is not trusted:

```text
R1 → SW1 → SW2 → PC
```

DHCP server responses can be blocked before they reach the PC.

As a result, running:

```text
ipconfig /renew
```

on PC1 may fail to obtain a valid DHCP address.

---

# 7. Fix the DHCP Problem

The solution is to configure the interfaces toward the legitimate DHCP server as **trusted**.

### SW1

```cisco
enable
configure terminal

ip dhcp snooping
ip dhcp snooping vlan 1

interface gigabitEthernet 0/2
ip dhcp snooping trust
exit

end
```

### SW2

```cisco
enable
configure terminal

ip dhcp snooping
ip dhcp snooping vlan 1

interface gigabitEthernet 0/1
ip dhcp snooping trust
exit

end
```

Save both switches:

```cisco
write memory
```

Then renew the PC's DHCP lease:

```text
ipconfig /release
ipconfig /renew
```

---

# 🔐 Why DHCP Snooping Is Used

DHCP Snooping protects the network from **rogue DHCP servers**.

Without DHCP Snooping, a malicious or unauthorized device could start acting as a DHCP server and provide clients with:

* A fake default gateway
* Incorrect DNS servers
* Incorrect IP addresses
* Malicious network configuration

DHCP Snooping creates a distinction between:

### Trusted Ports

Ports where legitimate DHCP server messages are expected.

In this topology:

```text
SW1 G0/2 → R1
SW2 G0/1 → SW1
```

### Untrusted Ports

Ports where DHCP server messages should not normally originate.

In this topology:

```text
SW2 F0/1 → PC1
SW2 F0/2 → PC2
SW2 F0/3 → PC3
```

---

# 🔄 DHCP Packet Flow

The normal DHCP process is:

```text
PC1
 |
 | DHCP Discover
 ↓
SW2
 |
 | DHCP Discover
 ↓
SW1
 |
 | DHCP Discover
 ↓
R1
 |
 | DHCP Offer
 ↓
SW1
 |
 | DHCP Offer
 ↓
SW2
 |
 | DHCP Offer
 ↓
PC1
```

DHCP Snooping allows legitimate DHCP server responses to pass through the **trusted uplinks**.

---

# 🧪 Verification Commands

## On R1

Check interface status:

```cisco
show ip interface brief
```

Check DHCP pool:

```cisco
show ip dhcp pool
```

Check assigned DHCP addresses:

```cisco
show ip dhcp binding
```

Check DHCP statistics:

```cisco
show ip dhcp server statistics
```

---

## On SW1 and SW2

Check DHCP Snooping:

```cisco
show ip dhcp snooping
```

Check running configuration:

```cisco
show running-config
```

Check interface status:

```cisco
show ip interface brief
```

---

## On PC1

```text
ipconfig
```

```text
ipconfig /release
```

```text
ipconfig /renew
```

Test connectivity to the router:

```text
ping 192.168.1.1
```

---

# ✅ Expected Result

After the correct configuration:

* R1 operates as the DHCP server.
* `192.168.1.1 - 192.168.1.9` are excluded from DHCP allocation.
* PCs receive addresses from `192.168.1.10` onward.
* Default gateway is `192.168.1.1`.
* DHCP Snooping is enabled on SW1 and SW2.
* R1-facing/uplink interfaces are trusted.
* PC-facing interfaces remain untrusted.
* PC1 successfully obtains an IP address using `ipconfig /renew`.
* PC1 can ping `192.168.1.1`.

---

# 📋 Final Configuration Summary

### R1

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

ip dhcp excluded-address 192.168.1.1 192.168.1.9

ip dhcp pool LAN_POOL
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
exit

end
write memory
```

### SW1

```cisco
enable
configure terminal

ip dhcp snooping
ip dhcp snooping vlan 1

interface gigabitEthernet 0/2
ip dhcp snooping trust
exit

end
write memory
```

### SW2

```cisco
enable
configure terminal

ip dhcp snooping
ip dhcp snooping vlan 1

interface gigabitEthernet 0/1
ip dhcp snooping trust
exit

end
write memory
```

---

# 📝 Conclusion

This lab demonstrates the interaction between **DHCP Server configuration** and **DHCP Snooping**. DHCP Snooping improves network security by allowing DHCP server responses only through trusted interfaces. In this topology, the uplinks between R1, SW1, and SW2 must be trusted so that legitimate DHCP messages can reach the PCs, while the PC-facing ports remain untrusted to prevent rogue DHCP servers.
