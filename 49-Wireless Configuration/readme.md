# Wireless LAN Controller (WLC) – VLAN & WLAN Configuration Lab

## Objective

Configure a Cisco Wireless LAN Controller (WLC) to provide wireless access for Internal and Guest users using separate VLANs, dynamic interfaces, and WPA2-PSK authentication.

## Network Information

| VLAN | Purpose | Network | SSID |
|---|---|---|---|
| VLAN 10 | Management | 172.16.1.0/24 | — |
| VLAN 100 | Internal | 10.0.0.0/24 | Internal |
| VLAN 200 | Guest | 10.1.0.0/24 | Guest |

## Devices Used

- Cisco WLC 3504
- Cisco 3500-24PS Switch
- 2 × Lightweight Access Points (LAP)
- PC
- Smartphone

## Topology

- WLC1 connected to SW1 using `G1/0/1`
- AP1 connected to SW1 using `G1/0/2`
- AP2 connected to SW1 using `G1/0/3`
- PC1 connected to SW1 using `G1/0/4`
- Smartphone connects wirelessly through an AP

## WLC Management Access

Access the WLC GUI from PC1 using a web browser.

- Protocol: **HTTPS**
- Username: `admin`
- Password: `Cisco123`

## Lab Tasks

### 1. Access the WLC GUI

From PC1:

1. Open **Web Browser**.
2. Enter the WLC management IP address using **HTTPS**.
3. Login with:
   - Username: `admin`
   - Password: `Cisco123`

### 2. Explore the WLC GUI

Familiarize yourself with the different WLC tabs and identify:

- Current WLC configuration
- Interfaces
- Access Points
- WLANs
- Clients
- Security settings
- Wireless network status
- System information

### 3. Configure Dynamic Interfaces

Create the following dynamic interfaces on the WLC:

#### Internal Interface

- Interface Name: `Internal`
- VLAN ID: `100`
- IP Network: `10.0.0.0/24`

#### Guest Interface

- Interface Name: `Guest`
- VLAN ID: `200`
- IP Network: `10.1.0.0/24`

Configure the appropriate VLAN tagging and switch trunk connectivity between the WLC and SW1.

### 4. Create WLANs

Create two WLANs:

#### Internal WLAN

- WLAN/SSID: `Internal`
- VLAN: `100`
- Security: **WPA2 + PSK**
- Configure a pre-shared key of your choice.

#### Guest WLAN

- WLAN/SSID: `Guest`
- VLAN: `200`
- Security: **WPA2 + PSK**
- Configure a pre-shared key of your choice.

Enable both WLANs after configuration.

### 5. Configure Access Points

Verify that:

- AP1 is connected to SW1.
- AP2 is connected to SW1.
- The APs successfully join the WLC.
- The configured WLANs are being broadcast by the APs.

### 6. Connect a Wireless Client

Use the smartphone as the wireless client.

1. Open the smartphone wireless configuration.
2. Search for available SSIDs.
3. Select the appropriate WLAN.
4. Enter the configured WPA2-PSK password.
5. Verify that the client successfully associates with the AP.

## Verification

Check the following after completing the configuration:

- WLC GUI is accessible through HTTPS.
- APs are successfully joined to the WLC.
- Dynamic interface for VLAN 100 is configured.
- Dynamic interface for VLAN 200 is configured.
- `Internal` WLAN is enabled.
- `Guest` WLAN is enabled.
- WPA2-PSK authentication is working.
- Wireless client successfully connects to the selected SSID.
- Client receives an IP address from the appropriate VLAN/network.
- Client is associated with an AP.

## Expected Result

At the end of the lab:

- **VLAN 10** provides WLC management connectivity.
- **VLAN 100** carries Internal WLAN traffic.
- **VLAN 200** carries Guest WLAN traffic.
- WLC manages the lightweight APs.
- Both `Internal` and `Guest` SSIDs are available.
- Wireless clients can authenticate using WPA2-PSK and connect to the appropriate WLAN.

## Key Concepts Practiced

- Cisco Wireless LAN Controller (WLC)
- Lightweight Access Points (LAP)
- VLANs
- Trunking
- WLC Management Interface
- Dynamic Interfaces
- WLAN/SSID Configuration
- WPA2-PSK
- Wireless Client Association
- Centralized AP Management