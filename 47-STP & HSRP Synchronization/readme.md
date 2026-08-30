# HSRP + STP Load Balancing Lab

## 📌 Lab Objective

Configure **HSRP (Hot Standby Router Protocol)** on DSW1 and DSW2 to provide gateway redundancy for VLAN 10 and VLAN 20.

Configure **STP (Spanning Tree Protocol)** so that HSRP active devices are also the STP root bridges for their respective VLANs.

### Required Design

| VLAN    | Network      | HSRP Active | HSRP Standby | STP Root | STP Secondary |
| ------- | ------------ | ----------- | ------------ | -------- | ------------- |
| VLAN 10 | 10.0.10.0/24 | DSW1        | DSW2         | DSW1     | DSW2          |
| VLAN 20 | 10.0.20.0/24 | DSW2        | DSW1         | DSW2     | DSW1          |

### HSRP Virtual Gateway IPs

| VLAN    | DSW1 SVI  | DSW2 SVI  | Virtual IP      |
| ------- | --------- | --------- | --------------- |
| VLAN 10 | 10.0.10.1 | 10.0.10.2 | **10.0.10.254** |
| VLAN 20 | 10.0.20.1 | 10.0.20.2 | **10.0.20.254** |

PC1:

```text
IP Address: 10.0.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 10.0.10.254
```

PC2:

```text
IP Address: 10.0.20.10
Subnet Mask: 255.255.255.0
Default Gateway: 10.0.20.254
```

---

# 1. Network Topology

```text
                    DSW1 ================= DSW2
                 G1/0/3                 G1/0/3
                /       \               /      \
           G1/0/1     G1/0/2       G1/0/2    G1/0/1
             /           \           /          \
          ASW1 ========== ASW2
            |               |
           PC1             PC2
        VLAN 10          VLAN 20
       10.0.10.10       10.0.20.10
```

### Important Links

**DSW1:**

```text
G1/0/1 → ASW1 G0/1
G1/0/2 → ASW2 G0/2
G1/0/3 → DSW2 G1/0/3
```

**DSW2:**

```text
G1/0/1 → ASW2 G0/1
G1/0/2 → ASW1 G0/2
G1/0/3 → DSW1 G1/0/3
```

---

# 2. Configure DSW1

## Create VLANs

```cisco
enable
configure terminal

hostname DSW1

vlan 10
 name VLAN10
exit

vlan 20
 name VLAN20
exit
```

## Enable Layer 3 Routing

```cisco
ip routing
```

## Configure VLAN 10 SVI

DSW1 must be the **HSRP active router** and **STP root** for VLAN 10.

```cisco
interface vlan 10
 ip address 10.0.10.1 255.255.255.0
 standby 10 ip 10.0.10.254
 standby 10 priority 110
 standby 10 preempt
 no shutdown
exit
```

## Configure VLAN 20 SVI

DSW1 must be the **HSRP standby router** and **STP secondary root** for VLAN 20.

```cisco
interface vlan 20
 ip address 10.0.20.1 255.255.255.0
 standby 20 ip 10.0.20.254
 standby 20 priority 100
 standby 20 preempt
 no shutdown
exit
```

---

# 3. Configure STP on DSW1

Use Rapid PVST+:

```cisco
spanning-tree mode rapid-pvst
```

Make DSW1 the root for VLAN 10:

```cisco
spanning-tree vlan 10 root primary
```

Make DSW1 the secondary root for VLAN 20:

```cisco
spanning-tree vlan 20 root secondary
```

---

# 4. Configure DSW1 Trunk Ports

### Link to ASW1

```cisco
interface gigabitEthernet 1/0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

### Cross-link to ASW2

```cisco
interface gigabitEthernet 1/0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

### DSW1 ↔ DSW2

```cisco
interface gigabitEthernet 1/0/3
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

Save:

```cisco
end
write memory
```

---

# 5. Configure DSW2

```cisco
enable
configure terminal

hostname DSW2

vlan 10
 name VLAN10
exit

vlan 20
 name VLAN20
exit

ip routing
```

## Configure VLAN 10 SVI

DSW2 must be the **HSRP standby router** and **STP secondary root** for VLAN 10.

```cisco
interface vlan 10
 ip address 10.0.10.2 255.255.255.0
 standby 10 ip 10.0.10.254
 standby 10 priority 100
 standby 10 preempt
 no shutdown
exit
```

## Configure VLAN 20 SVI

DSW2 must be the **HSRP active router** and **STP root** for VLAN 20.

```cisco
interface vlan 20
 ip address 10.0.20.2 255.255.255.0
 standby 20 ip 10.0.20.254
 standby 20 priority 110
 standby 20 preempt
 no shutdown
exit
```

---

# 6. Configure STP on DSW2

```cisco
spanning-tree mode rapid-pvst
```

Make DSW2 the secondary root for VLAN 10:

```cisco
spanning-tree vlan 10 root secondary
```

Make DSW2 the primary root for VLAN 20:

```cisco
spanning-tree vlan 20 root primary
```

---

# 7. Configure DSW2 Trunk Ports

### Link to ASW2

```cisco
interface gigabitEthernet 1/0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

### Cross-link to ASW1

```cisco
interface gigabitEthernet 1/0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

### DSW2 ↔ DSW1

```cisco
interface gigabitEthernet 1/0/3
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

Save:

```cisco
end
write memory
```

---

# 8. Configure ASW1

```cisco
enable
configure terminal

hostname ASW1

vlan 10
 name VLAN10
exit

vlan 20
 name VLAN20
exit
```

## ASW1 ↔ DSW1

```cisco
interface gigabitEthernet 0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

## ASW1 ↔ DSW2

```cisco
interface gigabitEthernet 0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

---

# 9. Configure PC1 Port on ASW1

Use the switch port connected to PC1. For example, if PC1 is connected to `G0/3`:

```cisco
interface gigabitEthernet 0/3
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 no shutdown
exit
```

If a different port is connected to PC1, replace `G0/3` with that port.

Save:

```cisco
end
write memory
```

---

# 10. Configure ASW2

```cisco
enable
configure terminal

hostname ASW2

vlan 10
 name VLAN10
exit

vlan 20
 name VLAN20
exit
```

## ASW2 ↔ DSW2

```cisco
interface gigabitEthernet 0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

## ASW2 ↔ DSW1

```cisco
interface gigabitEthernet 0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit
```

---

# 11. Configure PC2 Port on ASW2

For example, if PC2 is connected to `G0/3`:

```cisco
interface gigabitEthernet 0/3
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
 no shutdown
exit
```

Save:

```cisco
end
write memory
```

---

# 12. Configure PC1

On PC1:

```text
IP Address:       10.0.10.10
Subnet Mask:      255.255.255.0
Default Gateway:  10.0.10.254
```

---

# 13. Configure PC2

On PC2:

```text
IP Address:       10.0.20.10
Subnet Mask:      255.255.255.0
Default Gateway:  10.0.20.254
```

---

# 14. Verify VLANs

On all switches:

```cisco
show vlan brief
```

Expected:

```text
10    VLAN10
20    VLAN20
```

---

# 15. Verify Trunks

On DSW1, DSW2, ASW1 and ASW2:

```cisco
show interfaces trunk
```

The trunk links should be carrying:

```text
VLAN 10
VLAN 20
```

---

# 16. Verify SVI Status

On DSW1:

```cisco
show ip interface brief
```

Expected:

```text
Vlan10    10.0.10.1    YES    up    up
Vlan20    10.0.20.1    YES    up    up
```

On DSW2:

```cisco
show ip interface brief
```

Expected:

```text
Vlan10    10.0.10.2    YES    up    up
Vlan20    10.0.20.2    YES    up    up
```

> **Important:** An SVI becomes `up/up` only when the VLAN exists and there is at least one active Layer-2 port/trunk carrying that VLAN.

---

# 17. Verify HSRP

On DSW1:

```cisco
show standby brief
```

Expected:

```text
Vlan10    10    Active    local    10.0.10.254
Vlan20    20    Standby   local    10.0.20.254
```

On DSW2:

```cisco
show standby brief
```

Expected:

```text
Vlan10    10    Standby   local    10.0.10.254
Vlan20    20    Active    local    10.0.20.254
```

### HSRP Result

```text
VLAN 10 → DSW1 ACTIVE
VLAN 20 → DSW2 ACTIVE
```

This provides **gateway redundancy** while also distributing the active gateway role across the two distribution switches.

---

# 18. Verify STP

On DSW1:

```cisco
show spanning-tree vlan 10
```

DSW1 should be:

```text
Root Bridge
```

On DSW2:

```cisco
show spanning-tree vlan 20
```

DSW2 should be:

```text
Root Bridge
```

Check the opposite VLANs:

```cisco
DSW1 → VLAN 20 → Secondary Root
DSW2 → VLAN 10 → Secondary Root
```

---

# 19. Test Connectivity

From PC1:

```text
ping 10.0.10.254
```

Then:

```text
ping 10.0.20.10
```

From PC2:

```text
ping 10.0.20.254
```

Then:

```text
ping 10.0.10.10
```

Both PCs should be able to communicate through the Layer-3 distribution switches.

---

# 20. Test HSRP Failover

## VLAN 10 Test

Since DSW1 is active for VLAN 10, shut down its VLAN 10 SVI:

```cisco
DSW1(config)# interface vlan 10
DSW1(config-if)# shutdown
```

Check:

```cisco
DSW2# show standby brief
```

DSW2 should become:

```text
ACTIVE
```

Restore DSW1:

```cisco
DSW1(config)# interface vlan 10
DSW1(config-if)# no shutdown
```

Because `preempt` is configured, DSW1 should regain the active role.

---

# 21. Test VLAN 20 Failover

On DSW2:

```cisco
DSW2(config)# interface vlan 20
DSW2(config-if)# shutdown
```

Check:

```cisco
DSW1# show standby brief
```

DSW1 should become:

```text
ACTIVE
```

Restore:

```cisco
DSW2(config)# interface vlan 20
DSW2(config-if)# no shutdown
```

DSW2 should regain the active role because of:

```cisco
standby 20 preempt
```

---

# 22. Important Verification Commands

### HSRP

```cisco
show standby
show standby brief
```

### STP

```cisco
show spanning-tree
show spanning-tree vlan 10
show spanning-tree vlan 20
```

### VLANs

```cisco
show vlan brief
```

### Trunks

```cisco
show interfaces trunk
```

### SVI/IP status

```cisco
show ip interface brief
```

### Routing

```cisco
show ip route
```

### Interface status

```cisco
show interfaces status
```

---

# 23. Expected Final State

```text
                 DSW1                     DSW2
                  |                         |
             HSRP ACTIVE               HSRP STANDBY
              VLAN 10                   VLAN 10
             STP ROOT                 STP SECONDARY
                  |                         |
                  +-------------------------+

                 DSW1                     DSW2
                  |                         |
             HSRP STANDBY               HSRP ACTIVE
              VLAN 20                   VLAN 20
           STP SECONDARY                STP ROOT
                  |                         |
                  +-------------------------+
```

### Final Roles

| Device   | VLAN 10                      | VLAN 20                      |
| -------- | ---------------------------- | ---------------------------- |
| **DSW1** | HSRP Active + STP Root       | HSRP Standby + STP Secondary |
| **DSW2** | HSRP Standby + STP Secondary | HSRP Active + STP Root       |

---

# 24. Key Concepts Learned

### HSRP

HSRP provides a **virtual default gateway** using a virtual IP address.

For VLAN 10:

```text
DSW1 = 10.0.10.1
DSW2 = 10.0.10.2
VIP  = 10.0.10.254
```

For VLAN 20:

```text
DSW1 = 10.0.20.1
DSW2 = 10.0.20.2
VIP  = 10.0.20.254
```

The PCs always use the **virtual IP** as their default gateway.

### STP

STP prevents Layer-2 switching loops by blocking redundant paths.

In this lab, STP is deliberately aligned with HSRP:

```text
VLAN 10 → DSW1 is HSRP Active + STP Root
VLAN 20 → DSW2 is HSRP Active + STP Root
```

This prevents unnecessary traffic from crossing the network just to reach the active gateway.

### Load Balancing

The lab achieves VLAN-based load balancing:

```text
VLAN 10 traffic → DSW1
VLAN 20 traffic → DSW2
```

while maintaining redundancy:

```text
If DSW1 fails → DSW2 takes over VLAN 10
If DSW2 fails → DSW1 takes over VLAN 20
```

---
