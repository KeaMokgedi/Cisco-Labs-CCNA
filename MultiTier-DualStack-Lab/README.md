<img width="504" height="435" alt="Screenshot 2026-05-28 001057" src="https://github.com/user-attachments/assets/55229a19-578a-43aa-92a4-938cccd4dc64" />

# Multi-Tier Hierarchical Network Design & Dual-Stack IPv6 Integration

## Project Overview
This repository contains a comprehensive implementation of a unified network topology designed in **Cisco Packet Tracer**. The project synthesizes practical network engineering designs—incorporating multi-tier hierarchical models (Three-Tier and Two-Tier architectures)—with a complete functional migration and comparative analysis between legacy **IPv4** networks and modern **Dual-Stack IPv6** frameworks. 

The lab showcases foundational modern networking concepts, including Stateless Address Autoconfiguration (**SLAAC**), Switch Database Management (**SDM**) hardware profiling adjustments, and structural enterprise redundancy patterns across various operational scales (Branch Office, Campus Core, and Spine-Leaf Data Centers).

## Hardware & Network Manifest

### Device & Topology Architecture
The simulated environment structures components into distinct architectural layers designed to isolate broadcast domains while ensuring reliable transit routing:

<img width="555" height="483" alt="Screenshot 2026-05-28 001349" src="https://github.com/user-attachments/assets/d7e4300a-be58-4d8a-8826-7330fc3486b0" />

### Physical Wiring Matrix
| Source Device | Source Port | Destination Device | Destination Port | Cable Type |
| :--- | :--- | :--- | :--- | :--- |
| **Core-R1** | `Gi0/0/0` | **Dist-Dist1** | `Gi0/1` | Copper Straight-Through |
| **Dist-Dist1** | `Gi0/2` | **Access-Branch1** | `Gi0/1` | Copper Straight-Through (Trunk) |
| **Dist-Dist1** | `Fa0/24` | **IPv4-Server** | `Fa0/0` | Copper Straight-Through |
| **Access-Branch1** | `Fa0/1` | **PC-Static** | `Fa0/0` | Copper Straight-Through |
| **Access-Branch1** | `Fa0/2` | **PC-SLAAC** | `Fa0/0` | Copper Straight-Through |

---

## Configuration Scripts

### 1. Core Router Configuration (`Core-R1`)
```vlan
enable
configure terminal
hostname Core-R1

! Enable global IPv6 unicast routing
ipv6 unicast-routing

! Configure point-to-point transit link to L3 Switch
interface GigabitEthernet0/0/0
 description Link_to_Core_Distribution
 ip address 10.0.0.1 255.255.255.252
 ipv6 address 2001:db8:aced::1/64
 ipv6 enable
 no shutdown
exit
```

### 2. Distribution Layer 3 Switch Configuration (Dist-Dist1)
```
enable
configure terminal
hostname Dist-Dist1

! Adjust hardware SDM template resource profile for Dual-Stack routing
sdm prefer dual-ipv4-and-ipv6 routing
exit
write memory
! Note: Device reload required in Packet Tracer after saving configuration!

enable
configure terminal
ip routing
ipv6 unicast-routing

! Configure physical routed uplink to Core Gateway
interface GigabitEthernet0/1
 description Link_to_Core-R1
 no switchport
 ip address 10.0.0.2 255.255.255.252
 ipv6 address 2001:db8:aced::2/64
 no shutdown
exit

! Configure trunking connection down to Access Layer
interface GigabitEthernet0/2
 description Link_to_Access-Branch1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
exit

! Configure local server access delivery port
interface FastEthernet0/24
 description Link_to_IPv4-Server
 switchport mode access
 switchport access vlan 1
 no shutdown
exit

! Configure Switch Virtual Interface (SVI) Gateway for Client LAN
interface Vlan1
 ip address 192.168.1.1 255.255.255.0
 ipv6 address 2001:db8:1::1/64
 ipv6 enable
 no shutdown
exit
```

### 3. Access Layer Switch Configuration (Access-Branch1)
```
enable
configure terminal
hostname Access-Branch1

interface GigabitEthernet0/1
 description Uplink_to_Dist-Dist1
 switchport mode trunk
 no shutdown
exit

interface range FastEthernet0/1-2
 switchport mode access
 switchport access vlan 1
 no shutdown
exit
```

## Verification & Operational Testing
Status Commands
Run the following verification sequences on the edge routing nodes to confirm protocol initialization:

```
show ipv6 interface brief
show ipv6 interface vlan 1
```

## Expected End-Device Behavior
  PC-Static: Manually provisioned with IP 2001:db8:1::100/64 targets the default gateway address 2001:db8:1::1.

  PC-SLAAC: When configured to Automatic/SLAAC, the interface dynamically processes Router Advertisements (RAs) emitted from Dist-Dist1, assigning itself a distinct EUI-64 global unicast coordinate seamlessly.
