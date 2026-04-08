#Proj

Very initial topology:
![[Pasted image 20260319072701.png]]

Physical section hasn't been setup yet.


Next step on topology
![[Pasted image 20260320084247.png]]
Initial physical allocation of devices
Three buildings - Data center, Building A, Building B
### Data Center
ISP router
Firewall
Core and distribution switches
Server farm + Server switch
#### Physical setup
![[Pasted image 20260320084736.png]]


- [!] Important note on cables
> Cross-over cables - connecting between devices that operate at the same OSI layer
> Straight-through - connecting between devices that operate at different OSI layers


- [x] Cable management
- [x] Initial top of hierarchy configuration
- [x] Connection to bottom layer (building A and B)


End device connection.
IDF mounting per building.
Change of topology for added redundancy on the connection between distribution switches and access switches.
- [!] Remember: For connection between buildings - Strong trunk uplink (fiber, preferably); direct connection between distribution switches and access.
This added redundancy between the layers requires the implementation of either STP or EtherChannel
> STP - will block one path, and keep backup ready - Active Passive

> EtherChannel - combine links using both actively - Active Active

- [!] VLAN 99 - MGMT -> this is a VLAN which purpose is to give out an IP for networking devices (switches, routers...) to be able to SSH into them and configure them. It is In-Band Management, and not Out-of-Band Management.

- [x] [[VLAN Design]]
- [x] [[Trunk Links]]
- [x] [[Spanning Tree]]
- [x] [[Inter-Vlan Routing]]
- [x] [[DHCP config]]
- [x] [[Default Route & ACLs]]
- [x] [[OSPF]]
- [ ] [[Firewall Configuration]]
- [ ] [[DNS]]

### Testing
Troubleshooting for VLAN: `show run | section interface Vlan`

Disconnected Dist1 Switch for failover testing:
![[Pasted image 20260330061021.png]]
Dist 2 switch took over and resumed services as regular
DHCP renew works in order
Students are denied to access IT or ADMIN vlans
Inter-vlan connectivity is working

[[Updating Layout]]
