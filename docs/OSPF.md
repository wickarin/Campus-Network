#Proj 
In order for OSPF to be useful here, the current design has to be manipulated.
The connection between Core and Dist switches has to go from layer 2 (VLANs) to layer 3
This way OSPF will take care of routing, making it faster and scalable

All switches:
- Apply `no switchport` -> Layer 3 routed interface, still uplink
- Assign ip addresses
- Enable ip routing

Dist switches:
- Move VLANs from Core to these
- This change forces the gateways to be on Dist switches, which in turn could force a problem because both switches try to act as a gateway
HSRP - Hot Standby Router Protocol
- HSRP takes a passive-active role, while creating a virtual IP. The access switches will never speak directly to the upper layer.
- They will direct traffic towards the virtual IP, and the Dist switches will manage to load balance the traffic.
- Both switches will have all VLANs, but Virtual IP priority will change per vlan.



Core:
Use 2 /30 for direct connection between the switches
```
int g1/1/2
no switchport
ip address 10.0.0.1 255.255.255.252
int g1/1/3
no switchport
ip address 10.0.0.5 255.255.255.252
exit
ip routing
```

Dist 1:
```
int g1/1/1
no switchport
ip address 10.0.0.2 255.255.255.252
exit
ip routing
```

Dist 2:
```
int g1/1/1
no switchport
ip address 10.0.0.6 255.255.255.252
exit
ip routing
```

## HSRP
>The following IOS commands are available :
>- standby  <0-4095> ip : Enable HSRP and set the virtual IP address
>- standby  <0-4095> preempt : Overthrow lower priority Active routers
>- standby  <0-4095> priority :Priority level
>- standby  <0-4095> timers :Hello and hold timers
>- standby  <0-4095> track :Priority Tracking
>- standby version <1-2>  :HSRP version

Dist 1:
```
int vlan 10
ip address 192.168.10.194 255.255.255.224
standby 10 ip 192.168.10.193
standby 10 priority 110
standby 10 preempt

int vlan 20
ip address 192.168.10.226 255.255.255.224
standby 20 ip 192.168.10.225
standby 20 priority 110
standby 20 preempt

int vlan 30
ip address 192.168.10.2 255.255.255.128
standby 30 ip 192.168.10.1
standby 30 priority 110
standby 30 preempt

int vlan 40
ip address 192.168.10.130 255.255.255.192
standby 40 ip 192.168.10.129
%default priority of 100, and no preempt

int vlan 50
ip address 192.168.11.2 255.255.255.240
standby 50 ip 192.168.11.1
%default priority of 100, and no preempt

int vlan 99
ip address 192.168.11.18 255.255.255.240
standby 99 ip 192.168.11.17
%default priority of 100, and no preempt

```

Dist 2:
```
int vlan 10
ip address 192.168.10.195 255.255.255.224
standby 10 ip 192.168.10.193

int vlan 20
ip address 192.168.10.227 255.255.255.224
standby 20 ip 192.168.10.225

int vlan 30
ip address 192.168.10.3 255.255.255.128
standby 30 ip 192.168.10.1

int vlan 40
ip address 192.168.10.131 255.255.255.192
standby 40 ip 192.168.10.129
standby 40 priority 110
standby 40 preempt

int vlan 50
ip address 192.168.11.3 255.255.255.240
standby 50 ip 192.168.11.1
standby 50 priority 110
standby 50 preempt

int vlan 99
ip address 192.168.11.19 255.255.255.240
standby 99 ip 192.168.11.17
standby 99 priority 110
standby 99 preempt
```

If DUPP_ADDR occurs: shutdown interface, check ARP table on switches

As for OSPF implementation.
Core:
```
router ospf 1
network 10.0.0.0 0.0.0.255 area 0
default-information originate
```

Dist 1 & Dist 2:
```
router ospf 1
network 10.0.0.0 0.0.0.255 area 0
network 192.168.10.192 0.0.0.31 area 0 ! VLAN 10
network 192.168.10.224 0.0.0.31 area 0 ! VLAN 20
network 192.168.10.0 0.0.0.127 area 0  ! VLAN 30
network 192.168.10.128 0.0.0.63 area 0 ! VLAN 40
network 192.168.11.0 0.0.0.15 area 0   ! VLAN 50
network 192.168.11.16 0.0.0.15 area 0  ! VLAN 99
```

Troubleshooting commands:
`show ip ospf neighbor`
`show ip route`

