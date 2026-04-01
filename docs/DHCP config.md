#Proj 
Subnet table for guidance
![[VLAN Design#VLAN Segmentation]]

![[Pasted image 20260325071452.png]]

Per VLAN (except 50 & 99):
```
int vlan VLAN
ip helper-address DHCP_SERVER_IP
```

DHCP server config: 192.168.11.6