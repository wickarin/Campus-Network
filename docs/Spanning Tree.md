#Proj 

### Implementing Load Balancing
- [-] Standby configurations are not found
- [-] STP is giving quite a lot of troubles
- [-] Will require reconfiguration of Standy-active

First set the spanning tree mode: pvst
`spanning-tree mode pvst`
This mode is a per-VLAN STP, this is the reason why the diagram does not display amber blocking ports. On some vlans they are blocked, but not on others.

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

VLAN 60
ip address 192.168.11.130 255.255.255.192
standby 60 ip 192.168.11.129
standby 60 priority 110
standby 60 preempt 
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

VLAN 60
ip address 192.168.11.131 255.255.255.192
standby 60 ip 192.168.11.129
```

`show ip interface brief`
`show running-config | begin interface Vlan10`
Vlan10 - names that show on the first command

Proof of load balancing:
![[Pasted image 20260402060906.png]]
PC0 (VLAN 30) and PC10 (VLAN 10)

### Important troubleshooting commands
`show standby brief`
`show spanning-tree summary`
![[Pasted image 20260401204806.png]]
This the result for Dist2 switch. Analyzing this image, it shows that the STP root for MGMT vlan is Dist 2, while for HSRP Dist 1 is the active. 
This causes a 'Tromboning' issue.
Since Dist 2 is already Root for STP, just have to change it's HSRP priority to fix it.
Updated image without the 'Tromboning' issue
![[Pasted image 20260410054049.png]]

