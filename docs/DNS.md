#Proj 

### ISP-simulated 8.8.8.8
Since the router does not actually know the DNS server, it has to be set as a loopback interface.
```
conf t
int loopback 0
ip address 8.8.8.8 255.255.255.255
no shut
```

Having a simulated DNS and DNS server's IP included on the DHCP assignments. 
The DNS server can now have it's DNS functionality turned on and configured.
![[Pasted image 20260408065757.png]]

### Testing
Successful pings from PC to `8.8.8.8` and `google.com`
![[Pasted image 20260408065939.png]]

