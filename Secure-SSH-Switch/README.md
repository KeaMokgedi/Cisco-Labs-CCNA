# Cisco Switch Security: Configuring SSH and Password Encryption

This project demonstrates the transition from insecure Telnet management to secure SSH (Secure Shell) on a Cisco 2960 Switch using Packet Tracer.

##  Objectives
* Secure device passwords using Cisco IOS encryption.
* Generate RSA keys for data encryption.
* Disable Telnet and enforce SSH-only remote access.
* Configure local database authentication for administrative users.

##  Topology & Addressing
| Device | Interface | IP Address  | Subnet Mask   |
|--------|-----------|-------------|---------------|
| S1     | VLAN 1    | 10.10.10.2  | 255.255.255.0 |
| PC1    | NIC       | 10.10.10.10 | 255.255.255.0 |



##  Key Configurations Applied

### 1. Password Encryption
Enabled global encryption to prevent passwords from being viewed in plain text within the running configuration.
```bash
S1(config)# service password-encryption
