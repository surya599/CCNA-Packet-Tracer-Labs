# Cisco IOS Syslog and Logging Configuration Lab

## 📌 Lab Overview

This lab demonstrates how to configure and manage **system logging (Syslog)** on a Cisco router using Cisco Packet Tracer.

You will practice:

* Configuring router credentials
* Generating Syslog messages by shutting down an interface
* Viewing Syslog messages on the console
* Enabling timestamps for log messages
* Monitoring Syslog messages during a Telnet session
* Configuring buffered logging
* Sending Syslog messages to a remote Syslog server
* Configuring the Syslog severity level

---

## 🖥️ Network Topology

| Device | Interface | IP Address         | Network        |
| ------ | --------- | ------------------ | -------------- |
| R1     | G0/0      | `192.168.1.1/24`   | 192.168.1.0/24 |
| PC1    | NIC       | `192.168.1.12/24`  | 192.168.1.0/24 |
| SRV1   | NIC       | `192.168.1.100/24` | 192.168.1.0/24 |
| PC2    | NIC       | `192.168.1.x/24`   | 192.168.1.0/24 |

**Router:** Cisco 2911
**Switch:** Cisco 2960-24TT
**Network:** `192.168.1.0/24`

---

## 🔐 Initial Router Configuration

Connect to **R1** through the console and configure the required credentials.

```cisco
enable
configure terminal

hostname R1

username jeremy password ccna
enable password ccna
```

Configure the G0/0 interface:

```cisco
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

---

# 1. Generate and Configure Syslog Messages

### Step 1: Shut down G0/0

```cisco
configure terminal
interface gigabitEthernet 0/0
shutdown
```

The router should generate a Syslog message indicating that the interface state has changed.

Example:

```text
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to administratively down
```

### Step 2: Re-enable G0/0

After receiving the Syslog message:

```cisco
no shutdown
```

The interface should generate another message when it comes back up.

### Step 3: Configure timestamps

Enable timestamps for Syslog messages:

```cisco
service timestamps log datetime msec
```

You can verify the configuration with:

```cisco
show running-config
```

### Syslog Severity Levels

Cisco IOS uses severity levels from **0 to 7**:

| Level | Name          | Description                       |
| ----: | ------------- | --------------------------------- |
|     0 | Emergencies   | System unusable                   |
|     1 | Alerts        | Immediate action required         |
|     2 | Critical      | Critical conditions               |
|     3 | Errors        | Error conditions                  |
|     4 | Warnings      | Warning conditions                |
|     5 | Notifications | Normal but significant conditions |
|     6 | Informational | Informational messages            |
|     7 | Debugging     | Debugging messages                |

The interface state-change message is commonly associated with **severity level 5 (notifications)**.

---

# 2. Telnet to R1 and Monitor Syslog

From **PC1**, open the Command Prompt and Telnet to R1's G0/0 interface:

```text
telnet 192.168.1.1
```

Log in using:

```text
Username: jeremy
Password: ccna
```

Enter privileged EXEC mode:

```cisco
enable
```

Password:

```text
ccna
```

---

## Why Does the Syslog Message Not Appear?

When you are connected through Telnet, Syslog messages are **not automatically displayed on the VTY session** in the same way they are on the console.

The current Telnet session needs to be configured to display monitoring messages.

Use:

```cisco
terminal monitor
```

Now generate another Syslog event, for example:

```cisco
configure terminal
interface gigabitEthernet 0/1
shutdown
```

The Syslog message should now appear in the Telnet session.

### Disable the interface again if required:

```cisco
no shutdown
```

> **Note:** In Packet Tracer, `logging monitor` may not be available as it is on some real Cisco IOS environments. `terminal monitor` enables Syslog messages for the current VTY/Telnet session.

---

# 3. Configure Buffered Logging

Configure the router to store Syslog messages in its local logging buffer.

Set the buffer size to **8192 bytes**:

```cisco
configure terminal
logging buffered 8192
```

Verify the logging configuration:

```cisco
show running-config | include logging
```

View the messages stored in the buffer:

```cisco
show logging
```

You should see information similar to:

```text
Syslog logging: enabled
    Console logging: ...
    Monitor logging: ...
    Buffer logging: ...
```

The command:

```cisco
show logging
```

is particularly useful for viewing stored Syslog messages.

---

# 4. Configure SRV1 as a Syslog Server

SRV1 has the IP address:

```text
192.168.1.100
```

The router should send Syslog messages to this server.

### Configure R1

Enter global configuration mode:

```cisco
configure terminal
```

Specify SRV1 as the Syslog server:

```cisco
logging 192.168.1.100
```

Configure the Syslog severity level as **debugging (level 7)**:

```cisco
logging trap debugging
```

The complete configuration is:

```cisco
logging 192.168.1.100
logging trap debugging
```

---

## 🖥️ Configure the Syslog Server in Packet Tracer

On **SRV1**:

1. Click **SRV1**.
2. Open the **Services** tab.
3. Select **SYSLOG**.
4. Turn the Syslog service **On**.
5. Confirm that Syslog messages are being received.

The server should now receive Syslog messages generated by R1.

---

# 🔎 Verification Commands

Use the following commands on R1 to verify the configuration.

### View current configuration

```cisco
show running-config
```

### View logging configuration and messages

```cisco
show logging
```

### Check interface status

```cisco
show ip interface brief
```

### Check configured logging commands

```cisco
show running-config | include logging
```

Expected configuration should contain commands similar to:

```cisco
service timestamps log datetime msec
logging buffered 8192
logging 192.168.1.100
logging trap debugging
```

---

# 📝 Key Commands Summary

```cisco
! Credentials
username jeremy password ccna
enable password ccna

! Timestamps
service timestamps log datetime msec

! Buffered logging
logging buffered 8192

! Remote Syslog server
logging 192.168.1.100

! Syslog severity
logging trap debugging

! Display logs in current Telnet/VTY session
terminal monitor

! View logs
show logging
```

---

## 🎯 Learning Objectives

After completing this lab, you should be able to:

1. Explain the purpose of **Syslog** in Cisco networks.
2. Understand Cisco Syslog severity levels **0–7**.
3. Generate Syslog messages through interface state changes.
4. Display Syslog messages during a Telnet session.
5. Configure local buffered logging.
6. Configure a router to forward Syslog messages to a remote server.
7. Verify Syslog operation using Cisco IOS commands.
