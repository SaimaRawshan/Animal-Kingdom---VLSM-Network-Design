# Animal-Kingdom---VLSM-Network-Design

A Cisco Packet Tracer network simulation implementing a hierarchical multi-router topology using Variable Length Subnet Masking (VLSM), static routing, RIPv2 dynamic routing, and centralized DHCP.

**Network Overview**
Base Address: 192.168.0.0/16
RouterSubnetHostsSubnet MaskDog192.168.0.0/211201255.255.248.0Cat192.168.8.0/22801255.255.252.0Elephant192.168.12.0/25101255.255.255.128Lion192.168.12.128/288255.255.255.240WAN1 (L→E)192.168.12.144/302255.255.255.252WAN2 (E→D)192.168.12.148/302255.255.255.252WAN3 (E→C)192.168.12.152/302255.255.255.252

**Topology**

4 routers: Lion, Elephant, Dog, Cat (PT-Empty)
Elephant acts as the central hub connecting all other routers via serial WAN links
Lion subnet hosts dedicated servers: Web, Email, DNS, DHCP
Switches: Cisco 2960 (IOS 15) with Gigabit interfaces for high host-count subnets

**Features Implemented**

VLSM — efficient subnetting with no wasted address space
Static Routing — Lion routes all traffic via se0/0 to Elephant; all routers have a static route pointing to Lion's subnet (192.168.12.128/28)
RIPv2 Dynamic Routing — Elephant, Dog, and Cat advertise their networks via RIPv2 with no auto-summary
Centralized DHCP — DHCP server at 192.168.12.142; ip helper-address configured on Elephant, Dog, and Cat LAN interfaces
DNS & Web Server — hosted in the Lion subnet at 192.168.12.137 (www.animals.com)


**Tools Used**

Cisco Packet Tracer
PT-Empty Router
Cisco 2960 Switch (IOS 15)

**Assumptions**

Base network address: 192.168.0.0/16
Static IPs assigned to Lion subnet end devices and the main PC of each tribe
All other end devices obtain addresses via DHCP
Gigabit switch interfaces used on subnets with large host counts
