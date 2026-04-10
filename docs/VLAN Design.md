#Proj 
Created the following VLANs
- vlan 10 name ADMIN
- vlan 20 name IT
- vlan 30 name STUDENTS
- vlan 40 name GUEST
- vlan 50 name SERVERS
- vlan 99 name MGMT
- extra - vlan 60 WIRELESS

All of these vlans have been configured onto the switches, with the exception of the server switch.
This switch was only configured with vlans 50 and 99.

### Access Switch 1
3 interfaces with VLAN 30.
```
switchport mode access # set interface to access mode
switchport access vlan 30
```
### Access Switch 2
1 interface with VLAN 10
1 interface with VLAN 20

### Access Switch 3
2 interfaces with VLAN 10
1 interface with VLAN 20

### Access Switch 4
4 interfaces with VLAN 30

### Access Switch 5
3 interfaces with VLAN 10

### VLAN Segmentation
Required /23 block
192.168.10.0/23 - also leaves space for expansion

| VLAN | Name     | Nº Devices | Mask | Hosts | Subnet            | Gateway |
| ---- | -------- | ---------- | ---- | ----- | ----------------- | ------- |
| 10   | ADMIN    | ~30        | .224 | 30    | 192.168.10.192/27 | .193    |
| 20   | IT       | ~25        | .224 | 30    | 192.168.10.224/27 | .225    |
| 30   | STUDENTS | ~100       | .128 | 126   | 192.168.10.0/25   | .1      |
| 40   | GUEST    | ~50        | .192 | 62    | 192.168.10.128/26 | .129    |
| 50   | SERVERS  | ~10        | .240 | 14    | 192.168.11.0/28   | .1      |
| 99   | MGMT     | ~10        | .240 | 14    | 192.168.11.16/28  | .17     |
| 60   | WIRELESS | ~50        | .192 | 62    | 192.168.11.128/26 | .129    |

