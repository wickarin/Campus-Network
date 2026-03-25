#Proj 

Being a layer 3 switch (core) will be using SVIs.
```
int vlan 10
ip address 192.168.10.1 255.255.255.0
```

Following the planned segmentation
![[VLAN Design#VLAN Segmentation]]

- [x] VLAN 10
```
interface vlan 10
ip address 192.168.10.193 255.255.255.224
```
- [x] VLAN 20
```
interface vlan 20
ip address 192.168.10.225 255.255.255.224
```
- [x] VLAN 30
```
interface vlan 30
ip address 192.168.10.1 255.255.255.128
```
- [x] VLAN 40
```
interface vlan 40
ip address 192.168.10.129 255.255.255.192
```
- [x] VLAN 50
```
interface vlan 50
ip address 192.168.11.1 255.255.255.240
```
- [x] VLAN 99
```
interface vlan 99
ip address 192.168.11.17 255.255.255.240
```

Enable Routing: `ip routing`