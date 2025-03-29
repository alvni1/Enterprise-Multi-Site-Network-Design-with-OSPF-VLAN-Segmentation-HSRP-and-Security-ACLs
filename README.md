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
Here is an overview of the network. 

![Screenshot 2025-03-29 012956](https://github.com/user-attachments/assets/f0c7008e-3e02-4bff-83d4-27e051a968d5)
