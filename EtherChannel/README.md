# Lab: Implement EtherChannel (Cisco Packet Tracer)

##  Overview
This lab demonstrates the implementation of link aggregation using **PAgP** (Cisco Proprietary) and **LACP** (IEEE Standard) to increase bandwidth and provide redundancy in a switched network.

##  Topology & Connections
The network consists of three Cisco 2960 switches connected in a triangle.

| Channel Group | Switches | Protocol | Member Ports | Mode (Initiator/Responder) |
| :--- | :--- | :--- | :--- | :--- |
| **Po1** | SWA - SWB | PAgP | G0/1, G0/2 | Desirable / Desirable |
| **Po2** | SWA - SWC | LACP | F0/21, F0/22 | Active / Active |
| **Po3** | SWB - SWC | LACP | F0/23, F0/24 | Passive / Active |

##  Key Configurations

### 1. Trunking
Before bundling, all physical interfaces were set to static trunking:
```ios
interface range <interface-id>
 switchport mode trunk
