
# Implement VLANs and Trunking (Cisco Packet Tracer)

## Project Overview
This project demonstrates the implementation of VLAN segmentation, Voice VLANs, and trunking (both Static and Dynamic) across a multi-switch network. 

## Topology
The network consists of three Cisco 2960 switches (SWA, SWB, SWC) and 7 end devices.
- **VLAN 10**: Admin
- **VLAN 20**: Accounts
- **VLAN 30**: HR
- **VLAN 40**: Voice
- **VLAN 99**: Management
- **VLAN 100**: Native

## Key Configurations Implemented
### 1. VLAN Assignment
- Configured VLAN database on all switches.
- Assigned access ports for departmental isolation.
- Configured a **Voice VLAN** on SWC Fa0/4 to support both data and voice traffic.

### 2. Trunking & DTP
- **Static Trunking**: Configured between SWA and SWB using `switchport mode trunk` and `nonegotiate`.
- **Dynamic Trunking (DTP)**: Configured between SWA (Dynamic Desirable) and SWC (Dynamic Auto).
- **Security**: Moved the Native VLAN from 1 to 100 on all trunk links to mitigate VLAN hopping.

### 3. Management
- SVI (Switch Virtual Interface) configured on VLAN 99 for all switches for remote management capabilities.

## Verification Commands
- `show vlan brief`: To verify port-to-VLAN mapping.
- `show interfaces trunk`: To verify trunking status and native VLAN matching.
- `show interfaces fa0/4 switchport`: To verify Voice VLAN functionality.

## How to Run
1. Download the `.pkt` file.
2. Open with **Cisco Packet Tracer**.
3. Use the CLI on any switch to inspect configurations.
