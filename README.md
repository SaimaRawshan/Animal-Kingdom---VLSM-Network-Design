# Animal-Kingdom-VLSM-Network-Design

A Cisco Packet Tracer network simulation implementing a hierarchical multi-router topology using Variable Length Subnet Masking (VLSM), static routing, RIPv2 dynamic routing, and centralized DHCP.

**Base Address:** `192.168.0.0/16`

| Router | Subnet | Hosts | Subnet Mask |
|--------|--------|-------|-------------|
| Dog | 192.168.0.0/21 | 1201 | 255.255.248.0 |
| Cat | 192.168.8.0/22 | 801 | 255.255.252.0 |
| Elephant | 192.168.12.0/25 | 101 | 255.255.255.128 |
| Lion | 192.168.12.128/28 | 8 | 255.255.255.240 |
| WAN1 (L→E) | 192.168.12.144/30 | 2 | 255.255.255.252 |
| WAN2 (E→D) | 192.168.12.148/30 | 2 | 255.255.255.252 |
| WAN3 (E→C) | 192.168.12.152/30 | 2 | 255.255.255.252 |

**Topology**

4 routers: Lion, Elephant, Dog, Cat (PT-Empty)
Elephant acts as the central hub connecting all other routers via serial WAN links
Lion subnet hosts dedicated servers: Web, Email, DNS, DHCP
Switches: Cisco 2960 (IOS 15) with Gigabit interfaces for high host-count subnets

**IP Address Table**
## 📋 IP Address Table

### Lion

| Interface | Network | Device | IP Address | Subnet Mask | Default Gateway |
|-----------|---------|--------|------------|-------------|-----------------|
| Fa2/0 | 192.168.12.128/28 | L_main_PC | 192.168.12.138 | 255.255.255.240 | 192.168.12.129 |
| | | L_PC1 | 192.168.12.139 | | |
| | | L_PC2 | 192.168.12.140 | | |
| | | DNS | 192.168.12.141 | | |
| | | DHCP | 192.168.12.142 | | |
| | | Web | 192.168.12.137 | | |
| | | Email | 192.168.12.136 | | |
| Se0/0 | 192.168.12.144/30 | Elephant Router | 192.168.12.145 | 255.255.255.252 | |

---

### Elephant

| Interface | Network | Device | IP Address | Subnet Mask | Default Gateway |
|-----------|---------|--------|------------|-------------|-----------------|
| Fa2/0 | 192.168.12.0/25 | E_main_PC | 192.168.12.13 | 255.255.255.128 | 192.168.12.1 |
| | | E_PC1 | DHCP | | |
| | | E_PC2 | DHCP | | |
| Se0/0 | 192.168.12.144/30 | Lion Router | 192.168.12.146 | 255.255.255.252 | |
| Se1/0 | 192.168.12.148/30 | Dog Router | 192.168.12.149 | 255.255.255.252 | |
| Se4/0 | 192.168.12.152/30 | Cat Router | 192.168.12.153 | 255.255.255.252 | |

---

### Dog

| Interface | Network | Device | IP Address | Subnet Mask | Default Gateway |
|-----------|---------|--------|------------|-------------|-----------------|
| Fa2/0 | 192.168.0.0/21 | D_main_PC | 192.168.0.12 | 255.255.248.0 | 192.168.0.1 |
| | | D_PC1 | DHCP | | |
| | | D_PC2 | DHCP | | |
| Se0/0 | 192.168.12.148/30 | Elephant Router | 192.168.12.150 | 255.255.255.252 | |

---

### Cat

| Interface | Network | Device | IP Address | Subnet Mask | Default Gateway |
|-----------|---------|--------|------------|-------------|-----------------|
| Fa2/0 | 192.168.8.0/22 | C_main_PC | 192.168.8.11 | 255.255.252.0 | 192.168.8.1 |
| | | C_PC1 | DHCP | | |
| | | C_PC2 | DHCP | | |
| Se1/0 | 192.168.12.152/30 | Elephant Router | 192.168.12.154 | 255.255.255.252 | |

**Features Implemented**

1. VLSM — efficient subnetting with less wasted address space
2. Static Routing — Lion routes all traffic via se0/0 to Elephant; all routers have a static route pointing to Lion's subnet (192.168.12.128/28)
3. RIPv2 Dynamic Routing — Elephant, Dog, and Cat advertise their networks via RIPv2 with no auto-summary
4. Centralized DHCP — DHCP server at 192.168.12.142; ip helper-address configured on Elephant, Dog, and Cat LAN interfaces
5. DNS & Web Server — hosted in the Lion subnet at 192.168.12.137 (www.animals.com)


**Tools Used**

Cisco Packet Tracer
PT-Empty Router
Cisco 2960 Switch (IOS 15)

**Assumptions**

- Base network address: 192.168.0.0/16
- Static IPs assigned to Lion subnet end devices and the main PC of each tribe
- All other end devices obtain addresses via DHCP
- Gigabit switch interfaces used on subnets with large host counts
