# **2.0 Network Access**

## **2.1 Configure and verify VLANs (normal range) spanning multiple switches**

#### 2\.1.a Access ports (data and voice)

- Access ports are configured to belong to a specific VLAN and are used to connect end devices like PCs or phones.
- Voice VLANs are separate VLANs configured on access ports to carry voice traffic for IP phones.
- Example Configuration:
  - `Interface GigabitEthernet 0/1` 
  - `Switchport mode access`
  - `Switchport access vlan 10` (assigns data VLAN 10)
  - `Switchport voice vlan 20` (assigns voice VLAN 20)
  - `No shutdown`
  - `Show vlan brief` (verification command) 

#### 2\.1.b Default VLAN

- The default VLAN on Cisco switches is VLAN 1, which all ports belong to by default.
- It is recommended to avoid using VLAN 1 for user or management traffic for security purposes.

#### 2\.1.c InterVLAN connectivity

- InterVLAN connectivity allows communication between devices in different VLANs by routing traffic through a Layer 3 device (like a router or Layer 3 switch).

- This is done using Router-on-a-Stick or SVI (Switch Virtual Interface).

- Router-on-a-Stick Example:
  - `Interface GIgabitEthernet 0/0.10`
  - `Encapsulation dot1q 10`
  - `Ip address 192.168.10.1 255.255.255.0`
  - –
  - `Interface GigabitEthernet 0/0.20`
  - `Encapsulation dot1q 20`
  - `Ip address 192.168.20.1 255.255.255.0`

- SVI (Switch Virtual Interface) Example:
  - `interface vlan 10`
  - `ip address 192.168.10.1 255.255.255.0`
  - `no shutdown`
  - –
  - `interface vlan 20`
  - `ip address 192.168.20.1 255.255.255.0`
  - `no shutdown`

- `Show ip route` (verification command)

## **2.2 Configure and verify interswitch connectivity**

#### 2\.2.a Trunk ports

- Trunk ports carry traffic for multiple VLANs between switches. They allow VLAN traffic to span across the network.

- Configuration Example (on both switches):
  - `interface GigabitEthernet0/1`
  - `switchport mode trunk`
  - `switchport trunk allowed vlan 10,20,30`   (Allow specific VLANs across the trunk)
  - `no shutdown`

- Verification commands
  - `Show interfaces trunk`
  - `Show vlan brief`

#### 2\.2.b 802.1Q

- 802.1Q is the VLAN tagging protocol used for trunking, allowing VLAN IDs to be identified across trunk links.

- Configuration Example:
  - `interface GigabitEthernet0/1`
  - `switchport trunk encapsulation dot1q`  (Specify 802.1Q encapsulation)
  - `switchport mode trunk`

- Verification commands:
  - `Show interfaces trunk`

#### 2\.2.c Native VLAN

- The native VLAN carries untagged traffic on a trunk link. It must match on both ends of the link to avoid traffic drops.

- Configuration Example (Set Native VLAN to 99):
  - `interface GigabitEthernet0/1`
  - `switchport trunk native vlan 99`

- Verification Commands:
  - `show interfaces trunk`
  - `show running-config interface GigabitEthernet0/1`

## **2.3 Configure and verify Layer 2 discovery protocols (Cisco Discovery Protocol and LLDP)**

**Cisco Discovery Protocol (CDP)**
    - CDP is a Cisco proprietary protocol for discovering directly connected Cisco devices.

  - Enable/Disable CDP:
    - `cdp run` (Globally enable CDP)
    - `interface GigabitEthernet0/1`
    - `cdp enable` (Enable CDP on an interface)

  - Verification command:
    - `Show cdp neighbors`

**Link Layer Discovery Protocol (LLDP)**
    - LLDP is an open-standard protocol for discovering devices on the same Layer 2 network.

  - Enable/Disable LLDP:
    - `lldp run`  (Globally enable LLDP)
    - `interface GigabitEthernet0/1`
    - `lldp transmit`
    - `lldp receive`

  - Verification command:
    - `Show lldp neighbors`

## **2.4 Configure and verify (Layer 2 and Layer 3) EtherChannel (LACP)**

EtherChannel is a technology that allows you to bundle multiple physical Ethernet links into a single logical link, providing increased bandwidth and redundancy.

There are two types of EtherChannel protocols:
  - Link Aggregation Control Protocol (LACP)
  - Port Aggregation Protocol (PAgP) *cisco proprietary*

- LACP configuration example:
  - `interface GigabitEthernet 0/1`
  - `switchport mode trunk`
  - `channel-group 1 mode active`  ( Enable LACP (active mode) )

- channel-group modes
  - `active` : Enable LACP unconditionally
  - `passive` : Enable LACP if a LACP device is detected
  - `auto` : Enable PAgP only if a PAgP device is detected
  - `desirable` : Enable LACP unconditionally
  - `on` : Enable Etherchannel only

- Verification commands:
  - `show interfaces port-channel 1 status`
  - `show interfaces port-channel 1 etherchannel`

## **2.5 Interpret basic operations of Rapid PVST+ Spanning Tree Protocol**

#### 2\.5.a Root port, root bridge (primary/secondary), and other port names

- Root Port: The root port is the port on a non-root switch that provides the lowest-cost path to the root bridge (the switch with the lowest bridge ID).
- Root Bridge: The root bridge (also known as the primary root) is the switch with the lowest bridge ID, which serves as the reference point for the entire network. A secondary root bridge can be designated in the event of a failure of the primary root bridge.
- Designated Port: Provide a path to the root bridge for a particular segment
- Alternate Port: Provide a backup path in the event that the designated port fails.
- Verification command:
  - `show spanning-tree`

#### 2\.5.b Port states (forwarding/blocking)

- Blocking: A blocking port is a port that is inactive and not forwarding traffic. It is used to prevent loops in the network.
- Forwarding: A forwarding port is a port that is active and forwarding traffic in the network.
- Port State Transitions:
  - Blocking -> Listening -> Learning -> Forwarding

#### 2\.5.c PortFast

- PortFast skips the listening and learning states, enabling ports to immediately transition to forwarding for edge devices.

- Configuration example:
  - `interface GigabitEthernet0/1`
  - `spanning-tree portfast`

#### 2\.5.d Root guard, loop guard, BPDU filter, and BPDU guard

- Root Guard: Prevents unauthorized switches from becoming the root bridge.
  - `interface GigabitEthernet0/1`
  - `spanning-tree guard root`

- Loop Guard: Prevents loops by disabling non-designated ports in inconsistent states.
  - `interface GigabitEthernet0/1`
  - `spanning-tree guard loop`

- BDPU Filter: Suppresses BPDU transmission/processing on a port.
  - `interface GigabitEthernet0/1`
  - `spanning-tree bpdufilter enable`

- BDPU Guard: Disables a port if it receives unexpected BPDUs.
  - `interface GigabitEthernet0/1`
  - `spanning-tree bpduguard enable`

- Verification command
  - `Show spanning-tree summary`

## **2.6 Describe Cisco Wireless Architectures and AP modes**

1\. **Cisco Wireless Architectures: Define how Aps and controllers work together (centralized, distributed, cloud, or converged).**

- Centralized Architecture (Controller-Based)
  - A Wireless LAN Controller (WLC) manages multiple Access Points (APs) centrally.
  - All traffic is tunneled to the controller using the Control and Provisioning of Wireless Access Points (CAPWAP) protocol.
  - Ideal for large networks requiring centralized management, advanced features, and seamless roaming.

- Distributed Architecture (Autonomous Aps)
  - APs operate independently without a central controller.
  - Configuration is done manually on each AP, making it suitable for small networks or branch offices.
  - Limited scalability and centralized management.

- Cloud-Based Architecture
  - APs are managed via a cloud-based platform like Cisco Meraki.
  - Provides centralized management, monitoring, and configuration through a web-based interface.
  - Ideal for organizations with multiple distributed locations.

- Converged Access Architecture
  - Combines wireless and wired infrastructure management using a Unified Access Layer.
  - Controllers are embedded within switches, allowing for **local management** of APs.
  - Suitable for medium-sized networks with tight integration of wired and wireless networks.

2\. **Access Point Modes: Determine how an AP operates (client-serving, monitoring, bridging, or analyzing).**

- Local Mode
  - Default mode for lightweight APs in a centralized architecture.
  - Handles client traffic by sending it to the WLC via a CAPWAP tunnel for processing.

- FlexConnect Mode
  - Allows APs to continue functioning during a loss of connection with the WLC.
  - Client traffic is locally switched at the AP instead of being tunneled back to the controller.
  - Ideal for remote sites or branch offices.

- Monitor Mode
  - AP scans the wireless spectrum for rogue APs, interference, and channel usage.
  - Does not serve clients while in this mode.

- Sniffer Mode
  - AP captures and forwards wireless traffic to a network analyzer tool for troubleshooting and monitoring.

- Bridge Mode
  - AP acts as a bridge connecting two networks, typically in a point-to-point or point-to-multipoint configuration.
  - Used for connecting remote buildings.

- Mesh Mode
  - APs form a wireless mesh network, with one AP connected to the wired network (Root AP) and others connecting wirelessly (Mesh APs).
  - Ideal for environments where running cables is difficult.

- SE-Connect Mode (Spectrum Analysis Mode)
  - AP provides real-time RF spectrum analysis to identify interference sources or assess channel utilization.

- Rogue Detector Mode
  - AP scans for unauthorized devices connected to the network to detect rogue APs or clients.

## **2.7 Describe physical infrastructure connections of WLAN components (AP, WLC, access and trunk ports, and LAG)**

**Access Points (Aps)**
  - Role: Aps connect wireless clients to the wired network
  - Physical connection
    - Typically connected to switch access ports.
    - If using PoE (Power over Ethernet), the AP is powered directly through the Ethernet cable, eliminating the need for a separate power source.
  - Configuration: Switchport set to access mode for the VLAN serving wireless clients
    - `interface GigabitEthernet0/1`
    - `switchport mode access`
    - `switchport access vlan 10`

**Wireless LAN Controller (WLC)**
  - Role: Manages APs, handles wireless traffic, and enforces policies.
  - Physical Connection:
    - Connected to the core or distribution switch through trunk ports.
    - Trunking allows the WLC to handle traffic from multiple VLANs, including management, control, and user traffic VLANs.
  - Example configuration for WLC connection:
    - interface GigabitEthernet0/2
    - `switchport mode trunk`
    - `switchport trunk allowed vlan 10,20,30`

**Access Ports (APs and Clients)**
  - Purpose: Used for single VLAN assignments.
  - Key Points:
    - APs: Access ports are assigned to the VLAN for wireless client traffic.
    - Clients: Connect wirelessly to the AP, and their traffic is tagged based on the VLAN configuration of the AP.

**Trunk Ports (WLC and APs in FlecConnect Mode)**
  - Purpose: Trunk ports allow traffic from multiple VLANs
  - Use cases:
    - WLC connections
    - Aps in FlexConnect Mode (locally switching traffic to multiple VLANs)
  - Example Configuration for AP in FlexConnect Mode:
    - `interface GigabitEthernet0/3`
    - `switchport mode trunk`
    - `switchport trunk allowed vlan 10,20,30`

**Link Aggregation Group (LAG)**
  - Purpose: LAG bundles multiple physical links into a single logical connection to increase bandwidth and provide redundancy
  - Use Case:
    - WLCs often use LAG to connect multiple Ethernet interfaces to a switch.
    - This provides load balancing and fault tolerance for wireless traffic.
  - Configuration:
    - Enable LAG on the WLC (this aggregates all physical WLC ports).
    - Configure the switch with a port-channel interface.
  - Example LAG Configuration
    - `interface range GigabitEthernet0/1 - 2`
    - `channel-group 1 mode active`  (Enable LACP)
    - `interface port-channel 1`
    - `switchport mode trunk`
    - `switchport trunk allowed vlan 10,20,30`

|**Component**|**Connection Type**|**Port Configuration**|**Notes**|
| :- | :- | :- | :- |
|**Access Points**|Access Port|switchport mode access|Uses PoE for power or external power source.|
|**WLC**|Trunk Port / LAG|switchport mode trunk|Handles multiple VLANs for wireless traffic.|
|**Access Ports**|Single VLAN|switchport access vlan <VLAN-ID>|Used for client or AP connections.|
|**Trunk Ports**|Multiple VLANs|switchport trunk allowed vlan <vlan-IDs>|Used for APs in FlexConnect or WLC|
|**LAG**|Multiple Links|channel-group and port-channel config|Provides redundancy and load balancing.|

## **2.8 Describe network device management access (Telnet, SSH, HTTP, HTTPS, console, TACACS+, RADIUS, and cloud managed)**

Telnet
  - A protocol that allows for remote command-line management of devices over an unencrypted connection.
  - Operates on TCP port 23

SSH (Secure Shell)
  - A secure protocol for remote command-line management using encrypted communication.
  - Operates on TCP port 22

HTTP
  - A protocol for accessing the web-based interface of devices.
  - Operates on TCP port 80

HTTPS
  - A secure version of HTTP that uses SSL/TLS for encrypted communication.
  - Operates on TCP port 443

Console
  - A physical connection to the device via a console port (e.g., RJ-45 or USB).

TACACS+ (Terminal Access Controller Access Control System Plus)
  - A Cisco-proprietary protocol for centralized authentication, authorization, and accounting (AAA).
  - Operates on TCP port 49.

RADIUS (Remote Authentication Dial-In User Service)
  - A standards-based protocol for centralized AAA, commonly used in wireless networks.
  - Operates on UDP ports 1812 (authentication) and 1813 (accounting).

Cloud-Management
  - Device management via a centralized cloud platform, such as Cisco Meraki Dashboard.

## **2.9 Interpret the wireless LAN GUI configuration for client connectivity, such as WLAN creation, security settings, QoS profiles, and advanced settings**

When configuring a wireless LAN (WLAN) through a graphical user interface (GUI), several key components must be set up to enable client connectivity. Below is an explanation of each major element:

**WLAN Creation**
  - Purpose: Define a wireless network (SSID) for clients to connect to
  - Key Steps in GUI:
    - Provide an SSID name
    - Assign the WLAN to a specific VLAN (if required for segmentation)
    - Enable or disable broadcasting of the SSID

**Security Settings**
- Purpose: Configure authentication and encryption methods to secure the WLAN
- Options in GUI:
  - Open: No authentication; used for guest networks.
  - WPA2/WPA3 Personal: Pre-shared key (PSK)-based authentication for personal use
  - WPA2/WPA3 Enterprise: 802.1X-based authentication using RADIUS server for enterprise environments
  - Encryption Methods: AES is commonly used for secure communication
- Other Features
  - Configure MAC filtering to restrict access based on device MAC addresses

**Quality of Service (QoS) Profiles**
  - Purpose: Prioritize network traffic based on the type of application (e.g., voice, video, or data).
  - Options in GUI:
    - Best Effort: Default QoS for standard traffic
    - Voice: Prioritize voice traffic to ensure low latency and high reliability
    - Video: prioritize video traffic for smoother streaming
    - Background: Low-priority traffic like backups or updates
  - Advanced QoS Settings: Adjust bandwidth limits or traffic shaping policies

**Advanced Settings**
  - Purpose: Configure additional WLAN behaviors and optimizations
  - Options in GUI:
    - Band Selections: Choose whether the WLAN operates on 2.4 GHz, 5GHz, or dual-band
    - Client Load Balancing: Distribute clients evenly across Aps to prevent overloading
    - Fast Roaming: Enable features like 802.11r for seamless handoffs between Aps
    - Data Rate Optimization: Limit minimum data rates to improve performance by forcing clients to connect at faster speeds
    - Broadcast/Multicast Optimization: Reduce overhead by converting multicast traffic to unicast where applicable

