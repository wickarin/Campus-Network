#Proj 
Now to configure the uplinks. These interfaces must be trunk ports, not access.

- [!] To view switchport mode of an interface `show interfaces *INT* switchport`


```
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
```
Dist 1 - Core
Dist 1 - Dist 2
Dist 1 - Acc Switches
Dist 1 - Server Switch

Dist 2 - Core
Dist 2 - Dist 1
Dist 2 - Acc Switches
Dist 2 - Server Switch

Server Switch - Dist 1
Server Switch - Dist 2

Core - Dist 1
Core - Dist 2

Acc Switches - Dist 1
Acc Switches - Dist 2



