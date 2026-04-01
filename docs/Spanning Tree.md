#Proj 
2 commands to attribute priority to Core and Dist 1 switches
Dist1: `spanning-tree vlan 1,10,20,30,40,50,99 priority 4096`
Dist2: `spanning-tree vlan 1,10,20,30,40,50,99 priority 8192`

Core becomes Root bridge, and Dist1 becomes secondary root

### Implementing Load Balancing
Standby configurations are not found
STP is giving quite a lot of troubles
Will require reconfiguration of Standy-active

First set the spanning tree mode: pvst
`spanning-tree mode pvst`

Dist1:
```
VLAN 10:
ip address 192.168.10.194 255.255.255.224
standby 10 ip 192.168.10.193
standby 10 priority 110
standby 10 preempt

VLAN 20:
ip address 192.168.10.226 255.255.255.224
standby 20 ip 192.168.10.225
standby 20 priority 110
standby 20 preempt

VLAN 30
ip address 192.168.10.2 255.255.255.128
standby 30 ip 192.168.10.1
%default priority of 100, and no preempt

VLAN 40
ip address 192.168.10.130 255.255.255.192
standby 40 ip 192.168.10.129

VLAN 50
ip address 192.168.11.2 255.255.255.240
standby 50 ip 192.168.11.1

VLAN 99
ip address 192.168.11.18 255.255.255.240
standby 99 ip 192.168.11.17
```
Dist2:
```
VLAN 10:
ip address 192.168.10.195 255.255.255.224
standby 10 ip 192.168.10.193
%default priority of 100, and no preempt

VLAN 20:
ip address 192.168.10.227 255.255.255.224
standby 20 ip 192.168.10.225

VLAN 30
ip address 192.168.10.3 255.255.255.128
standby 30 ip 192.168.10.1
standby 30 priority 110
standby 30 preempt

VLAN 40
ip address 192.168.10.131 255.255.255.192
standby 40 ip 192.168.10.129
standby 40 priority 110
standby 40 preempt

VLAN 50
ip address 192.168.11.3 255.255.255.240
standby 50 ip 192.168.11.1

VLAN 99
ip address 192.168.11.19 255.255.255.240
standby 99 ip 192.168.11.17
standby 99 priority 110
standby 99 preempt
```

`show ip interface brief`
`show running-config | begin interface Vlan10`
Vlan10 - names that show on the first command

### Important troubleshooting commands
`show standby brief`
`show spanning-tree summary`
![[Pasted image 20260401204806.png]]
This the result for Dist2 switch. Analyzing this image, it shows that the STP root for MGMT vlan is Dist 2, while for HSRP Dist 1 is the active. 
This causes a 'Tromboning' issue. 
Since Dist 2 is already Root for STP, just have to change it's HSRP priority to fix it.

