# Implement DHCPv4 - Centralized Server and Relay Agent

##  Project Overview
This project demonstrates the implementation of a centralized DHCPv4 infrastructure on Cisco IOS devices. The lab focuses on providing dynamic IP addressing across multiple subnets, including remote segments separated by a router. Key concepts include VLSM, Router-on-a-Stick (ROAS), and DHCP Relay Agent configuration.

##  Objectives
- **Subnetting:** Segmented a 192.168.1.0/24 address space into three subnets using VLSM.
- **Inter-VLAN Routing:** Configured 802.1Q encapsulation for traffic between Clients and Management VLANs.
- **DHCP Server:** Configured **R1** to manage multiple address pools and exclusions.
- **DHCP Relay:** Configured **R2** as a relay agent to forward broadcast requests from its local LAN to the server.
- **Network Security:** Secured infrastructure via encrypted passwords, SSH lines, and "Parking Lot" VLANs for unused ports.

##  Topology
The network consists of:
- **R1 (DHCP Server):** Serves Subnet A (Local) and Subnet C (Remote).
- **R2 (Relay Agent):** Routes traffic and relays DHCP requests to R1.
- **S1 (Multilayer/Access):** Handles VLAN trunks and access ports for PC-A.
- **S2 (Access):** Provides connectivity for PC-B on a flat network.



##  Addressing Scheme
| Subnet | Purpose | Network | Range | Mask |
| :--- | :--- | :--- | :--- | :--- |
| **Subnet A** | R1 Client LAN | 192.168.1.0 | .1 - .62 | /26 |
| **Subnet B** | Management | 192.168.1.64 | .65 - .94 | /27 |
| **Subnet C** | R2 Client LAN | 192.168.1.96 | .97 - .110 | /28 |

##  Key Configurations

### R1: DHCP Pool Setup
```text
ip dhcp excluded-address 192.168.1.1 192.168.1.5
ip dhcp pool R1_Client_LAN
 network 192.168.1.0 255.255.255.192
 default-router 192.168.1.1
 domain-name ccna-lab.com
