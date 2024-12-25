**1.0 Network Fundamentals**

1. **Explain the role and function of network components** 

1\.1a Routers

- Routers direct data packets between different networks, ensuring efficient data transmission and connectivity.

1\.1.b Layer 2 and Layer 3 Switches

- Layer 2 switches handle data transmission within the same network (using MAC addresses), while
- Layer 3 switches also route data between different networks (using IP addresses).

1\.1.c Next-Generation Firewalls and IPS

- Next-generation firewalls provide advanced network security features, including application awareness and intrusion prevention systems (IPS) to block malicious activities.

1\.1.d Access Points

- Access Points connect wireless devices to a wired network, enabling seamless Wi-Fi communication and mobility.

1\.1.e Controllers (Cisco DNA Center and WLC)

- Controllers like Cisco DNA Center and Wireless LAN controllers (WLC) centralize management and automation of network devices and wireless access points.

1\.1.f Endpoints

- Endpoints are devices like laptops, phones, and IoT devices that connect to the network for communication and services.

1\.1.g Servers

- Servers provide centralized resources, applications, and services to clients on a network.

1\.1.h PoE (Power over Ethernet)

- PoE delivers power and data over Ethernet cables to devices like access points, IP cameras, and VoIP phones, simplifying installations.

  1. **Describe characteristics of network topology architectures**

1\.2.a Two-Tier

- A two-tier topology consists of access and distribution layers, providing simplicity for small to medium-sized networks.

1\.2.b Three-tier

- A three-tier topology adds a core layer to the access and distribution layers, enabling scalability and optimized data flow in large networks.

1\.2.c Spine-leaf

- The spine-leaf architecture connects every leaf switch to every spine switch, ensuring low-latency, high-bandwidth connectivity for data centers.

1\.2.d WAN

- WAN connects multiple local area networks (LANs) over large geographical areas, enabling communication across cities, countries, or globally.

1\.2.e Small office/home office (SOHO)

- SOHO networks are compact setups designed to provide basic connectivity, often integrating routers, switches, and access points into a single device.

1\.2.f On-premise and cloud

- On-premise networks host infrastructure locally within an organization, while cloud networks rely on remote servers for storage, computing, and applications.

  1. **Compare physical interface and cabling types**

1\.3.a Single-Mode Fiber, Multimode Fiber, Copper

- Single-mode fiber is used for long-distance, high-bandwidth connections using a single light path.
- Multimode fiber is designed for shorter distances with multiple light paths, providing cost-effective solutions for LANs.
- Copper cables (e.g., Ethernet) are commonly used for shorter-distance connections, offering flexibility but lower speed and interference resistance compared to fiber.

1\.3.b Connections (Ethernet Shared Media and Point-to-Point)

- Ethernet shared media allows multiple devices to share the same transmission medium, potentially leading to collisions.
- Point-to-point connections directly connect two devices, offering dedicated bandwidth and lower latency.

  1. **Identify interface and cable issues (collisions, errors, mismatch duplex, and/or speed)**

- Collisions
  - Collisions occur when multiple devices transmit data simultaneously on a shared medium, causing data loss and requiring retransmission (common in half-duplex networks).

- Errors
  - Errors refer to corrupted or dropped packets caused by interference, poor-quality cables, or excessive cable lengths, impacting communication reliability.

- Duplex Mismatch
  - A duplex mismatch happens when one device operates in full-duplex and the other in half-duplex, resulting in collisions and poor network performance.

- Speed Mismatch
  - Speed mismatch occurs when connected devices are set to different speeds (e.g., 1 Gbps vs. 100 Mbps), leading to connectivity issues or degraded performance.

1. **Compare TCP to UDP**

- TCP (Transmission Control Protocol)
  - TCP is connection-oriented, ensures reliable data delivery with error checking and retransmission, and is used for applications like web browsing, email, and file transfers.

- UDP (User Datagram Protocol)
  - UDP is connectionless, prioritizes speed over reliability with no error checking or retransmission, and is ideal for real-time applications like video streaming, gaming, and VoIP.

1. **Configure and verify IPv4 addressing and subnetting**

- Configure example
  - ‘Interface GigabitEthernet 0/1’
  - ‘Ip address 192.168.1.10	 255.255.255.0’
  - ‘no shutdown’ (ensures interface is enabled if it is administratively down)

- Subnetting
  - Ip used in example above is a /24 with 256 total IP addresses.
  - 254 usable IP addresses from the range of 192.168.1.1 – 192.168.1.254

- Verify
  - ‘show ip interface brief’ : Displays interface status and assigned Ips.
  - ‘Ping <IP address>’ : Tests connectivity to another host.
  - ‘Traceroute <IP address>’ : Identifies the path packets take to the destination

1. **Describe private IPv4 addressing**

- Private IPv4 addressing refers to a set of IP addresses that are reserved for use within private networks, not routed on the public internet. These addresses are used to create local networks while conserving the global IPv4 address space.

- Private IPv4 Address Ranges:
  - Class A: 10.0.0.0 – 10.255.255.255.255
  - Class B: 172.16.0.0 – 172.31.255.255
  - Class C: 192.168.0.0 – 192.168.255.255




1. **Configure and verify IPv6 addressing and prefix**

- Configure example
  - ‘Interface GigabitEthernet 0/1’
  - ‘ipv6 address 2001:db8:1::1/64
  - ‘no shutdown’ (ensures interface is enabled if it is administratively down)

1. **Describe IPv6 address and types**

1\.9.a Unicast (global, unicast local, and link local)

- Global Unicast (2000::/3): Publicly routable IPv6 addresses used on the internet.
- Unique Local (FC00::/7): Private IPv6 addresses used within organizations, similar to private IPv4.
- Link-Local (FE80::/10): Automatically assigned for communication within the same link or network segment.

1\.9.b Anycast

- Anycast (No specific prefix): A single IPv6 address assigned to multiple interfaces, with packets delivered to the nearest interface based on routing distance.

1\.9.c Multicast

- Multicast (FF00::/8): Used to send packets to multiple devices simultaneously, commonly for group communication like routing updates.

1\.9.d Modified EUI 64

- Modified EUI-64: A method of automatically generating an IPv6 interface identifier by expanding the 48-bit MAC address to 64 bits, often used in SLAAC (Stateless Address Autoconfiguration)

  1. **Verify IP parameters for Client OS (Windows, Mac OS, Linux)**

- Windows: Use ipconfig or ipconfig /all.
- Mac OS: Use ifconfig, netstat -nr, or scutil --dns.
- Linux: Use ip addr show, ip route, or check /etc/resolv.conf.

  1. **Describe wireless principles**

1\.11.a Nonoverlapping Wi-Fi channels

- Nonoverlapping Wi-Fi channels are specific frequency ranges in the 2.4 GHz and 5 GHz bands that do not interfere with each other, ensuring reduced signal overlap and better performance.
- Example: In the 2.4 GHz band, channels 1, 6, and 11 are nonoverlapping.

1\.11.b SSID

- Service Set Identifier (SSID) is the name of a Wi-Fi network that devices use to identify and connect to the wireless network.
- It can be broadcasted (visible) or hidden (invisible to users).

1\.11.c RF (Radio Frequency)

- RF refers to the wireless signals used for communication in Wi-Fi networks, typically operating in the 2.4 GHz and 5 GHz frequency bands.
- RF is susceptible to interference from physical obstacles and other devices like microwaves or Bluetooth.

1\.11.d Encryption

- Encryption secures wireless communication by encoding data, ensuring only authorized users can access it.
- Comon encryption protocols
  - WPA2: Strong encryption using AES
  - WPA3: Enhanced security with improved encryption and authentication

  1. **Explain virtualization fundamentals (server virtualization, containers, and VRFs)**

- Server Virtualization
  - Server virtualization allows multiple virtual servers (virtual machines or VMs) to run on a single physical server by using a hypervisor (e.g., VMware ESXi, Microsoft Hyper-V).
  - Benefits: Optimizes hardware usage, isolates applications, and reduces costs.

- Containers
  - Containers are lightweight, portable units that bundle an application and its dependencies to run consistently across different environments (e.g., Docker).
  - Containers share the host operating system kernel, making them faster and more efficient than traditional VMs.

- VRFs (Virtual Routing and Forwarding)
  - VRFs allow multiple routing tables to exist on the same physical router, enabling network segmentation and isolation without requiring separate hardware.
  - Commonly used in multi-tenant environments and to separate traffic (e.g., production and development networks).

1. **Describe switching concepts**

1\.13.a MAC Learning and Aging

- MAC Learning: Switches dynamically learn the MAC addresses of devices by examining the source address of incoming frames and adding them to the MAC address table.
- MAC Aging: If a MAC address is not seen in a specific time (default: 5 minutes), the switch removes it from the MAC address table to free resources.

1\.13.b Frame Switching

- Frame switching is the process by which a switch forwards Ethernet frames based on the destination MAC address.
- The switch consults the MAC address table to determine the correct port for frame delivery.

1\.13.c Frame Flooding

- Frame flooding occurs when the destination MAC address of a frame is unknown to the switch (not in the MAC address table).
- The switch sends the frame out on all ports (except the source port) to find the intended recipient.

1\.13.d MAC Address Table

- A MAC address table is a database maintained by the switch that maps MAC addresses to specific switch ports.
- It is used to make efficient forwarding decisions and avoid unnecessary flooding.

