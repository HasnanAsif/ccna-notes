# **3.0 IP Connectivity**

## **3.1 Interpret the components of routing table**

The routing table contains critical information that routers use to determine the best path for forwarding packets to their destinations. Each component of the routing table has a specific role:

#### 3\.1.a Routing Protocol Code

- Definition: A code that identifies the source of the route (how the route was learned)
- Examples:
  - C – Connected (directly connected interfaces)
  - S – Static (manually configured route)
  - R – RIP (Routing Information Protocol)
  - O – OSPF (Open Shortest Path First)
  - D – EIGRP (Enhanced Interior Gateway Routing Protocol)
  - B – BGP (Border Gateway Protocol)

#### 3\.1.b Prefix

- Definition: The network address or destination IP address range to which the route applies.
- Example: 192.168.1.0 in a route like 192.168.1.0/24.

#### 3\.1.c Network Mask

- Definition: Indicates the subnet mask associated with the prefix, defining the range of IP addresses within the route.
- Example: 
  - /24 corresponds to 255.255.255.0.
  - In 192.168.1.0/24, the /24 defines the first 24 bits as the network portion.

#### 3\.1.d Next Hop

- Definition: The IP address of the next router or device that the packet should be forwarded to, on its path to the destination.
- Example: 10.1.1.1 in a route like:
  192.168.2.0/24 via 10.1.1.1.

#### 3\.1.e Administrative Distance (AD)

- Definition: A value that indicates the trustworthiness of a route, with lower values being preferred.
- Examples:
  - Directly Connected: AD = 0
  - Static Route: AD = 1
  - EIGRP (internal): AD = 90
  - OSPF: AD = 110
- Key Point: AD is used to choose between multiple routes learned via different protocols.

#### 3\.1.f Metric

- Definition: A value used to determine the best path to a destination when multiple routes exist for the same network. Lower metrics are preferred.
- Examples:
  - RIP uses hop count.
  - OSPF uses cost (based on bandwidth).
  - EIGRP uses a composite metric (bandwidth, delay, etc.).

#### 3\.1.g Gateway of Last Resort

- Definition: The router or next-hop IP address where packets destined for unknown or non-matching networks are forwarded (default route).
- Example:
  - 0.0.0.0/0 is commonly used as the prefix for a default route.
  - A route like S\* 0.0.0.0/0 via 192.168.1.1 indicates that traffic for all unknown networks goes to 192.168.1.1.

## **3.2 Determine how a router makes a forwarding decision by default**

When a router receives a packet, it makes a forwarding decision based on three key factors in order:

#### 3\.2a Longest Prefix Match

- Definition: The router searches its routing table and selects the route with the most specific (longest) subnet mask that matches the destination IP address.
- Why it matters: A longer prefix provides a more precise match for the destination.

#### 3\.2.b Administrative Distance (AD)

- Definition: Administrative Distance is a value that determines the trustworthiness of a route when multiple routes to the same destination exist but are learned from different routing protocols.
- Lower AD values are preferred

#### 3\.2.c Routing Protocol Metric

- Definition: If multiple routes to the same destination exist from the same routing protocol, the router uses the metric to determine the best path. The route with the lowest metric is chosen.
- How Metrics are Calculated:
  - RIP: Hop count (fewer hops = better route).
  - OSPF: Cost (based on bandwidth; higher bandwidth = lower cost).
  - EIGRP: Composite metric (bandwidth, delay, reliability, etc.).

Order of Router Forwarding Decision

- Longest Prefix Match – Find the most specific route in the routing table.
- Administrative Distance – Choose the route with the lowest AD if multiple routes exist.
- Routing Protocol Metric – Select the route with the lowest metric when routes are from the same protocol.

## **3.3 Configure and verify IPv4 and IPv6 static routing**

Static routes allow network administrators to manually specify paths for traffic. Below are the key types of static routes and their configurations.

#### 3\.3a Default Route

- A default route acts as a "catch-all" for any destination that doesn't match a specific route in the routing table.

- IPv4 Configuration: 
  - `ip route 0.0.0.0 0.0.0.0 (next-hop-IP) or (exit-interface)`
  - Example: `ip route 0.0.0.0 0.0.0.0 192.168.1.1`

- IPv6 Configuration: 
  - `ipv6 route ::/0 (next-hop-IP)`
  - Example: `ipv6 route ::/0 2001:db8::1`

#### 3\.3b Network Route

- A network route specifies a route to an entire subnet or network.
- IPv4 Configuration

  - `ip route (destination-network) (subnet-mask) (next-hop-IP) or (exit-interface)`
  - Example: `ip route 192.168.10.0 255.255.255.0 192.168.1.1`

- IPv6 Configuration
  - `ipv6 route (destination-prefix) (prefix-length) (next-hop-IP)`
  - Example: `ipv6 route 2001:db8:1::/64 2001:db8::1`

#### 3\.3c Host Route

- A host route is a route to a specific IP address, often used for precise control over traffic. (/32 for IPv4 or /128 for IPv6)

- IPv4 Configuration
  - `ip route (host-IP) 255.255.255.255 (next-hop-IP)`
  - Example: `ip route 192.168.1.10 255.255.255.255 192.168.1.1`

- IPv6 Configuration
  - `ipv6 route (host-IP)/128 (next-hop-IP)`
  - Example: `ipv6 route 2001:db8:1::10/128 2001:db8::1`

#### 3\.3d Floating Static 

- A floating static route acts as a backup route and only takes effect if the primary route fails. This is achieved by assigning a higher administrative distance (AD) to the static route.

- IPv4 Configuration
  - `ip route (destination-network) (subnet-mask) (next-hop-IP) (administrative-distance)`  
  - Example: `ip route 192.168.10.0 255.255.255.0 192.168.1.1 200`

- IPv6 Configuration
  - `ipv6 route (destination-prefix) (prefix-length) (next-hop-IP) (administrative-distance)`  
  - Example: `ipv6 route 2001:db8:1::/64 2001:db8::1 200`

Verifying Static Routes

- Show routing Table
  - IPv4: `show ip route`
  - IPv6: `show ipv6 route`

- Ping Test:
  - Use `ping (destination-IP)` to confirm connectivity.

- Traceroute Test:
  - Pv4: `traceroute (destination-IP)`
  - IPv6: `traceroute ipv6 (destination-IP)`

Summary

- Default Route: Catch-all route for unspecified destinations.
- Network Route: Route to an entire network or subnet.
- Host Route: Route to a specific IP address (/32 for IPv4 or /128 for IPv6).
- Floating Static Route: Backup route with a higher administrative distance.

## **3.4 Configure and verify single area OSPFv2**

#### 3\.4.a Neighbor adjacencies

- OSPF routers establish neighbor relationships with ‘hello’ packets being communicated to each other for the purpose of exchanging routing information. Neighbor adjacency is formed when OSPF parameters (such as area ID, hello/dead timers, network type, and authentication if configured) match between routers.

- Configuration Steps
  - `router ospf (process-id)`
  - `network (ip-address) (wildcard-mask) area (area-id)`

- Configuration Example
  - `router ospf 1`
  - `network 192.168.100.10 0.0.0.255 area 1`

- Verification of Adjacencies
  - `show ip ospf neighbor`

#### 3\.4.b Point-to-point

- Point-to-point networks connect two OSPF routers directly without DR/BDR election. This is commonly used on serial links.

- Configuration Steps:
- Assign the interface to OSPF:
  - `router ospf (process-id)`
  - `network (ip-address) (wildcard-mask) area (area-id)`

- Set the network type to point-to-point
  - `interface (interface-id)`
  - `ip ospf network point-to-point`

- Verify the OSPF neighbor on the link
  - `show ip ospf neighbor`

#### 3\.4.c Broadcast (DR/BDR selection)

- Broadcast networks allow multiple OSPF routers to communicate. OSPF elects a Designated Router (DR) and Backup Designated Router (BDR) to reduce the number of adjacencies and minimize LSA flooding.

- DR/BDR Election Process:
  - The router priority (configured using ip ospf priority) determines the DR/BDR election.
  - Highest priority becomes DR, second-highest becomes BDR.
  - If priorities are equal, the highest router ID is used.

- Configuration Steps:
- Set OSPF priority (default is 1; a priority of 0 prevents DR/BDR election): 
  - `interface (interface-id)`
  - `ip ospf priority (value)`

- Assign the interface to an OSPF area
  - `router ospf (process-id)`
  - `network (ip-address) (wildcard-mask) area (area-id)`

- Verify DR/BDR
  - `show ip ospf neighbor`

#### 3\.4.d Router ID

- The Router ID uniquely identifies an OSPF router in the network. It is selected in this order:
  - Manually configured Router ID.
  - Highest IP address on a loopback interface.
  - Highest IP address on an active physical interface.

- Manually Configure Router ID:
  - `router ospf 1`
  - `router-id 1.1.1.1`

- Verify router ID:
  - `show ip ospf`

- When configuring OSPF, some changes, such as modifying the Router ID, require either restarting the OSPF process or reloading the router for the changes to take effect.
  - `clear ip ospf process`
  - `reload` (disrupts all network operations during reload)

## **3.5 Describe the purpose, functions, and concepts of first hop redundancy protocols**

- Purpose of FHRP
  - First Hop Redundancy Protocols (FHRPs) ensure gateway redundancy by providing a backup default gateway if the primary gateway fails. This improves network availability and reliability by enabling hosts to seamlessly switch to a backup router without manual reconfiguration.

- Functions of FHRP
  - Redundancy: Multiple routers act as a single virtual gateway. If the active router fails, a standby router takes over.
  - Default Gateway Failover: Hosts continue to send traffic to the same gateway IP, even during a failover.
  - High Availability: Minimizes downtime for critical services by ensuring traffic flow continuity.

- Concepts of FHRPs – There are three main FHRPs:
  - (HSRP) – Hot Standby Router Protocol
  - (GLBP) – Gateway Load Balancing Protocol
  - (VRRP) – Virtual Router Redundancy Protocol

- Hot Standby Router Protocol (HSRP)
  - Cisco proprietary protocol.
  - Uses one active router and one standby router.
  - Virtual IP address is shared between routers.
  - The active router forwards traffic, while the standby router monitors and takes over if the active router fails.
  - Hello Timer: Default 3 seconds.
  - Hold Timer: Default 10 seconds.
  - Verification command: show standby

- Virtual Router Redundancy Protocol (VRRP)
  - Open standard protocol.
  - Similar to HSRP but allows a virtual router to have a Master router and Backup routers.
  - The Master router forwards traffic, while Backup routers monitor and take over if the Master fails.
  - VRRP has faster failover compared to HSRP.
  - Verification command: show vrrp

- Gateway Load Balancing Protocol (GLBP)
  - Cisco proprietary protocol.
  - Provides both redundancy and load balancing by distributing traffic across multiple routers.
  - Uses an Active Virtual Gateway (AVG) to manage traffic distribution.
  - Each router in the group acts as an Active Virtual Forwarder (AVF) for load balancing.
  - Verification command: show glbp

|FHRP|Terminology|Multicast IP|Virtual MAC|Cisco Proprietary?|Failover|
| :- | :- | :- | :- | :- | :- |
|HSRP|Active/Standby|<p>v1: 224.0.0.2</p><p>v2: 224.0.0.102 </p>|<p>V1: 0000.0c07.acXX</p><p>V2:</p><p>0000\.0c9f.fXXX</p>|Yes|Slower|
|VRRP|Master/Backup|224\.0.0.18|0000\.5e00.01XX|No|Faster|
|GLBP|AVG/AVF|224\.0.0.102|0007\.b400.XXYY|Yes|Faster with Load Balancing|

