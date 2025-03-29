<h1>Enterprise Multi-Site Network Design with OSPF, VLAN Segmentation, HSRP and Security-ACLs </h1>

<h2>Description</h2>


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
