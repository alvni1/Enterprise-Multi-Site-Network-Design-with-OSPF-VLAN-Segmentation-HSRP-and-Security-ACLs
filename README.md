<h1>Enterprise Multi-Site Network Design with OSPF, VLAN Segmentation, HSRP and Security-ACLs </h1>

<h2>Description</h2>
This network design incorporates: multi-site WAN connectivity using OSPF dynamic routing, VLAN segmentation at the headquarters for traffic separation, HSRP for first-hop redundancy in the LAN environment, DHCP for dynamic IP addressing, NAT for simulated Internet access, and ACLs for inter-VLAN and remote access security. A multi-site WAN is used when an organization has multiple locations that need to share data and services as though they were on one unified network.

- <b>OSPF automatically updates the best routes, adapting quickly to network changes with minimal manual configuration. It’s widely used in large networks for its speed, scalability, and reliability. </b>

- <b>HSRP provides redundancy for the default gateway in a LAN by allowing multiple routers to act as backups for each other. This ensures minimal downtime and continuous network access if the primary gateway fails.</b> 

- <b>DHCP automatically assigns IP addresses and other settings to devices, eliminating the need for manual configuration. This reduces errors, prevents address conflicts, and simplifies network administration.</b> 

- <b>ACLs control which traffic is allowed or denied on network devices, helping to prevent unauthorized access and improve overall security.</b> 

- <b>NAT allows multiple devices on a private network to share a single public IP address, helping conserve IPv4 addresses. It also enhances security by hiding internal IPs, reducing direct exposure of internal hosts to the internet.</b>

<h2>Things Learned/Troubleshooting</h2>

Issue 1: Routers not forming OSPF neighbors
- <b>Cause: Incorrect IP assignments on serial interfaces; same IP/subnet used for multiple links.</b> 
- <b>Fix: Reassigned correct /30 subnet IPs per WAN link; verified with show ip interface brief.</b> 

Issue 2: Router7 unable to connect to Router6 and Router9
- <b>Cause: IP addressing mismatch and missing OSPF network statements.</b> 
- <b>Fix: Corrected IPs, applied correct network commands for 10.0.0.x subnets.</b> 

Issue 3: No VLAN 10 communication; pings failed
- <b>Cause: VLAN 10 was not created on Switch4; trunk ports misconfigured.</b> 
- <b>Fix: Created VLAN 10, verified trunking with show interfaces trunk.</b> 

Issue 4: HSRP roles incorrect; Router7 remained Active for all VLANs
- <b>Cause: Trunk cable misconnection; wrong interfaces used.</b> 
- <b>Fix: Moved cables to correct ports (Router6 G0/1 ↔ Switch4 G0/1, Router7 G0/2 ↔ Switch4 G0/2); reconfigured subinterfaces.</b> 

Issue 5: ACL not blocking HR to Engineering
- <b>Cause: PC3 was not in VLAN20; had VLAN30 IP.</b> 
- <b>Fix: Reassigned PC3 to correct VLAN; verified IP and ACL hits.</b> 

<h2>Languages and Utilities Used</h2>

- <b>Cisco Command Line Interface</b> 

<h2>Environments Used </h2>

- <b>Cisco Packet Tracer</b>

<h2>Program walk-through:</h2>
1. Initial Topology Setup:

Connected routers with serial DCE/DTE cables

Configured PCs to appropriate VLANs via access ports on Switch4

Connected HQ routers to Switch4 using trunk ports 

![Screenshot 2025-03-29 012956](https://github.com/user-attachments/assets/2414c4da-5c0f-4fd1-9fad-cfef569c63dc)

2. VLAN Creation and Trunking:

vlan 10
vlan 20
vlan 30
interface range g0/1 - 2
switchport mode trunk
switchport trunk allowed vlan all

![Screenshot 2025-03-22 001353](https://github.com/user-attachments/assets/737de86b-2357-46a9-86d5-821f0c53de69)


3. Router Subinterfaces for VLANs (Router7):

interface g0/2.10
encapsulation dot1q 10
ip address 192.168.10.3 255.255.255.0

![Screenshot 2025-03-22 021225](https://github.com/user-attachments/assets/2ff09a46-0f59-4772-b9ff-b77c86ff9be6)

4. HSRP Redundancy Setup (Router6 as Active):

interface g0/1.10
ip address 192.168.10.2 255.255.255.0
standby 10 ip 192.168.10.1
standby 10 priority 110
standby 10 preempt

![Screenshot 2025-03-22 011152](https://github.com/user-attachments/assets/6394ad7d-604d-4e4d-9ee2-24eb35d54298)

5. DHCP Pools for Each VLAN:

ip dhcp pool VLAN10_POOL
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8

(Repeat for VLAN20 and VLAN30)

![Screenshot 2025-03-23 003640](https://github.com/user-attachments/assets/2d91200f-0318-4061-9e3b-cc13dd837f25)

6. OSPF Configuration (Router7):

router ospf 1
router-id 7.7.7.7
network 10.0.0.8 0.0.0.3 area 0
network 10.0.0.4 0.0.0.3 area 0
...
default-information originate

![Screenshot 2025-03-22 235725](https://github.com/user-attachments/assets/88aa7300-aa33-4385-9be0-cea68080e669)

![Screenshot 2025-03-23 001322](https://github.com/user-attachments/assets/d96b396c-87b0-42cc-a086-f34497e7c388)


7. ACL for Inter-VLAN Access Control:

ip access-list extended BLOCK_HR_TO_ENG
deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
permit ip any any
interface g0/2.20
ip access-group BLOCK_HR_TO_ENG in

![Screenshot 2025-03-23 002052](https://github.com/user-attachments/assets/3e5c5a47-c380-43a4-9c33-3a5169afb746)


