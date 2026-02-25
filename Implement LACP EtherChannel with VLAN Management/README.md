# Lab: Implement LACP EtherChannel with VLAN Management

##  Overview
This lab focuses on configuring a robust switched network using **LACP** for link aggregation, implementing a custom **Native VLAN** for security, and securing unused ports via the **"Parking Lot"** method.

##  Topology Diagram
- **Switches:** 2 x Cisco 2960 (S1, S2)
- **Hosts:** 2 x Generic PCs (PC-A, PC-B)
- **Aggregation:** 2 x FastEthernet links (F0/1, F0/2) bundled into **Port-Channel 1**

##  VLAN Configuration Table
| VLAN | Name | Purpose | Assigned Ports |
| :--- | :--- | :--- | :--- |
| 10 | Management | Switch SVI Access | N/A |
| 20 | Clients | End-user traffic | S1:F0/6, S2:F0/18 |
| 999 | Parking_Lot | Security / Disabled Ports | All unused ports |
| 1000 | Native | Trunking Security | N/A (Tagged) |

##  Configuration Steps

### 1. Basic Device Hardening
- Disabled DNS lookup.
- Configured encrypted passwords for `Privileged EXEC`, `Console`, and `VTY` lines.
- Applied `service password-encryption`.

### 2. Security: The "Parking Lot"
All unused ports were moved to a non-routed VLAN and administratively shut down to prevent unauthorized physical access.
```ios
interface range f0/3-5, f0/7-24, g0/1-2
 switchport mode access
 switchport access vlan 999
 shutdown
