# DHCP Server, DHCP Client & DHCP Relay Agent Lab

## Objective

Configure R2 as the DHCP server, R1 G0/0 as a DHCP client, and R1 G0/1 as a DHCP relay agent for the `192.168.1.0/24` network.

The lab should result in:

* R1 G0/0 receiving `203.0.113.2` from R2.
* PC1 receiving an address from DHCP Pool 1 through R1.
* PC2 receiving an address from DHCP Pool 2 directly from R2.
* DNS configured as `8.8.8.8`.
* Domain configured as `jeremysitlab.com`.

## Network Addressing

| Device | Interface | Address                 |
| ------ | --------- | ----------------------- |
| R2     | G0/1      | `192.168.2.1/24`        |
| R2     | G0/0      | `203.0.113.1/30`        |
| R1     | G0/0      | DHCP → `203.0.113.2/30` |
| R1     | G0/1      | `192.168.1.1/24`        |
| PC1    | NIC       | DHCP                    |
| PC2    | NIC       | DHCP                    |

## DHCP Pools

### POOL1

* Network: `192.168.1.0/24`
* Exclude: `.1`–`.10`
* Gateway: `192.168.1.1`
* DNS: `8.8.8.8`
* Domain: `jeremysitlab.com`

### POOL2

* Network: `192.168.2.0/24`
* Exclude: `.1`–`.10`
* Gateway: `192.168.2.1`
* DNS: `8.8.8.8`
* Domain: `jeremysitlab.com`

### POOL3

* Network: `203.0.113.0/30`
* Exclude: `.1`
* Expected R1 address: `203.0.113.2`

## R2 DHCP Server Configuration

```cisco
enable
configure terminal

interface gigabitEthernet 0/1
 ip address 192.168.2.1 255.255.255.0
 no shutdown
exit

interface gigabitEthernet 0/0
 ip address 203.0.113.1 255.255.255.252
 no shutdown
exit

ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp excluded-address 192.168.2.1 192.168.2.10
ip dhcp excluded-address 203.0.113.1

ip dhcp pool POOL1
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
 domain-name jeremysitlab.com
exit

ip dhcp pool POOL2
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 8.8.8.8
 domain-name jeremysitlab.com
exit

ip dhcp pool POOL3
 network 203.0.113.0 255.255.255.252
exit

ip route 192.168.1.0 255.255.255.0 203.0.113.2

end
write memory
```

## R1 Configuration

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
 ip address dhcp
 no shutdown
exit

interface gigabitEthernet 0/1
 ip address 192.168.1.1 255.255.255.0
 ip helper-address 203.0.113.1
 no shutdown
exit

end
write memory
```

The important DHCP relay command is:

```cisco
ip helper-address 203.0.113.1
```

## PC Configuration

For both PCs:

**PC → Desktop → IP Configuration → DHCP**

Expected:

* **PC1:** `192.168.1.11` or another available `192.168.1.x` address.
* **PC2:** `192.168.2.11` or another available `192.168.2.x` address.

## Verification

On R2:

```cisco
show ip dhcp pool
show ip dhcp binding
show ip dhcp conflict
show ip interface brief
show ip route
```

On R1:

```cisco
show ip interface brief
show dhcp lease
show running-config interface gigabitEthernet 0/1
```

R1 G0/0 should show:

```text
203.0.113.2
```

## Expected Final Result

| Device  | DHCP Pool | Expected Network |
| ------- | --------- | ---------------- |
| R1 G0/0 | POOL3     | `203.0.113.0/30` |
| PC1     | POOL1     | `192.168.1.0/24` |
| PC2     | POOL2     | `192.168.2.0/24` |

## Troubleshooting

If **R1 does not receive an IP**, verify:

```cisco
show ip interface brief
show dhcp lease
```

If **PC1 does not receive an IP**, verify:

```cisco
show running-config interface gigabitEthernet 0/1
```

and make sure:

```cisco
ip helper-address 203.0.113.1
```

is present.

If DHCP relay replies cannot return to R1, verify R2 has:

```cisco
ip route 192.168.1.0 255.255.255.0 203.0.113.2
```

