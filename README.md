# Cisco Basic Switch Configuration Lab

## Project Overview
In this lab, I configured a Cisco Catalyst switch with basic security and management settings. This includes setting up a Switch Virtual Interface (SVI) on a custom management VLAN.

## Key Configurations Applied
* *Hostname:* S1
* *Management VLAN:* VLAN 99 (IP: 192.168.1.2)
* *Security:* Encrypted passwords and MOTD banner.
* *Access:* Configured Console and VTY lines for remote management.

## Verification
I verified the configuration using show vlan brief to ensure all user ports were moved to the correct VLAN.
