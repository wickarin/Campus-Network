#Proj 
2 commands to attribute priority to Core and Dist 1 switches
Core: `spanning-tree vlan 1,10,20,30,40,50,99 priority 4096`
Dist1: `spanning-tree vlan 1,10,20,30,40,50,99 priority 8192`

Core becomes Root bridge, and Dist1 becomes secondary root