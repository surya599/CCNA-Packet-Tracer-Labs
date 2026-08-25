# Dynamic NAT Configuration Lab – Cisco Packet Tracer

## 📌 Lab Objective

In this lab, you will configure **Dynamic NAT** on Cisco router **R1** to translate private IP addresses from the `172.16.0.0/24` network into a limited pool of public/global IP addresses.

You will:

* Configure NAT inside and outside interfaces.
* Create a dynamic NAT pool.
* Configure an access control list (ACL) to identify inside traffic.
* Test NAT with multiple PCs.
* Observe what happens when the NAT pool has fewer addresses than the number of hosts.
* Replace Dynamic NAT with **PAT (NAT Overload)** using R1's public IP address.
* Verify NAT translations.

---

## 🖥️ Network Topology

```text
                    +--------+
PC1 172.16.0.1 ----|        |
PC2 172.16.0.2 ----|  SW1   |---- R1 ---- INTERNET ---- Server 8.8.8.8
PC3 172.16.0.3 ----|        |
                    +--------+
```

### IP Addressing

| Device          | Interface | IP Address      | Network  |
| --------------- | --------- | --------------- | -------- |
| PC1             | NIC       | 172.16.0.1/24   | Inside   |
| PC2             | NIC       | 172.16.0.2/24   | Inside   |
| PC3             | NIC       | 172.16.0.3/24   | Inside   |
| R1              | G0/1      | 172.16.0.254/24 | Inside   |
| R1              | G0/0      | 203.0.113.1/30  | Outside  |
| Internet Router | G0/0      | 203.0.113.2/30  | Outside  |
| Server          | NIC       | 8.8.8.8         | Internet |

### Dynamic NAT Pool

```text
100.0.0.1 – 100.0.0.2
Subnet: 100.0.0.0/24
```

The pool contains only **2 global addresses**, while there are **3 inside hosts**.

---

# 🧪 Lab Tasks

## 1. Configure Dynamic NAT on R1

Enter privileged EXEC mode:

```cisco
enable
configure terminal
```

### Configure the inside interface

```cisco
interface gigabitEthernet 0/1
ip nat inside
exit
```

### Configure the outside interface

```cisco
interface gigabitEthernet 0/0
ip nat outside
exit
```

---

## 2. Create the Dynamic NAT Pool

Create a pool containing the addresses `100.0.0.1` through `100.0.0.2`:

```cisco
ip nat pool NATPOOL 100.0.0.1 100.0.0.2 netmask 255.255.255.0
```

---

## 3. Configure an ACL for the Inside Network

Create an ACL that permits traffic from the `172.16.0.0/24` network:

```cisco
access-list 1 permit 172.16.0.0 0.0.0.255
```

---

## 4. Associate the ACL with the NAT Pool

Configure Dynamic NAT:

```cisco
ip nat inside source list 1 pool NATPOOL
```

Exit configuration mode:

```cisco
end
```

---

# 🔍 5. Verify the Dynamic NAT Configuration

Check the NAT configuration:

```cisco
show running-config | include ip nat
```

Check the NAT pool:

```cisco
show ip nat pool
```

Check the NAT translations:

```cisco
show ip nat translations
```

Check NAT statistics:

```cisco
show ip nat statistics
```

---

# 🌐 6. Test Dynamic NAT

From **PC1**, run:

```bash
ping google.com
```

Then from **PC2**:

```bash
ping google.com
```

Finally, from **PC3**:

```bash
ping google.com
```

Because the NAT pool contains only **two addresses**, PC1 and PC2 can obtain translations when they generate traffic.

PC3 may fail to obtain a NAT translation if both addresses in the pool are already being used.

### Expected behavior

```text
PC1 → 100.0.0.1
PC2 → 100.0.0.2
PC3 → No available NAT address
```

The exact host that receives an address can depend on the order in which traffic is generated.

---

# 🧹 7. Clear NAT Translations and Remove Dynamic NAT

First clear the existing translations:

```cisco
clear ip nat translation *
```

Remove the Dynamic NAT configuration:

```cisco
configure terminal
no ip nat inside source list 1 pool NATPOOL
no ip nat pool NATPOOL
no access-list 1
end
```

Verify that the Dynamic NAT configuration has been removed:

```cisco
show running-config | include ip nat
```

---

# 🔄 8. Configure PAT Using R1's Public IP

Now configure **PAT (Port Address Translation)** so that multiple inside hosts can share R1's single public IP address.

Create the ACL again:

```cisco
configure terminal
access-list 1 permit 172.16.0.0 0.0.0.255
```

Configure PAT using R1's G0/0 address:

```cisco
ip nat inside source list 1 interface gigabitEthernet 0/0 overload
```

Exit configuration mode:

```cisco
end
```

The `overload` keyword enables PAT.

---

# 🌐 9. Test PAT From All PCs

From **PC1**:

```bash
ping google.com
```

From **PC2**:

```bash
ping google.com
```

From **PC3**:

```bash
ping google.com
```

### Expected Result

All three PCs should be able to communicate with the Internet because they can share the single public address:

```text
203.0.113.1
```

PAT differentiates the connections using **port numbers**.

---

# 🔎 10. Examine NAT Translations

On R1:

```cisco
show ip nat translations
```

You should see multiple inside-local addresses using the same inside-global address, with different port numbers.

For example:

```text
Inside Local        Inside Global
172.16.0.1:xxxxx    203.0.113.1:xxxxx
172.16.0.2:xxxxx    203.0.113.1:xxxxx
172.16.0.3:xxxxx    203.0.113.1:xxxxx
```

Also check:

```cisco
show ip nat statistics
```

---

# 📚 Key Concepts

## Dynamic NAT

Dynamic NAT maps private addresses to addresses from a configured public address pool.

```text
172.16.0.1 → 100.0.0.1
172.16.0.2 → 100.0.0.2
```

The translations are created dynamically when traffic is generated.

---

## NAT Pool

The configured pool contains:

```text
100.0.0.1 – 100.0.0.2
```

Since there are only **2 addresses**, only two hosts can have simultaneous Dynamic NAT translations.

---

## PAT / NAT Overload

PAT allows many private hosts to share one public IP address by using different TCP/UDP port numbers.

```text
172.16.0.1 ─┐
172.16.0.2 ─┼──> 203.0.113.1
172.16.0.3 ─┘
```

This is why PAT is commonly used in networks where many internal devices need Internet access but only a small number of public IP addresses are available.

---

# 📝 Important Commands

| Purpose                     | Command                                                         |
| --------------------------- | --------------------------------------------------------------- |
| View NAT translations       | `show ip nat translations`                                      |
| View NAT statistics         | `show ip nat statistics`                                        |
| View NAT configuration      | `show running-config \| include ip nat`                         |
| Clear NAT translations      | `clear ip nat translation *`                                    |
| Configure NAT pool          | `ip nat pool NATPOOL 100.0.0.1 100.0.0.2 netmask 255.255.255.0` |
| Configure Dynamic NAT       | `ip nat inside source list 1 pool NATPOOL`                      |
| Configure PAT               | `ip nat inside source list 1 interface g0/0 overload`           |
| Configure inside interface  | `ip nat inside`                                                 |
| Configure outside interface | `ip nat outside`                                                |

---

# 🎯 Expected Results

| Test                          | Dynamic NAT                        | PAT              |
| ----------------------------- | ---------------------------------- | ---------------- |
| PC1 → google.com              | ✅                                  | ✅                |
| PC2 → google.com              | ✅                                  | ✅                |
| PC3 → google.com              | ⚠️ May fail when pool is exhausted | ✅                |
| Available global addresses    | 2                                  | 1 shared address |
| Multiple hosts sharing one IP | ❌                                  | ✅                |
| Uses port numbers             | ❌                                  | ✅                |

---

# 🧠 Lab Questions

### 1. Why does PC3 fail with Dynamic NAT?

The NAT pool contains only two addresses (`100.0.0.1` and `100.0.0.2`). If both addresses are already assigned to PC1 and PC2, no address remains for PC3.

### 2. Why does PC3 work after configuring PAT?

PAT allows multiple internal hosts to share R1's single public IP address, `203.0.113.1`, by using different port numbers.

### 3. What happens when NAT translations are cleared?

The existing dynamic translations are removed. New translations are created when the PCs generate traffic again.

### 4. What is the main difference between Dynamic NAT and PAT?

**Dynamic NAT** requires a pool of global addresses and generally maps hosts to available addresses from that pool.

**PAT** allows multiple hosts to share one global IP address by translating the source port numbers.

---

## ✅ Conclusion

This lab demonstrates the operation of **Dynamic NAT and PAT** using Cisco Packet Tracer. Dynamic NAT provides temporary one-to-one translations from a limited public address pool, while PAT allows multiple internal hosts to share a single public IP address.

The lab also demonstrates an important practical networking concept: **when the number of internal hosts exceeds the number of available Dynamic NAT addresses, PAT can provide Internet connectivity by allowing multiple hosts to share one public address.**
