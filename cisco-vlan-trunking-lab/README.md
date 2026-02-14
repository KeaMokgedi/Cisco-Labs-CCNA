# Cisco Lab: Implementing VLANs and Trunking

## Project Overview
This project demonstrates the implementation of Virtual Local Area Networks (VLANs) and 802.1Q Trunking using Cisco Catalyst 2960 switches. The goal was to segment network traffic into logical broadcast domains to improve security, manageability, and performance.

## Network Topology
- **Switches:** 2 x Cisco Catalyst 2960
- **Hosts:** 2 x Generic PCs
- **Link:** 802.1Q Trunk between S1 and S2 on interface F0/1

### VLAN Assignments
| VLAN | Name | Purpose |
| :--- | :--- | :--- |
| 10 | Management | Remote switch administration |
| 20 | Sales | Departmental traffic for PC-A |
| 30 | Operations | Departmental traffic for PC-B |
| 999 | ParkingLot | Security: All unused ports |
| 1000 | Native | Native VLAN for trunking |

## Key Features Implemented

### 1. Logical Segmentation
Created specific VLANs to isolate department traffic. Ports were manually assigned to access mode to prevent unauthorized VLAN hopping.

### 2. 802.1Q Trunking
Configured a trunk link between S1 and S2 on `F0/1`. 
- **Native VLAN:** Set to 1000 to move untagged traffic away from the default (VLAN 1).
- **VLAN Pruning:** Restricted the trunk to only allow VLANs 10, 20, 30, and 1000 for improved security and bandwidth.

### 3. Layer 2 Security (Parking Lot)
Following industry best practices, all unused ports were:
- Assigned to a non-routable "Parking Lot" VLAN (VLAN 999).
- Administratively shut down (`shutdown`) to prevent unauthorized physical access.

### 4. Device Hardening
- Encrypted passwords using `service password-encryption`.
- Configured `enable secret` for privileged access.
- Implemented a Warning Banner (MOTD) to deter unauthorized users.

## How to Use
1. Download the `.pkt` file.
2. Open it in **Cisco Packet Tracer**.
3. Use the `show vlan brief` and `show interfaces trunk` commands to verify the configuration.

## Verification Results
- **Intra-VLAN Connectivity:** Devices in the same VLAN can communicate (once Inter-VLAN routing is added).
- **Security Constraint:** PC-A (VLAN 20) cannot ping PC-B (VLAN 30), confirming successful Layer 2 isolation.
