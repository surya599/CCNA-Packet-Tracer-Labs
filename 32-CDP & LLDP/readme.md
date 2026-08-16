# Day 36 – CDP and LLDP Configuration Lab

## 📌 Objective

The objective of this lab is to practice **CDP (Cisco Discovery Protocol)** and **LLDP (Link Layer Discovery Protocol)** to identify neighboring network devices, determine missing IP/interface information, and configure secure device-discovery settings.

## 🏗️ Topology

The lab consists of:

* 3 Cisco 2911 routers: **R1, R2, R3**
* 3 Cisco 2960 switches: **SW1, SW2, SW3**
* 3 PCs: **PC1, PC2, PC3**

### Addressing Information

| Device | Interface | IP Address    | Network        |
| ------ | --------- | ------------- | -------------- |
| PC1    | NIC       | 192.168.1.1   | 192.168.1.0/24 |
| R1     | G0/1      | 192.168.1.254 | 192.168.1.0/24 |
| R1     | G0/0      | 10.0.12.1     | 10.0.12.0/30   |
| R1     | G0/0 ↔ R2 | —             | 10.0.12.0/30   |
| R1     | G0/0 ↔ R3 | 10.0.13.1     | 10.0.13.0/30   |
| R2     | G0/0      | 10.0.12.2     | 10.0.12.0/30   |
| R2     | G0/1      | 192.168.2.254 | 192.168.2.0/24 |
| R2     | G0/2      | 10.0.23.1     | 10.0.23.0/30   |
| R3     | G0/1      | 10.0.13.2     | 10.0.13.0/30   |
| R3     | G0/2      | 10.0.23.2     | 10.0.23.0/30   |
| R3     | G0/0      | 192.168.3.254 | 192.168.3.0/24 |
| PC2    | NIC       | 192.168.2.1   | 192.168.2.0/24 |
| PC3    | NIC       | 192.168.3.1   | 192.168.3.0/24 |

> **Note:** The exact missing interface/IP information should be identified using CDP and other IOS commands rather than assuming the values from the topology.

---

## 🎯 Tasks

### 1. Identify Missing IP Addresses and Interface IDs

Use CDP and other Cisco IOS commands to discover neighboring devices and identify:

* Neighboring device hostnames
* Local interfaces connected to neighbors
* Remote interfaces
* Device capabilities
* Platform information
* IP addresses where available

Useful commands:

```bash
show cdp neighbors
show cdp neighbors detail
show cdp entry *
show ip interface brief
show interfaces
```

### 2. Disable CDP on Interfaces Connected to PCs

CDP should not be unnecessarily enabled on interfaces connected directly to end devices.

On the appropriate switch interfaces:

```bash
interface fa0/10
no cdp enable
exit
```

For example, on SW1:

```bash
SW1(config)# interface fa0/10
SW1(config-if)# no cdp enable
```

Repeat the configuration on the PC-facing interfaces of the other switches.

Verify with:

```bash
show cdp interface
```

---

## 3. Disable CDP Globally

Disable CDP globally on each network device:

```bash
no cdp run
```

Verify:

```bash
show cdp
```

The output should indicate that CDP is not running.

> **Important:** If CDP is disabled globally, interface-level CDP configuration becomes inactive until CDP is enabled again.

---

## 4. Enable LLDP Globally

Enable LLDP on each router and switch:

```bash
lldp run
```

Verify:

```bash
show lldp
```

---

## 5. Enable LLDP Transmit and Receive

LLDP transmit and receive are currently disabled on the interfaces.

On interfaces connected to other network devices:

```bash
interface g0/0
lldp transmit
lldp receive
exit
```

Repeat for the appropriate router and switch interfaces.

For switch interfaces:

```bash
interface g0/1
lldp transmit
lldp receive
exit
```

---

## 🔍 LLDP Verification Commands

Use the following commands to verify LLDP operation:

```bash
show lldp neighbors
show lldp neighbors detail
show lldp interface
show lldp
```

The detailed output can provide information such as:

* Neighbor device ID
* Local interface
* Neighbor interface
* Device capabilities
* Platform
* Management IP address

---

## 🧪 Verification Checklist

```bash
show ip interface brief
show cdp neighbors
show cdp neighbors detail
show cdp interface
show lldp
show lldp neighbors
show lldp neighbors detail
show lldp interface
```

Confirm that:

* [ ] Missing interfaces were identified using CDP/IOS commands.
* [ ] Missing IP information was identified where available.
* [ ] CDP was disabled on PC-facing switch interfaces.
* [ ] CDP was disabled globally on all network devices.
* [ ] LLDP was enabled globally on all network devices.
* [ ] LLDP transmit was enabled on required interfaces.
* [ ] LLDP receive was enabled on required interfaces.
* [ ] LLDP neighbors can be discovered successfully.

## 🧠 Key Concepts Learned

### CDP

**Cisco Discovery Protocol** is a Cisco proprietary Layer 2 neighbor-discovery protocol. It allows Cisco devices to discover information about directly connected Cisco devices.

### LLDP

**Link Layer Discovery Protocol** is an IEEE 802.1AB vendor-neutral Layer 2 neighbor-discovery protocol. Unlike CDP, LLDP can operate between devices from different vendors.

### CDP vs LLDP

| Feature                    | CDP               | LLDP         |
| -------------------------- | ----------------- | ------------ |
| Standard                   | Cisco proprietary | IEEE 802.1AB |
| Vendor support             | Primarily Cisco   | Multi-vendor |
| Layer                      | Layer 2           | Layer 2      |
| Neighbor discovery         | Yes               | Yes          |
| Useful for troubleshooting | Yes               | Yes          |

## 💡 Practical Takeaway

CDP and LLDP are extremely useful during network deployment and troubleshooting because they allow administrators to determine **which devices are physically connected, which interfaces are connected, and what neighboring devices are present**.

In production networks, unnecessary discovery protocols should be disabled on end-device-facing interfaces to reduce unnecessary information exposure.

## 📚 Commands Practiced

```bash
show cdp neighbors
show cdp neighbors detail
show cdp entry *
show cdp interface
no cdp enable
no cdp run

lldp run
lldp transmit
lldp receive
show lldp
show lldp neighbors
show lldp neighbors detail
show lldp interface
```

## ✅ Lab Status

**Day 36 completed — CDP and LLDP Neighbor Discovery & Configuration**
