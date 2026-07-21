# CCNA Packet Tracer Lab 04 – Cisco IOS CLI and Device Security

## Overview

This lab introduces the Cisco IOS Command-Line Interface (CLI) and basic device security configuration. The objective is to configure hostnames, secure privileged EXEC mode with passwords, encrypt stored passwords, and save the device configuration.

---

## Objective

- Configure hostnames for network devices.
- Secure privileged EXEC mode using passwords.
- Encrypt plain-text passwords.
- Configure an encrypted enable secret.
- Verify password encryption in the running configuration.
- Save the running configuration to startup configuration.

---

## Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco 2911 Router | 1 |
| Cisco 2960 Switch | 1 |
| PCs | 3 |

---

## Tasks Performed

- Changed the router hostname to **R1**.
- Changed the switch hostname to **SW1**.
- Configured an **enable password**.
- Verified access to privileged EXEC mode.
- Enabled password encryption using the `service password-encryption` command.
- Configured an **enable secret** password.
- Compared the encryption methods used for the enable password and enable secret.
- Saved the running configuration to NVRAM.

---

## IOS Commands Practiced

- `hostname`
- `enable password`
- `enable secret`
- `service password-encryption`
- `show running-config`
- `copy running-config startup-config`

---

## Key Concepts Learned

- Cisco IOS configuration modes
- User EXEC mode
- Privileged EXEC mode
- Global Configuration mode
- Enable password vs. Enable secret
- Password encryption
- Configuration management
- Saving device configurations

---

## Files Included

```
Day-04-Cisco-IOS-CLI-and-Device-Security/
│
├── Day 04 Lab - Basic Device Security.pkt
├── topology.png
└── readme.md
```

---

## Learning Outcome

After completing this lab, I can:

- Navigate Cisco IOS configuration modes.
- Configure and verify device hostnames.
- Secure privileged EXEC mode using passwords.
- Differentiate between **enable password** and **enable secret**.
- Encrypt passwords stored in the running configuration.
- Save and verify device configurations.

---

**Status:** ✅ Completed