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

Section moved to [[Spanning Tree#Implementing Load Balancing|STP]].

If DUPP_ADDR occurs: shutdown interface, check ARP table on switches

### OSPF implementation.
Core:
```
router ospf 1
network 10.0.0.0 0.0.0.255 area 0
default-information originate
```

Dist 1
```
int g1/1/1
no switchport
ip addr 10.0.0.1 255.255.255.252
router ospf 1
network 10.0.0.0 0.0.0.3 area 0
network 192.168.10.192 0.0.0.31 area 0 ! VLAN 10
network 192.168.10.224 0.0.0.31 area 0 ! VLAN 20
network 192.168.10.0 0.0.0.127 area 0  ! VLAN 30
network 192.168.10.128 0.0.0.63 area 0 ! VLAN 40
network 192.168.11.0 0.0.0.15 area 0   ! VLAN 50
network 192.168.11.16 0.0.0.15 area 0  ! VLAN 99
network 192.168.0.0 0.0.255.255 area 0
```

Dist 2
```
int g1/1/1
no switchport
ip addr 10.0.0.6 255.255.255.252
router ospf 1
network 10.0.0.4 0.0.0.3 area 0
network 192.168.10.192 0.0.0.31 area 0 ! VLAN 10
network 192.168.10.224 0.0.0.31 area 0 ! VLAN 20
network 192.168.10.0 0.0.0.127 area 0  ! VLAN 30
network 192.168.10.128 0.0.0.63 area 0 ! VLAN 40
network 192.168.11.0 0.0.0.15 area 0   ! VLAN 50
network 192.168.11.16 0.0.0.15 area 0  ! VLAN 99
network 192.168.0.0 0.0.255.255 area 0
```

Troubleshooting commands:
`show ip ospf neighbor`
`show ip route`

