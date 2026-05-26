<img width="777" height="641" alt="Screenshot 2026-05-26 121927" src="https://github.com/user-attachments/assets/1293bab0-23e3-4311-ba95-0268eda107b6" />

[ WAN LINK ]
                              172.16.1.0 / 255.255.255.252
                                      (29-bit)
                                         ||
              DCE (Clock: 128k)          ||          DTE
           [Router0] (Se0/1/0) <=========||=========> (Se0/1/0) [Router1]
                   |                                         |
           (Gi0/0/0) | 10.1.1.1                                (Gi0/0/0) | 10.2.1.1
                   |                                         |
             [ Switch0 ]                               [ Switch1 ]
         (24-Port Line-Rate)                       (24-Port Line-Rate)
        /     |     \\                             /     |     \\
       /      |      \\                           /      |      \\
  [PC0]    [PC1]    [AP0]                   [PC2]    [PC3]    [AP1]
10.1.1.2  10.1.1.3  (L1/L2)               10.2.1.2  10.2.1.3  (L1/L2)


##  Hardware Inventory & Interconnections

### 1. Module Slotting Requirement
Modern Cisco Integrated Services Routers (such as the ISR 4321) do not feature onboard legacy Serial Interfaces. Prior to cabling, physical interface modules must be manually added:
1. Click on **Router0** and **Router1** in the workspace.
2. Select the **Physical** tab and toggle the power switch to **OFF**.
3. Locate the **NIM-2T** (or **HWIC-2T**) high-speed serial interface module from the left list.
4. Drag and drop the module into an empty expansion slot.
5. Toggle the power switch back to **ON**. 
6. *Result:* This maps the physical WAN ports to the identifier **`Serial0/1/0`**.

### 2. Cable Mapping Matrix

| From Device | From Port | To Device | To Port | Cable Type | Rationale |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Router0** | Serial0/1/0 | **Router1** | Serial0/1/0 | **Serial DCE** (Red w/ Clock) | Simulates service provider clocking on Router0 (`clock rate 128000`) |
| **Router0** | GigabitEthernet0/0/0 | **Switch0** | GigabitEthernet0/1 | **Copper Straight-Through** | Layer 3 Router to Layer 2 Switch uplink |
| **Router1** | GigabitEthernet0/0/0 | **Switch1** | GigabitEthernet0/1 | **Copper Straight-Through** | Layer 3 Router to Layer 2 Switch uplink |
| **Switch0** | FastEthernet0/1 | **PC0** | FastEthernet0 | **Copper Straight-Through** | Access layer device-to-host connection |
| **Switch0** | FastEthernet0/2 | **PC1** | FastEthernet0 | **Copper Straight-Through** | Access layer device-to-host connection |
| **Switch0** | FastEthernet0/3 | **AP0** | Port 0 | **Copper Straight-Through** | Access layer to Wireless Access Point bridge |
| **Switch1** | FastEthernet0/1 | **PC2** | FastEthernet0 | **Copper Straight-Through** | Access layer device-to-host connection |
| **Switch1** | FastEthernet0/2 | **PC3** | FastEthernet0 | **Copper Straight-Through** | Access layer device-to-host connection |
| **Switch1** | FastEthernet0/3 | **AP1** | Port 0 | **Copper Straight-Through** | Access layer to Wireless Access Point bridge |

---

##  Network Configuration Scripts

###  Host & End-Device Schemes
Statically apply these parameters via the **Desktop -> IP Configuration** window on each endpoint:

#### Subnet A: Router0 Local Area Network
* **PC0 Terminal:**
  * IP Address: `10.1.1.2` | Subnet Mask: `255.255.255.0` | Default Gateway: `10.1.1.1`
* **PC1 Terminal:**
  * IP Address: `10.1.1.3` | Subnet Mask: `255.255.255.0` | Default Gateway: `10.1.1.1`

#### Subnet B: Router1 Local Area Network
* **PC2 Terminal:**
  * IP Address: `10.2.1.2` | Subnet Mask: `255.255.255.0` | Default Gateway: `10.2.1.1`
* **PC3 Terminal:**
  * IP Address: `10.2.1.3` | Subnet Mask: `255.255.255.0` | Default Gateway: `10.2.1.1`

---

###  Cisco IOS Device CLI Code
Access the **CLI** tab on each respective network device and execute the following configuration scripts:

#### Router0 (Left Gateway)
```text
enable
configure terminal

hostname Router0

! --- LAN Interface Provisioning ---
interface GigabitEthernet0/0/0
 description UPLINK_TO_SWITCH0
 ip address 10.1.1.1 255.255.255.0
 no shutdown
exit

! --- WAN Interface Provisioning ---
interface Serial0/1/0
 description WAN_LINK_TO_ROUTER1
 ip address 172.16.1.1 255.255.255.252
 clock rate 128000
 no shutdown
exit

! --- Static Route Insertion ---
! Syntax: ip route [Target_Network] [Subnet_Mask] [Next_Hop_IP]
ip route 10.2.1.0 255.255.255.0 172.16.1.2

end
write memory
