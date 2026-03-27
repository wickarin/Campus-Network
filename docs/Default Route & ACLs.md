#Proj 

The core needs a route to the firewall.
And the firewall needs a route to the internet.
The connection between the core and the firewall was done through a subnet /30 - 192.168.11.32/30
Core:
```
int g1/0/1
ip address 192.168.11.33 255.255.255.252
ip route 0.0.0.0 0.0.0.0 192.168.11.34
```
Firewall:
```
int g1/2
ip address 192.168.11.34 255.255.255.252
```

ACL has to be configured on the Core and not on the firewall.
If it were to be configured on the firewall, the traffic would have already left the internal network, and would not be efficient or practical for VLAN management.
Core:
```
access-list 100 deny ip 192.168.10.0 0.0.0.127 192.168.11.0 0.0.0.15
access-list 100 deny ip 192.168.10.0 0.0.0.127 192.168.11.0 0.0.0.15
int vlan 30
ip access-group 100 in
```
This blocks students from entering server space
Verification for access-lists:
`show access-lists`

