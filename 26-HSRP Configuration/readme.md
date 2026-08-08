# HSRP Version 2 Configuration – Day 29

## Lab Overview

This lab demonstrates the configuration and verification of HSRP Version 2 between R1 and R2 to provide first-hop redundancy. R1 and R2 share a Virtual IP address that is configured as the default gateway for PC1 and PC2.

R1 is configured with a higher HSRP priority and acts as the Active Router, while R2 acts as the Standby Router. The lab also tests HSRP failover by shutting down R1 and verifying that R2 takes over as the Active Router.

## Objectives

- Configure HSRP Version 2 on R1 and R2
- Configure the HSRP Virtual IP address
- Configure R1 with a higher priority
- Configure R2 with a lower priority
- Enable HSRP preemption
- Configure the Virtual IP as the default gateway
- Verify HSRP Active and Standby states
- Verify the HSRP virtual MAC address
- Test connectivity to 8.8.8.8
- Test HSRP failover
- Verify R1 becomes Active again after recovery

## Devices Used

- Cisco 2911 Router – R1
- Cisco 2911 Router – R2
- Cisco 2960-24TT Switches – SW1, SW2, SW3, SW4
- PC-PT – PC1
- PC-PT – PC2

## IP Addressing

| Device | Interface | IP Address |
|---|---|---|
| R1 | G0/0 | 10.0.1.253/24 |
| R2 | G0/0 | 10.0.1.252/24 |
| HSRP Virtual IP | — | 10.0.1.254 |
| PC1 | — | 10.0.1.1/24 |
| PC2 | — | 10.0.1.2/24 |
| R1 | G0/1 | 203.0.113.1/30 |
| R2 | G0/1 | 203.0.113.5/30 |

Default Gateway for PC1 and PC2:

10.0.1.254

## HSRP Configuration

### R1 – Active Router

    R1(config)# interface g0/0
    R1(config-if)# standby version 2
    R1(config-if)# standby 1 ip 10.0.1.254
    R1(config-if)# standby 1 priority 110
    R1(config-if)# standby 1 preempt

R1 is configured with a priority of 110, which is higher than the default HSRP priority of 100. Therefore, R1 becomes the Active Router.

### R2 – Standby Router

    R2(config)# interface g0/0
    R2(config-if)# standby version 2
    R2(config-if)# standby 1 ip 10.0.1.254
    R2(config-if)# standby 1 priority 90
    R2(config-if)# standby 1 preempt

R2 is configured with a priority of 90, which is lower than the default priority of 100. Therefore, R2 operates as the Standby Router while R1 is available.

## Verification

Check HSRP status on R1:

    R1# show standby

Expected:

    State is Active

Check HSRP status on R2:

    R2# show standby

Expected:

    State is Standby

The HSRP Virtual IP should be:

    10.0.1.254

## Connectivity Test

From PC1:

    ping 8.8.8.8

From PC2:

    ping 8.8.8.8

Both PCs should be able to reach the external server using 10.0.1.254 as their default gateway.

## ARP Verification

On PC1 or PC2:

    arp -a

The Virtual IP 10.0.1.254 should be mapped to the HSRP virtual MAC address.

For HSRP Version 2, the virtual MAC address format is:

    0000.0C9F.Fxxx

For HSRP group 1:

    0000.0C9F.F001

## HSRP Failover Test

First, save R1's configuration:

    R1# copy running-config startup-config

or:

    R1# write memory

Shut down R1's LAN interface:

    R1(config)# interface g0/0
    R1(config-if)# shutdown

Verify R2:

    R2# show standby

R2 should now become the Active Router.

Test connectivity:

    PC1> ping 8.8.8.8
    PC2> ping 8.8.8.8

R2 should continue providing gateway connectivity after R1 fails.

## Restore R1

Enable R1's interface:

    R1(config)# interface g0/0
    R1(config-if)# no shutdown

Since R1 has a higher priority and preemption is enabled, R1 should automatically become the Active Router again.

Verify:

    R1# show standby

Expected:

    State is Active

R2 should return to:

    State is Standby

## Expected HSRP Behavior

| Condition | R1 | R2 |
|---|---|---|
| Normal operation | Active | Standby |
| R1 fails | Down | Active |
| R1 recovers | Active | Standby |

## Key Concepts Learned

### HSRP

HSRP (Hot Standby Router Protocol) is a Cisco First-Hop Redundancy Protocol that provides a highly available default gateway.

### Virtual IP

The PCs use 10.0.1.254 as their default gateway. This IP is shared between R1 and R2 through HSRP.

### HSRP Priority

The router with the higher priority becomes the Active Router.

- R1 = 110
- R2 = 90

### Preemption

The preempt command allows the higher-priority router to automatically regain the Active role after recovering.

### HSRP Version 2

HSRP Version 2 provides improvements over Version 1 and supports a larger range of HSRP group numbers.


## Conclusion

This lab demonstrated how HSRP Version 2 provides gateway redundancy using a shared Virtual IP address.

R1 was configured as the preferred Active Router with a higher priority, while R2 acted as the Standby Router. When R1 failed, R2 automatically became the Active Router and continued providing gateway services.

After R1 was restored, HSRP preemption allowed R1 to reclaim the Active role.

