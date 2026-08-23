# Cisco IOS Upgrade Using TFTP and FTP

## 📌 Lab Overview

This lab demonstrates how to:

* Configure IP addresses on routers and a server.
* Configure static routing for end-to-end connectivity.
* Use **TFTP** to transfer an IOS image to **R1**.
* Upgrade the IOS on R1 and remove the old IOS image.
* Use **FTP** to transfer an IOS image to **R2**.
* Upgrade the IOS on R2 and remove the old IOS image.

The topology uses two Cisco 2911 routers, one Cisco 2960 switch, and one server.

---

## 🖥️ Network Topology

```text
                 10.0.0.0/24
       ┌─────────────────────────────┐
       │                             │
     SW1                           R1
  Cisco 2960                    Cisco 2911
       │                       G0/1     G0/0
       │                         │        │
     SRV1                        │        │
  10.0.0.1/24              10.0.0.254    │
                                        │
                                192.168.12.0/30
                                        │
                                      G0/0
                                        │
                                       R2
                                   Cisco 2911
                                  192.168.12.2
```

---

## 📋 IP Addressing Table

| Device | Interface | IP Address     | Subnet Mask       | Purpose         |
| ------ | --------- | -------------- | ----------------- | --------------- |
| SRV1   | NIC       | `10.0.0.1`     | `255.255.255.0`   | TFTP/FTP Server |
| R1     | G0/1      | `10.0.0.254`   | `255.255.255.0`   | LAN Gateway     |
| R1     | G0/0      | `192.168.12.1` | `255.255.255.252` | R1-R2 Link      |
| R2     | G0/0      | `192.168.12.2` | `255.255.255.252` | R1-R2 Link      |

### Server Default Gateway

```text
Gateway: 10.0.0.254
```

---

# 1. Configure SRV1

Configure the server with:

```text
IP Address:      10.0.0.1
Subnet Mask:     255.255.255.0
Default Gateway: 10.0.0.254
```

Enable the required services:

* **TFTP** — required for R1.
* **FTP** — required for R2.

For FTP, configure:

```text
Username: jeremy
Password: ccna
```

Place the following IOS image on the server:

```text
c2900-universalk9-mz.SPA.155-3.M4a.bin
```

---

# 2. Configure R1

Enter configuration mode:

```cisco
enable
configure terminal
```

Configure the LAN interface:

```cisco
interface gigabitEthernet 0/1
 ip address 10.0.0.254 255.255.255.0
 no shutdown
exit
```

Configure the R1-R2 link:

```cisco
interface gigabitEthernet 0/0
 ip address 192.168.12.1 255.255.255.252
 no shutdown
exit
```

Save the configuration:

```cisco
end
write memory
```

---

# 3. Configure R2

Enter configuration mode:

```cisco
enable
configure terminal
```

Configure the interface connected to R1:

```cisco
interface gigabitEthernet 0/0
 ip address 192.168.12.2 255.255.255.252
 no shutdown
exit
```

Configure a static route to the server LAN:

```cisco
ip route 10.0.0.0 255.255.255.0 192.168.12.1
```

Save the configuration:

```cisco
end
write memory
```

---

# 4. Verify Connectivity

From R1:

```cisco
ping 10.0.0.1
ping 192.168.12.2
```

From R2:

```cisco
ping 192.168.12.1
ping 10.0.0.1
```

From SRV1, ping the routers:

```text
ping 10.0.0.254
ping 192.168.12.2
```

All required pings should succeed before proceeding with the IOS upgrade.

---

# 5. Upgrade R1 Using TFTP

First check the existing IOS files:

```cisco
show flash:
```

Check the current IOS version:

```cisco
show version
```

Copy the new IOS image from SRV1 using TFTP:

```cisco
copy tftp: flash:
```

When prompted:

```text
Address or name of remote host? 10.0.0.1
Source filename? c2900-universalk9-mz.SPA.155-3.M4a.bin
Destination filename? c2900-universalk9-mz.SPA.155-3.M4a.bin
```

Verify that the new image exists:

```cisco
show flash:
```

---

## Configure R1 to Boot the New IOS

Remove any incorrect boot statements if necessary:

```cisco
configure terminal
no boot system
boot system flash:c2900-universalk9-mz.SPA.155-3.M4a.bin
end
```

Verify:

```cisco
show running-config | include boot
```

Save the configuration:

```cisco
write memory
```

Reload R1:

```cisco
reload
```

After R1 boots, verify the IOS:

```cisco
show version
```

---

## Delete the Old IOS Image on R1

Identify the old image:

```cisco
show flash:
```

Delete the old IOS file:

```cisco
delete flash:<old-ios-filename>
```

Confirm deletion when prompted.

Verify:

```cisco
show flash:
```

The new IOS image should remain in flash.

---

# 6. Upgrade R2 Using FTP

Check the current IOS version:

```cisco
show version
```

Check the contents of flash:

```cisco
show flash:
```

Copy the IOS image from SRV1 using FTP:

```cisco
copy ftp: flash:
```

Enter the FTP server information when prompted:

```text
Address or name of remote host? 10.0.0.1
Source filename? c2900-universalk9-mz.SPA.155-3.M4a.bin
Destination filename? c2900-universalk9-mz.SPA.155-3.M4a.bin
```

When prompted for credentials:

```text
Username: jeremy
Password: ccna
```

Verify the file:

```cisco
show flash:
```

---

# 7. Configure R2 to Boot the New IOS

Configure the new IOS image as the boot image:

```cisco
configure terminal
no boot system
boot system flash:c2900-universalk9-mz.SPA.155-3.M4a.bin
end
```

Verify:

```cisco
show running-config | include boot
```

Save:

```cisco
write memory
```

Reload R2:

```cisco
reload
```

After reboot, verify:

```cisco
show version
```

---

# 8. Delete the Old IOS Image on R2

Check flash:

```cisco
show flash:
```

Identify the old IOS image and delete it:

```cisco
delete flash:<old-ios-filename>
```

Verify:

```cisco
show flash:
```

Only the required/new IOS image should remain.

---

# 9. Verification Commands

### Check Interfaces

```cisco
show ip interface brief
```

Expected interfaces:

```text
R1 G0/1    10.0.0.254       up/up
R1 G0/0    192.168.12.1     up/up

R2 G0/0    192.168.12.2     up/up
```

### Check Routing Table

On R1:

```cisco
show ip route
```

On R2:

```cisco
show ip route
```

R2 should have a route similar to:

```text
S    10.0.0.0/24 [1/0] via 192.168.12.1
```

### Check IOS Version

```cisco
show version
```

### Check Flash

```cisco
show flash:
```

### Test End-to-End Connectivity

From R2:

```cisco
ping 10.0.0.1
```

From R1:

```cisco
ping 192.168.12.2
```

---

# 🎯 Expected Outcome

At the end of the lab:

* SRV1 is configured with `10.0.0.1/24`.
* R1 provides the gateway at `10.0.0.254/24`.
* R1 and R2 communicate over `192.168.12.0/30`.
* R2 has a static route to the `10.0.0.0/24` network.
* R1 successfully retrieves the IOS image using **TFTP**.
* R2 successfully retrieves the IOS image using **FTP**.
* Both routers boot using the new IOS image.
* The old IOS images are removed from flash.
* End-to-end connectivity between SRV1, R1, and R2 is verified.

---

## 🧠 Key Commands Summary

| Task                 | Command                        |
| -------------------- | ------------------------------ |
| Check interfaces     | `show ip interface brief`      |
| Check routing        | `show ip route`                |
| Check IOS            | `show version`                 |
| Check flash          | `show flash:`                  |
| Copy using TFTP      | `copy tftp: flash:`            |
| Copy using FTP       | `copy ftp: flash:`             |
| Configure boot image | `boot system flash:<filename>` |
| Delete IOS           | `delete flash:<filename>`      |
| Save configuration   | `write memory`                 |
| Restart router       | `reload`                       |
| Test connectivity    | `ping <ip-address>`            |

---

## 🔐 FTP Credentials

```text
Username: jeremy
Password: ccna
```

> **Note:** The exact old IOS filename should be identified using `show flash:` before deleting it. Do not delete the IOS image currently being used for booting until the new IOS image has been configured and verified.
