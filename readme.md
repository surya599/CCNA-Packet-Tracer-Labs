# NTP Configuration and Time Synchronization Lab

## 📌 Lab Overview

This lab focuses on configuring **Network Time Protocol (NTP)** on Cisco routers to synchronize system clocks across the network.

The lab demonstrates:

* Configuring the software clock
* Configuring the time zone
* Synchronizing R1 with an external NTP server
* Configuring R1 as a **Stratum 8 NTP master**
* Synchronizing R2 and R3 with R1
* Configuring **NTP authentication**
* Updating the hardware calendar from the software clock

---

## 🗺️ Network Topology

```text
                     Internet
                        |
                   203.0.113.0/30
                        |
                      R1
                    /    \
                   /      \
        192.168.12.0/30   192.168.13.0/30
                 /          \
                R2----------R3
                   192.168.23.0/30
```

### IP Addressing

| Device   | Interface | IP Address      |
| -------- | --------- | --------------- |
| R1       | G0/0      | 203.0.113.1/30  |
| Internet | —         | 203.0.113.2/30  |
| R1       | G0/1      | 192.168.12.1/30 |
| R2       | G0/0      | 192.168.12.2/30 |
| R1       | G0/2      | 192.168.13.1/30 |
| R3       | G0/0      | 192.168.13.2/30 |
| R2       | G0/1      | 192.168.23.1/30 |
| R3       | G0/1      | 192.168.23.2/30 |

---

# 🎯 Objectives

1. Configure the software clock on R1, R2, and R3 to:

   * **12:00:00**
   * **December 30, 2020**
   * **UTC**

2. Configure the appropriate time zone on all routers.

3. Configure R1 to synchronize with NTP server **1.1.1.1** over the Internet.

4. Check the **stratum** of the external NTP server.

5. Configure R1 as a **Stratum 8 NTP master**.

6. Configure R2 and R3 to synchronize with R1 using **NTP authentication**.

7. Configure all routers to update their hardware calendars from NTP.

---

# ⚙️ Step 1 — Configure the Software Clock

First, configure the initial clock on each router.

### R1

```cisco
R1# clock set 12:00:00 30 Dec 2020
```

### R2

```cisco
R2# clock set 12:00:00 30 Dec 2020
```

### R3

```cisco
R3# clock set 12:00:00 30 Dec 2020
```

Verify:

```cisco
show clock
```

---

# 🌍 Step 2 — Configure the Time Zone

Configure the time zone according to your own location.

For example, for **India Standard Time (IST)**:

```cisco
R1(config)# clock timezone IST 5 30
```

Repeat on R2 and R3:

```cisco
R2(config)# clock timezone IST 5 30
R3(config)# clock timezone IST 5 30
```

Verify:

```cisco
show clock
```

> **Note:** The `clock set` command sets the initial UTC time, while the timezone configuration determines how the time is displayed locally.

---

# 🌐 Step 3 — Configure R1 as an NTP Client

R1 must synchronize with the external NTP server **1.1.1.1**.

```cisco
R1(config)# ntp server 1.1.1.1
```

Verify the NTP configuration:

```cisco
R1# show ntp associations
```

and:

```cisco
R1# show ntp status
```

You should look for information such as:

* NTP synchronization status
* Reference clock
* Stratum
* Clock offset
* NTP server association

### Stratum of 1.1.1.1

Do **not assume the stratum value** beforehand. Check it on the router using:

```cisco
show ntp associations
```

The stratum shown by the lab/device is the value you should record.

---

# 🔐 Step 4 — Configure R1 as a Stratum 8 NTP Master

R1 will act as the internal NTP master for R2 and R3.

```cisco
R1(config)# ntp master 8
```

This makes R1 advertise itself as an **NTP Stratum 8 source** when acting as the master.

Verify:

```cisco
R1# show ntp status
```

---

# 🔑 Step 5 — Configure NTP Authentication

NTP authentication prevents unauthorized devices from pretending to be a trusted NTP source.

Choose an authentication key and password.

Example:

```cisco
R1(config)# ntp authenticate
R1(config)# ntp authentication-key 1 md5 NTP123
R1(config)# ntp trusted-key 1
```

Configure the same authentication parameters on R2 and R3:

### R2

```cisco
R2(config)# ntp authenticate
R2(config)# ntp authentication-key 1 md5 NTP123
R2(config)# ntp trusted-key 1
```

### R3

```cisco
R3(config)# ntp authenticate
R3(config)# ntp authentication-key 1 md5 NTP123
R3(config)# ntp trusted-key 1
```

> The authentication key number and password must match between the NTP client and server.

---

# 🖥️ Step 6 — Configure R2 as an NTP Client

According to the topology, R2 can use R1's G0/1 address:

```cisco
R2(config)# ntp server 192.168.12.1 key 1
```

Verify:

```cisco
R2# show ntp associations
R2# show ntp status
```

---

# 🖥️ Step 7 — Configure R3 as an NTP Client

R3 can use R1's G0/2 address:

```cisco
R3(config)# ntp server 192.168.13.1 key 1
```

Verify:

```cisco
R3# show ntp associations
R3# show ntp status
```

---

# 📅 Step 8 — Update the Hardware Calendar

Configure all routers to periodically update the hardware calendar from the software clock.

### R1

```cisco
R1(config)# ntp update-calendar
```

### R2

```cisco
R2(config)# ntp update-calendar
```

### R3

```cisco
R3(config)# ntp update-calendar
```

The Packet Tracer lab notes that the hardware calendar cannot be viewed directly in Packet Tracer.

---

# 🔍 Verification Commands

## Check current time

```cisco
show clock
```

## Check NTP associations

```cisco
show ntp associations
```

## Check NTP status

```cisco
show ntp status
```

## Check NTP configuration

```cisco
show running-config | include ntp
```

---

# ✅ Expected NTP Hierarchy

```text
              External NTP Server
                    1.1.1.1
                       |
                       | NTP
                       ↓
                  R1 - Stratum 8
                   /          \
              NTP /            \ NTP
                 ↓              ↓
        R2 - NTP Client    R3 - NTP Client
```

R1 obtains its time from the external NTP source and provides time synchronization to R2 and R3.

---

# 🧠 Key Concepts Learned

### NTP

**Network Time Protocol (NTP)** synchronizes clocks between network devices.

### Stratum

Stratum represents the distance from the authoritative time source.

```text
Stratum 0 → Reference clock
     ↓
Stratum 1
     ↓
Stratum 2
     ↓
Stratum 3
     ↓
...
Stratum 8 → R1 in this lab
```

A lower stratum generally means the device is closer to the authoritative reference clock.

### NTP Authentication

Authentication allows NTP clients to verify that the time source is trusted.

```text
NTP Authentication
       ↓
Authentication Key
       ↓
Trusted Key
       ↓
NTP Server
```

### Software Clock vs Hardware Calendar

* **Software clock** → Current time used by the router while operating.
* **Hardware calendar** → Persistent hardware-based time.
* `ntp update-calendar` → Updates the hardware calendar using the synchronized software clock.

---

# 📝 Useful Commands Summary

```cisco
! Set software clock
clock set 12:00:00 30 Dec 2020

! Configure timezone
clock timezone IST 5 30

! Configure external NTP server
ntp server 1.1.1.1

! Configure R1 as Stratum 8 master
ntp master 8

! Enable NTP authentication
ntp authenticate

! Configure authentication key
ntp authentication-key 1 md5 NTP123

! Mark key as trusted
ntp trusted-key 1

! Configure NTP client
ntp server 192.168.12.1 key 1
ntp server 192.168.13.1 key 1

! Update hardware calendar
ntp update-calendar

! Verification
show clock
show ntp associations
show ntp status
show running-config | include ntp
```

---

# 🎓 Lab Outcome

After completing this lab:

* R1 synchronizes with an external NTP source.
* R1 operates as a **Stratum 8 NTP master** for the internal network.
* R2 and R3 synchronize their clocks with R1.
* NTP authentication protects the synchronization process.
* The routers can update their hardware calendars using NTP.

**Lab Topic:** NTP Configuration, Stratum, Authentication & Time Synchronization
**Platform:** Cisco Packet Tracer
**Routers:** Cisco 2911
