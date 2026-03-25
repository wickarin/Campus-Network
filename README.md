# Campus-Network
Campus like network, set in a three-tier hierarchy.

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

Logical Map
<img width="948" height="707" alt="Pasted image 20260325072339" src="https://github.com/user-attachments/assets/d1bc7b23-bfc2-4db1-ae9e-6eec01ddc8a7" />

Building Separation

<img width="648" height="415" alt="Pasted image 20260325072433" src="https://github.com/user-attachments/assets/c84aeafc-dc2e-4c59-821d-0f6c8c54b506" />
