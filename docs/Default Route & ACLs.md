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

### Default Route
The Core switch knows how to reach the internet, but Dist 1 and Dist 2 do not. Instead of using static routes, the Core switch will take the following command to advertise the default route.
```
routers ospf 1
default-information originate
```

### ACL
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

Upon research, the configuration above is correct, however it does not follow best practice.
It should block closer to the source. So this configuration is moved to the Dist switches.

Testing ping from a VLAN 30 PC to the DHCP server, returns Destination host unreachable - which is exactly what's supposed to happen. For extra credibility, the 4 failed pings are caught on ACL counter on Dist 2:
![[Pasted image 20260402062520.png]]

