# Static NAT Configuration Lab – Cisco Packet Tracer

## 📌 Lab Objective

In this lab, you will configure **Static NAT (Network Address Translation)** on router **R1** to allow devices on the private LAN to communicate with the Internet.

You will:

* Test connectivity before NAT configuration.
* Configure inside and outside NAT interfaces.
* Configure static NAT mappings for PC1, PC2, and PC3.
* Verify Internet connectivity after NAT.
* Generate NAT translations by pinging `google.com`.
* Clear NAT translations and observe which entries remain.

---

## 🖥️ Network Topology

```text
PC1 ─┐
PC2 ─┼── SW1 ─── R1 ─── INTERNET ─── Server
PC3 ─┘
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

### Static NAT Mapping

The private IP addresses will be translated to addresses in the `100.0.0.0/24` network.

| Inside Local | Inside Global |
| ------------ | ------------- |
| 172.16.0.1   | 100.0.0.1     |
| 172.16.0.2   | 100.0.0.2     |
| 172.16.0.3   | 100.0.0.3     |

---

## 🧪 Lab Tasks

### 1. Test Connectivity Before NAT

From **PC1**, open Command Prompt and run:

```bash
ping 8.8.8.8
```

The ping should **fail** because NAT has not yet been configured.

---

## 2. Configure Static NAT on R1

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

### Configure Static NAT for PC1

```cisco
ip nat inside source static 172.16.0.1 100.0.0.1
```

### Configure Static NAT for PC2

```cisco
ip nat inside source static 172.16.0.2 100.0.0.2
```

### Configure Static NAT for PC3

```cisco
ip nat inside source static 172.16.0.3 100.0.0.3
```

Exit configuration mode:

```cisco
end
```

---

## 3. Verify NAT Configuration

Check the configured static translations:

```cisco
show ip nat translations
```

You should see entries similar to:

```text
Pro  Inside global      Inside local       Outside local      Outside global
---  100.0.0.1          172.16.0.1         ---                ---
---  100.0.0.2          172.16.0.2         ---                ---
---  100.0.0.3          172.16.0.3         ---                ---
```

You can also verify the NAT interfaces:

```cisco
show ip nat statistics
```

---

## 4. Test Internet Connectivity Again

From **PC1**:

```bash
ping 8.8.8.8
```

The ping should now be **successful**, assuming the routing configuration in the topology is correct.

Test from the other PCs as well:

```bash
PC2> ping 8.8.8.8
PC3> ping 8.8.8.8
```

---

## 5. Test DNS/Domain Connectivity

From each PC, try:

```bash
ping google.com
```

Run the command from:

* PC1
* PC2
* PC3

Then check the NAT translations on R1:

```cisco
show ip nat translations
```

Also check the NAT statistics:

```cisco
show ip nat statistics
```

This allows you to observe how traffic from the internal hosts is represented through NAT.

---

## 6. Clear NAT Translations

To clear dynamic NAT translations:

```cisco
clear ip nat translation *
```

Then check the table again:

```cisco
show ip nat translations
```

### Observation

The **static NAT entries remain** because they are permanent configuration entries.

The dynamic NAT entries, if present, are removed.

This demonstrates the difference between:

* **Static NAT** → Permanent one-to-one mapping.
* **Dynamic NAT** → Temporary translation created when traffic requires it.

---

## 🔍 Important Verification Commands

### View NAT translations

```cisco
show ip nat translations
```

### View NAT statistics

```cisco
show ip nat statistics
```

### View interface configuration

```cisco
show ip interface brief
```

### Verify routing table

```cisco
show ip route
```

### Clear dynamic NAT translations

```cisco
clear ip nat translation *
```

---

## 📚 Key Concepts

### Static NAT

Static NAT creates a **one-to-one mapping** between an inside local private address and an inside global public address.

Example:

```text
172.16.0.1  →  100.0.0.1
```

The mapping remains configured until the administrator removes it.

### Inside Interface

The interface connected to the private/internal network is configured with:

```cisco
ip nat inside
```

In this topology:

```text
R1 G0/1 → Inside
```

### Outside Interface

The interface connected toward the Internet is configured with:

```cisco
ip nat outside
```

In this topology:

```text
R1 G0/0 → Outside
```

---

## 🎯 Expected Results

| Test                         | Expected Result                                |
| ---------------------------- | ---------------------------------------------- |
| PC1 → 8.8.8.8 before NAT     | ❌ Fail                                         |
| Configure Static NAT         | ✅ NAT mappings created                         |
| PC1 → 8.8.8.8 after NAT      | ✅ Success                                      |
| PC2 → 8.8.8.8                | ✅ Success                                      |
| PC3 → 8.8.8.8                | ✅ Success                                      |
| `show ip nat translations`   | Shows static mappings                          |
| `clear ip nat translation *` | Dynamic entries cleared; static entries remain |

---

## 📝 Conclusion

This lab demonstrates how **Static NAT** translates private IP addresses into globally reachable addresses. By configuring `G0/1` as the NAT inside interface and `G0/0` as the NAT outside interface, R1 can translate traffic from PC1, PC2, and PC3.

The lab also demonstrates NAT verification using `show ip nat translations`, `show ip nat statistics`, and the behavior of static entries when NAT translations are cleared.
