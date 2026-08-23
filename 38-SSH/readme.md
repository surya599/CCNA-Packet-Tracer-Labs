# Cisco Packet Tracer Lab – SW2 Management and SSH Security

## 📌 Lab Overview

This lab demonstrates how to configure a newly added Layer 2 switch (**SW2**) for:

* Basic device identification and management
* Local user authentication
* Console line security
* Management SVI configuration
* Default gateway configuration
* Secure remote access using SSH
* RSA key generation
* VTY line security
* Restricting SSH access to **PC1 only**

## 🎯 Objectives

Configure SW2 with:

1. Hostname `SW2`
2. Enable secret
3. Local username/password
4. VLAN 1 SVI
5. Default gateway
6. Console local authentication
7. 5-minute EXEC timeout
8. SSH remote management
9. Domain name `jeremysitlab.com`
10. 2048-bit RSA key
11. SSH-only VTY access
12. Local authentication for SSH
13. SSH access restricted to **PC1 (`192.168.1.1`)**

## 🌐 IP Addressing

| Device  | Interface | IP Address       |
| ------- | --------- | ---------------- |
| PC1     | NIC       | 192.168.1.1/24   |
| SW1     | VLAN 1    | 192.168.1.253/24 |
| R1      | G0/1      | 192.168.1.254/24 |
| R1      | G0/0      | 10.0.0.1/30      |
| R2      | G0/0      | 10.0.0.2/30      |
| R2      | G0/1      | 192.168.2.254/24 |
| SW2     | VLAN 1    | 192.168.2.253/24 |
| Laptop1 | NIC       | 192.168.2.1/24   |

## ⚙️ Complete SW2 Configuration

```cisco
enable
configure terminal

hostname SW2
enable secret ccna
username jeremy secret ccna

interface vlan 1
 ip address 192.168.2.253 255.255.255.0
 no shutdown
exit

ip default-gateway 192.168.2.254

line console 0
 login local
 exec-timeout 5 0
exit

ip domain-name jeremysitlab.com
crypto key generate rsa modulus 2048
ip ssh version 2

access-list 10 permit host 192.168.1.1

line vty 0 15
 login local
 transport input ssh
 exec-timeout 5 0
 access-class 10 in
exit

end
copy running-config startup-config
```

## 🧪 Verification

```cisco
show running-config
show ip interface brief
show ip ssh
show crypto key mypubkey rsa
show access-lists
```

From PC1:

```text
ping 192.168.2.253
ssh -l jeremy 192.168.2.253
```

Password:

```text
ccna
```

### Expected Result

* ✅ PC1 → SW2 SSH: **Allowed**
* ❌ Other devices → SW2 SSH: **Denied**
* ❌ Telnet → SW2: **Disabled**
* ✅ Console authentication: **Local**
* ✅ SSH authentication: **Local**
* ✅ SSH version: **2**
* ✅ RSA key: **2048 bits**
* ✅ Management IP: **192.168.2.253/24**
* ✅ Default gateway: **192.168.2.254**
* ✅ EXEC timeout: **5 minutes**

## 🧠 Key Concepts

**SVI:** Provides the Layer 2 switch with an IP address for management.

**Default Gateway:** Allows SW2 to reach devices outside its local subnet.

**SSH:** Provides encrypted remote management.

**VTY Lines:** Control remote access sessions such as SSH.

**Access-Class:** Restricts incoming VTY connections so that only PC1 can SSH into SW2.

## 🏁 Conclusion

This lab demonstrates how to securely configure a Layer 2 switch for remote management using **SVI, local authentication, SSHv2, RSA encryption, VTY security, and an ACL restricting SSH access to PC1 only**.
::: 
