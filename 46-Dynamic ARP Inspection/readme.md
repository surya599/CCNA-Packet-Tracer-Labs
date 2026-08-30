# DHCP Snooping + Dynamic ARP Inspection (DAI) Lab

````markdown
# DHCP Snooping + Dynamic ARP Inspection (DAI) Lab

## 📌 Lab Objective

In this lab, configure:

1. **R1 as a DHCP Server**
2. **DHCP Snooping on SW1 and SW2**
3. **Dynamic ARP Inspection (DAI) on SW1 and SW2**
4. Enable all additional DAI validation checks
5. Configure trusted ports connected to routers or switches
6. Verify DHCP Snooping and DAI operation

---

## 🖥️ Network Topology

```text
                         192.168.1.0/24

                    ┌──────────────┐
                    │     R1      │
                    │   Cisco     │
                    │    2911     │
                    │ G0/0        │
                    │192.168.1.1 │
                    └──────┬───────┘
                           │
                         G0/0
                           │
                         G0/2
                    ┌──────┴───────┐
                    │     SW1      │
                    │ 2960-24TT    │
                    └──────┬───────┘
                         G0/1
                           │
                         G0/1
                    ┌──────┴───────┐
                    │     SW2      │
                    │ 2960-24TT    │
                    └───┬────┬────┬┘
                       F0/1 F0/2 F0/3
                        │    │    │
                       PC1  PC2  PC3
````

### Addressing

| Device | Interface | IP Address       |
| ------ | --------- | ---------------- |
| R1     | G0/0      | 192.168.1.1/24   |
| SW1    | G0/2      | Connected to R1  |
| SW1    | G0/1      | Connected to SW2 |
| SW2    | G0/1      | Connected to SW1 |
| PC1    | NIC       | DHCP             |
| PC2    | NIC       | DHCP             |
| PC3    | NIC       | DHCP             |

### DHCP Network

```text
Network:        192.168.1.0/24
Default Gateway: 192.168.1.1
Excluded IPs:    192.168.1.1 - 192.168.1.9
DHCP Range:      192.168.1.10 - 192.168.1.254
```

---

# 1. Configure R1 as DHCP Server

Enter configuration mode:

```cisco
enable
configure terminal
```

Configure the router interface:

```cisco
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

Exclude the first 9 addresses:

```cisco
ip dhcp excluded-address 192.168.1.1 192.168.1.9
```

Create the DHCP pool:

```cisco
ip dhcp pool LAN
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
exit
```

Save configuration:

```cisco
end
write memory
```

### Verify DHCP Configuration

```cisco
show ip dhcp pool
```

```cisco
show ip dhcp binding
```

```cisco
show ip dhcp excluded-address
```

---

# 2. Configure DHCP Snooping on SW1

Connect to SW1:

```cisco
enable
configure terminal
```

Enable DHCP Snooping globally:

```cisco
ip dhcp snooping
```

Enable it for VLAN 1:

```cisco
ip dhcp snooping vlan 1
```

Configure the port connected to R1 as trusted:

```cisco
interface gigabitEthernet 0/2
ip dhcp snooping trust
exit
```

Configure the uplink to SW2 as trusted:

```cisco
interface gigabitEthernet 0/1
ip dhcp snooping trust
exit
```

Exit and save:

```cisco
end
write memory
```

---

# 3. Configure DHCP Snooping on SW2

Connect to SW2:

```cisco
enable
configure terminal
```

Enable DHCP Snooping globally:

```cisco
ip dhcp snooping
```

Enable it for VLAN 1:

```cisco
ip dhcp snooping vlan 1
```

Trust the uplink connected to SW1:

```cisco
interface gigabitEthernet 0/1
ip dhcp snooping trust
exit
```

The ports connected to PCs remain **untrusted**:

```text
F0/1 → PC1 → UNTRUSTED
F0/2 → PC2 → UNTRUSTED
F0/3 → PC3 → UNTRUSTED
```

Save configuration:

```cisco
end
write memory
```

---

# 4. Configure Dynamic ARP Inspection on SW1

Enter configuration mode:

```cisco
enable
configure terminal
```

Enable DAI for VLAN 1:

```cisco
ip arp inspection vlan 1
```

Enable all additional ARP validation checks:

```cisco
ip arp inspection validate src-mac dst-mac ip
```

Trust the port connected to R1:

```cisco
interface gigabitEthernet 0/2
ip arp inspection trust
exit
```

Trust the uplink connected to SW2:

```cisco
interface gigabitEthernet 0/1
ip arp inspection trust
exit
```

Save:

```cisco
end
write memory
```

---

# 5. Configure Dynamic ARP Inspection on SW2

Enter configuration mode:

```cisco
enable
configure terminal
```

Enable DAI for VLAN 1:

```cisco
ip arp inspection vlan 1
```

Enable all additional ARP validation checks:

```cisco
ip arp inspection validate src-mac dst-mac ip
```

Trust the uplink connected to SW1:

```cisco
interface gigabitEthernet 0/1
ip arp inspection trust
exit
```

Keep the PC-facing ports untrusted:

```text
F0/1 → PC1 → UNTRUSTED
F0/2 → PC2 → UNTRUSTED
F0/3 → PC3 → UNTRUSTED
```

Save:

```cisco
end
write memory
```

---

# 6. Configure PCs for DHCP

On each PC:

```text
PC1 → Desktop → IP Configuration → DHCP
PC2 → Desktop → IP Configuration → DHCP
PC3 → Desktop → IP Configuration → DHCP
```

The PCs should receive addresses from R1.

Expected addresses:

```text
PC1 → 192.168.1.10 or higher
PC2 → 192.168.1.10 or higher
PC3 → 192.168.1.10 or higher
```

Default Gateway:

```text
192.168.1.1
```

---

# 7. Verify DHCP Snooping

## SW1

Run:

```cisco
show ip dhcp snooping
```

You should see:

```text
DHCP snooping is enabled
DHCP snooping is configured on VLANs:
1
```

Check trusted interfaces:

```cisco
show ip dhcp snooping
```

SW1 should have:

```text
G0/2 → Trusted
G0/1 → Trusted
```

Check the DHCP binding table:

```cisco
show ip dhcp snooping binding
```

---

## SW2

Run:

```cisco
show ip dhcp snooping
```

Expected:

```text
G0/1 → Trusted
F0/1 → Untrusted
F0/2 → Untrusted
F0/3 → Untrusted
```

Check bindings:

```cisco
show ip dhcp snooping binding
```

The DHCP bindings should contain information such as:

```text
MAC Address
IP Address
Lease Time
Type
VLAN
Interface
```

---

# 8. Verify Dynamic ARP Inspection

On SW1:

```cisco
show ip arp inspection
```

Verify the configuration:

```cisco
show ip arp inspection vlan 1
```

Check trusted interfaces:

```cisco
show ip arp inspection interfaces
```

Expected:

```text
G0/2 → Trusted
G0/1 → Trusted
```

---

## SW2

Run:

```cisco
show ip arp inspection
```

```cisco
show ip arp inspection vlan 1
```

Check interfaces:

```cisco
show ip arp inspection interfaces
```

Expected:

```text
G0/1 → Trusted
F0/1 → Untrusted
F0/2 → Untrusted
F0/3 → Untrusted
```

---

# 9. Verify DAI Validation Checks

Run:

```cisco
show ip arp inspection vlan 1
```

The validation configuration should include:

```text
src-mac
dst-mac
ip
```

The command used was:

```cisco
ip arp inspection validate src-mac dst-mac ip
```

### Validation Checks

| Check     | Purpose                                                               |
| --------- | --------------------------------------------------------------------- |
| `src-mac` | Verifies the ARP source MAC against the Ethernet source MAC           |
| `dst-mac` | Verifies the ARP destination MAC against the Ethernet destination MAC |
| `ip`      | Validates the ARP IP addresses                                        |

---

# 10. Test Connectivity

From PC1:

```text
ping 192.168.1.1
```

From PC2:

```text
ping 192.168.1.1
```

From PC3:

```text
ping 192.168.1.1
```

Test PC-to-PC connectivity:

```text
PC1 → ping PC2
PC1 → ping PC3
PC2 → ping PC3
```

All legitimate traffic should work.

---

# 11. DHCP Snooping Binding Verification

On SW2:

```cisco
show ip dhcp snooping binding
```

You should see entries similar to:

```text
MacAddress        IpAddress       Lease(sec)  Type
------------------------------------------------------
xxxx.xxxx.xxxx    192.168.1.10    ...         dhcp
xxxx.xxxx.xxxx    192.168.1.11    ...         dhcp
xxxx.xxxx.xxxx    192.168.1.12    ...         dhcp
```

These DHCP bindings provide the trusted IP-to-MAC information that DAI uses to validate ARP packets.

---

# 12. Important Concept

## DHCP Snooping

DHCP Snooping protects the network against **rogue DHCP servers**.

It divides switch ports into:

```text
Trusted Ports
     ↓
Legitimate DHCP server / network infrastructure

Untrusted Ports
     ↓
End-user devices
```

In this topology:

```text
R1
 │
 │ Trusted
 ▼
SW1
 │
 │ Trusted
 ▼
SW2
 │
 ├── F0/1 → PC1 → Untrusted
 ├── F0/2 → PC2 → Untrusted
 └── F0/3 → PC3 → Untrusted
```

---

# 13. Dynamic ARP Inspection

DAI protects the network against **ARP spoofing / ARP poisoning**.

DAI checks ARP packets arriving on untrusted ports against the DHCP Snooping binding table.

```text
PC
 │
 │ ARP Request/Reply
 ▼
SW2
 │
 ├── Check source MAC
 ├── Check destination MAC
 ├── Check IP information
 └── Compare with DHCP Snooping binding
          │
          ├── Valid → Forward
          │
          └── Invalid → Drop
```

---

# 14. Trusted vs Untrusted Ports

### SW1

```text
G0/2 → R1    → TRUSTED
G0/1 → SW2   → TRUSTED
```

### SW2

```text
G0/1 → SW1   → TRUSTED

F0/1 → PC1   → UNTRUSTED
F0/2 → PC2   → UNTRUSTED
F0/3 → PC3   → UNTRUSTED
```

Only ports connected to a legitimate router or switch infrastructure are trusted.

---

# 15. Complete Configuration Summary

## R1

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

ip dhcp excluded-address 192.168.1.1 192.168.1.9

ip dhcp pool LAN
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
exit

end
write memory
```

---

## SW1

```cisco
enable
configure terminal

ip dhcp snooping
ip dhcp snooping vlan 1

interface gigabitEthernet 0/2
ip dhcp snooping trust
ip arp inspection trust
exit

interface gigabitEthernet 0/1
ip dhcp snooping trust
ip arp inspection trust
exit

ip arp inspection vlan 1
ip arp inspection validate src-mac dst-mac ip

end
write memory
```

---

## SW2

```cisco
enable
configure terminal

ip dhcp snooping
ip dhcp snooping vlan 1

interface gigabitEthernet 0/1
ip dhcp snooping trust
ip arp inspection trust
exit

ip arp inspection vlan 1
ip arp inspection validate src-mac dst-mac ip

end
write memory
```

---

# 16. Verification Commands

### R1

```cisco
show ip interface brief
show ip dhcp pool
show ip dhcp binding
```

### SW1 / SW2

```cisco
show ip interface brief
show vlan brief
show ip dhcp snooping
show ip dhcp snooping binding
show ip arp inspection
show ip arp inspection vlan 1
show ip arp inspection interfaces
```

---

# 17. Expected Result

After completing the lab:

* R1 provides DHCP addresses to all PCs.
* PC1, PC2 and PC3 receive addresses from `192.168.1.10` onward.
* DHCP Snooping is enabled on SW1 and SW2.
* R1-facing and switch-uplink ports are trusted.
* PC-facing ports remain untrusted.
* DHCP Snooping creates IP-to-MAC bindings.
* DAI uses those bindings to validate ARP packets.
* Source MAC, destination MAC and IP validation are enabled.
* Legitimate ARP traffic is forwarded.
* Invalid/suspicious ARP traffic is dropped.
* PC-to-PC and PC-to-router connectivity works normally.

---

# 🎯 Key Takeaway

**DHCP Snooping + DAI provides protection against rogue DHCP servers and ARP spoofing.**

```text
DHCP Snooping
      ↓
Builds trusted IP-MAC bindings
      ↓
Dynamic ARP Inspection
      ↓
Validates ARP packets
      ↓
Legitimate ARP → FORWARD
Fake ARP        → DROP
```

```
```
