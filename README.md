# Campus-Network
Campus like network, set in a three-tier hierarchy.
![[img/Pasted image 20260325072415.png]]

DHCP, FTP, DNS and Web server implementations.
Network is separated throughout three buildings: Data center, Building A, Building B.
Includes a Firewall, STP and OSPF routing.
VLAN segmentation and subnetting done with a focus on IP address use efficiency, but also a logical separation:
- VLAN 10 - Admin (departments)
- VLAN 20 - IT
- VLAN 30 - Students
- VLAN 40 - Guests
- VLAN 50 - Servers
- VLAN 99 - MGMT (In-Band)