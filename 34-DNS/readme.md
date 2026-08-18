# DNS Configuration and Name Resolution Lab

## 📌 Objective

The objective of this lab is to configure DNS settings on end devices and router **R1**, configure a default route toward the Internet, and observe how DNS name resolution works using Packet Tracer Simulation Mode.

## 🖥️ Topology

```text
                Internet
                   |
              203.0.113.0/30
                   |
             2911 INTERNET
              203.0.113.2
                   |
              G0/0 .1
                 R1
              G0/1 .254
                   |
              192.168.0.0/24
                   |
                SW1
             /    |    \
          PC1    PC2    PC3
          .1     .2     .3
```

### Addressing

| Device          | Interface | IP Address       |
| --------------- | --------- | ---------------- |
| Internet Router | —         | 203.0.113.2      |
| R1              | G0/0      | 203.0.113.1/30   |
| R1              | G0/1      | 192.168.0.254/24 |
| PC1             | —         | 192.168.0.1/24   |
| PC2             | —         | 192.168.0.2/24   |
| PC3             | —         | 192.168.0.3/24   |
| DNS Server      | —         | 1.1.1.1          |

## ⚙️ Tasks

### 1. Configure a Default Route on R1

The default route allows R1 to forward traffic destined for networks that are not present in its routing table toward the Internet.

```cisco
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

Verify:

```cisco
R1# show ip route
```

You should see a default route:

```text
S* 0.0.0.0/0 [1/0] via 203.0.113.2
```

---

### 2. Configure DNS on PC1, PC2 and PC3

On each PC, configure:

* **DNS Server:** `1.1.1.1`
* **Default Gateway:** `192.168.0.254`

Example for PC1:

```text
IP Address:      192.168.0.1
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.0.254
DNS Server:      1.1.1.1
```

Configure the corresponding IP addresses on PC2 and PC3.

---

### 3. Configure R1 to Use 1.1.1.1 as DNS Server

Enter global configuration mode:

```cisco
R1(config)# ip name-server 1.1.1.1
```

Verify:

```cisco
R1# show running-config | include name-server
```

Expected:

```text
ip name-server 1.1.1.1
```

---

### 4. Configure a Host Entry on R1

Configure R1 so that the hostname `youtube.com` resolves to the Internet server's IP address.

```cisco
R1(config)# ip host youtube.com 203.0.113.2
```

Verify:

```cisco
R1# show hosts
```

You should see an entry similar to:

```text
youtube.com    203.0.113.2
```

Now test name resolution from R1:

```cisco
R1# ping youtube.com
```

R1 should resolve `youtube.com` to `203.0.113.2` and attempt to ping that address.

---

## 🔬 Simulation Mode

Switch Packet Tracer to **Simulation Mode**.

From **PC1**, execute:

```text
ping youtube.com
```

### Observe the packets

You should be able to observe the DNS resolution process.

The PC first needs to determine the IP address associated with:

```text
youtube.com
```

It sends a DNS query toward the configured DNS server:

```text
PC1 → DNS Server (1.1.1.1)
```

After receiving the DNS response, the PC can send ICMP traffic toward the resolved IP address.

```text
PC1 → Internet → youtube.com
```

### Important Concept

DNS and routing perform **different jobs**:

```text
DNS:
youtube.com → IP address

Routing:
Destination IP → Where should the packet go?
```

For example:

```text
youtube.com
      ↓
203.0.113.2
      ↓
Default Route on R1
      ↓
203.0.113.2 (Internet Router)
```

---

## 🧪 Verification Commands

### Check the routing table

```cisco
R1# show ip route
```

### Check DNS configuration

```cisco
R1# show running-config | include name-server
```

### Check configured hostnames

```cisco
R1# show hosts
```

### Test connectivity to the DNS server

```cisco
R1# ping 1.1.1.1
```

### Test hostname resolution from R1

```cisco
R1# ping youtube.com
```

### Test from PC1

```text
ping youtube.com
```

---

## 📚 Key Concepts Learned

* Configuring a **default static route**
* Understanding the purpose of a **DNS server**
* Configuring DNS settings on end devices
* Configuring `ip name-server` on Cisco IOS
* Creating static hostname-to-IP mappings using `ip host`
* Understanding **DNS name resolution**
* Understanding the difference between **DNS and routing**
* Using **Packet Tracer Simulation Mode** to analyze DNS and ICMP packets
* Troubleshooting connectivity using `ping` and `show` commands

## 🎯 Lab Outcome

After completing this lab, PC1, PC2, and PC3 can use the configured DNS server for hostname resolution, while R1 has a default route toward the Internet and a local hostname mapping for `youtube.com`.
