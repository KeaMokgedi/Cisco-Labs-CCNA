# Dual-Stack Router Configuration & Connectivity Lab

A technical implementation of a hybrid IPv4 and IPv6 network environment using Cisco IOS. This project demonstrates the ability to configure router interfaces, manage subnets with varying prefix lengths, and verify end-to-end connectivity across multiple LANs.

## 🌐 Network Topology


## 🛠️ Technical Objectives
* **IPv4 Implementation:** Configured Classless Inter-Domain Routing (CIDR) using `/25` masks on R1.
* **IPv6 Implementation:** Deployed global unicast and link-local addressing on R2.
* **Gateway Management:** Established default gateways for PC hosts to enable inter-network communication.
* **Verification:** Performed ICMP connectivity tests (Ping) to validate the dual-stack routing table.

## 📋 Addressing Schema

### IPv4 (R1)
| Interface | IP Address | Subnet Mask | Description |
| :--- | :--- | :--- | :--- |
| G0/0 | 172.16.20.1 | 255.255.255.128 | LAN 1 Gateway |
| G0/1 | 172.16.20.129 | 255.255.255.128 | LAN 2 Gateway |

### IPv6 (R2)
| Interface | IPv6 Address | Link-Local |
| :--- | :--- | :--- |
| G0/0 | 2001:db8:c0de:12::1/64 | fe80::2 |
| G0/1 | 2001:db8:c0de:13::1/64 | fe80::2 |

## 🚀 Key Configurations

### Enabling IPv6 Routing
Crucial step to allow R2 to forward IPv6 packets:
```bash
R2(config)# ipv6 unicast-routing
