# SNMP Configuration and MIB Browser Lab

## 1. Lab Overview

This lab demonstrates how to configure **SNMP (Simple Network Management Protocol)** on a Cisco router and use the **MIB Browser** on a PC to monitor and modify router information.

The lab focuses on:

* Configuring SNMP read-only and read/write communities.
* Using SNMP **Get** messages to retrieve information from a router.
* Viewing router system information through the MIB Browser.
* Identifying the router's interfaces using SNMP.
* Using an SNMP **Set** message to change the router hostname.

> **Note:** SNMP functionality is very limited in Cisco Packet Tracer.

---

## 2. Network Topology

```text
                  192.168.1.0/24
                       |
             G0/0      |
        .254           |          .1
     +--------+     +--------+    +--------+
     |   R1   |-----|  SW1   |----|  PC1   |
     |  2911  |     |2960-24TT|   | PC-PT  |
     +--------+     +--------+    +--------+
```

### Device Addressing

| Device | Interface | IP Address      | Subnet Mask     |
| ------ | --------- | --------------- | --------------- |
| R1     | G0/0      | `192.168.1.254` | `255.255.255.0` |
| PC1    | NIC       | `192.168.1.1`   | `255.255.255.0` |
| SW1    | —         | Not required    | —               |

**Network:** `192.168.1.0/24`

---

## 3. Objectives

By completing this lab, you will be able to:

1. Configure SNMP communities on a Cisco router.
2. Configure a **read-only** SNMP community.
3. Configure a **read/write** SNMP community.
4. Use the MIB Browser to send SNMP Get requests.
5. Retrieve the router's:

   * System uptime
   * Hostname
   * Number of interfaces
   * Interface descriptions
   * Other available system information
6. Use an SNMP Set request to change the router hostname.

---

## 4. SNMP Community Configuration

Two SNMP community strings are required:

| Community | Access     |
| --------- | ---------- |
| `Cisco1`  | Read-Only  |
| `Cisco2`  | Read/Write |

### Configure R1

Open the CLI of **R1** and enter:

```cisco
enable
configure terminal

snmp-server community Cisco1 RO
snmp-server community Cisco2 RW

end
write memory
```

### Verify SNMP Configuration

Use:

```cisco
show running-config | include snmp
```

Expected output:

```text
snmp-server community Cisco1 RO
snmp-server community Cisco2 RW
```

---

## 5. Configure IP Addressing

### R1 Configuration

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
 ip address 192.168.1.254 255.255.255.0
 no shutdown

end
```

Verify:

```cisco
show ip interface brief
```

G0/0 should show:

```text
192.168.1.254    up    up
```

### PC1 Configuration

Configure the PC with:

```text
IP Address:     192.168.1.1
Subnet Mask:    255.255.255.0
Default Gateway: 192.168.1.254
```

Test connectivity from PC1:

```text
ping 192.168.1.254
```

The ping should be successful.

---

## 6. Configure the MIB Browser on PC1

Open:

**PC1 → Desktop → MIB Browser**

Configure the SNMP manager to communicate with R1.

Use:

```text
Remote Host:       192.168.1.254
SNMP Version:      SNMPv1
Read Community:    Cisco1
Write Community:   Cisco2
```

The exact field names may vary slightly depending on the Packet Tracer version.

---

## 7. SNMP Get – System Uptime

Use an SNMP **Get** request to determine how long R1 has been running.

The standard MIB object is:

```text
sysUpTime
```

OID:

```text
1.3.6.1.2.1.1.3.0
```

Send an SNMP Get request.

The returned value represents the amount of time that R1 has been running since its last restart.

---

## 8. SNMP Get – Router Hostname

Use:

```text
sysName
```

OID:

```text
1.3.6.1.2.1.1.5.0
```

The returned value should be the currently configured hostname of R1.

Initially, this will normally be:

```text
R1
```

---

## 9. Find the Number of Interfaces

Use the IF-MIB object:

```text
ifNumber
```

OID:

```text
1.3.6.1.2.1.2.1.0
```

The SNMP response gives the number of interfaces known to the router's interface MIB.

> The number reported by SNMP may include logical/system interfaces in addition to the physical interfaces visible in the topology.

---

## 10. Identify the Interfaces

Use the `ifDescr` table to determine the interface names.

OID:

```text
1.3.6.1.2.1.2.2.1.2
```

The MIB Browser should return interface descriptions such as:

```text
GigabitEthernet0/0
GigabitEthernet0/1
GigabitEthernet0/2
```

Depending on the Packet Tracer router model and IOS implementation, additional logical/system interfaces may also appear.

Record the interfaces returned by the MIB Browser.

---

## 11. Additional Information Available Through SNMP

Use SNMP Get requests to explore the **MIB-2 System Group**.

Useful objects include:

| Information           | MIB Object    | OID                   |
| --------------------- | ------------- | --------------------- |
| System Description    | `sysDescr`    | `1.3.6.1.2.1.1.1.0`   |
| Object ID             | `sysObjectID` | `1.3.6.1.2.1.1.2.0`   |
| System Uptime         | `sysUpTime`   | `1.3.6.1.2.1.1.3.0`   |
| System Contact        | `sysContact`  | `1.3.6.1.2.1.1.4.0`   |
| System Name           | `sysName`     | `1.3.6.1.2.1.1.5.0`   |
| System Location       | `sysLocation` | `1.3.6.1.2.1.1.6.0`   |
| System Services       | `sysServices` | `1.3.6.1.2.1.1.7.0`   |
| Interface Count       | `ifNumber`    | `1.3.6.1.2.1.2.1.0`   |
| Interface Description | `ifDescr`     | `1.3.6.1.2.1.2.2.1.2` |

This demonstrates how SNMP can provide network-management information without directly logging into the router CLI.

---

## 12. SNMP Set – Change the Router Hostname

The `Cisco2` community has **read/write** access, so it can be used for SNMP Set operations.

The MIB object for the hostname is:

```text
sysName
```

OID:

```text
1.3.6.1.2.1.1.5.0
```

In the MIB Browser:

1. Select `sysName`.
2. Select the appropriate instance (`.0`).
3. Choose the **Set** operation.
4. Enter the new hostname.
5. Use the write community:

```text
Cisco2
```

For example, change the hostname to:

```text
SNMP-R1
```

Send the SNMP Set request.

Then verify from the R1 CLI:

```cisco
show running-config | include hostname
```

or:

```cisco
show running-config
```

The hostname should reflect the new value.

---

## 13. Verification

### Verify SNMP Communities

```cisco
show running-config | include snmp
```

Expected:

```text
snmp-server community Cisco1 RO
snmp-server community Cisco2 RW
```

### Verify Router IP

```cisco
show ip interface brief
```

### Verify Connectivity

From PC1:

```text
ping 192.168.1.254
```

### Verify Hostname

```cisco
show running-config | include hostname
```

### Verify SNMP Information

Use the PC1 MIB Browser to retrieve:

* `sysUpTime`
* `sysName`
* `ifNumber`
* `ifDescr`
* `sysDescr`
* `sysObjectID`
* `sysContact`
* `sysLocation`

---

## 14. Expected Results

After completing the lab:

* PC1 can communicate with R1 over `192.168.1.0/24`.
* R1 has the SNMP read-only community `Cisco1`.
* R1 has the SNMP read/write community `Cisco2`.
* PC1 can retrieve router information using SNMP Get.
* The router's uptime can be viewed through SNMP.
* The router hostname can be retrieved through `sysName`.
* Router interfaces can be identified using `ifNumber` and `ifDescr`.
* Additional system information can be explored through the MIB Browser.
* The router hostname can be changed using an SNMP Set request with the read/write community.

---

## 15. Key Commands

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
 ip address 192.168.1.254 255.255.255.0
 no shutdown

snmp-server community Cisco1 RO
snmp-server community Cisco2 RW

end
write memory
```

Verification:

```cisco
show ip interface brief
show running-config | include snmp
show running-config | include hostname
```

---

## 16. Conclusion

This lab demonstrates the basic operation of **SNMPv1 in Cisco Packet Tracer**. The router acts as the SNMP agent, while PC1 functions as the SNMP manager using the MIB Browser.

The **Cisco1** community provides read-only access for retrieving network information, while **Cisco2** provides read/write access and allows configuration values such as the router hostname to be modified through SNMP Set operations.

The lab provides practical experience with network monitoring, MIB objects, SNMP Get requests, and SNMP Set requests.
