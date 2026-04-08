#Proj 

To view IP interfaces on a firewall: `show ip`

Inside config: 192.168.11.34 255.255.255.252
Outside config: 200.0.0.2 255.255.255.252

Since ICMP is disabled on the firewall by default, it requires ICMP inspection to be enabled.
### ICMP Inspection
In config mode:
```
policy-map global_policy
class inspection_default
inspect icmp
```
This tells the firewall to look at outgoing pings and make an exception in the security to let the specific return reply back in.

### Route back
ISP recognizes the firewall, but has no idea about the rest of the network.
Hence the firewall needs a route back, for any private address:
`route inside 192.168.0.0 255.255.0.0 192.168.11.33`
However, the firewall wasn't able to recognize the named interface.
```
int g1/2
nameif inside
exit
```
The `nameif inside` command sets the interface to a trusted status.

By setting the inside interface, it is important to set the `outside` interface, to block traffic from the ISP zone (not trusted).

### Connectivity
Core is able to ping the firewall.
PCs are able to ping the firewall.

### ISP-Network Bridge
Starting the the firewall's default route, for traffic destined outside of the network.
`route outside 0.0.0.0 0.0.0.0 200.0.0.1`

For the grand finale, NAT has to be enabled. Since private IPs aren't allowed on the Internet, it needs to use the Firewall's public IP has a mask to be able to surf the web.
```
object network INTERNAL_NET
subnet 192.168.0.0 255.255.0.0
nat (inside,outside) dynamic interface
```

- [!] **From a PC:** Try to ping the Firewall's Outside IP (`200.0.0.2`).
 This ping won't work because the firewall is hiding itself. The pings could be turned on with `icmp permit any outside`, but it will remain off.
- [x] **From a PC:** Try to ping the ISP's Interface IP (`200.0.0.1`).
This what really matters, since this correct ping proved that the outside interface of the firewall is actually receiving the pings, just not responding to them.
- [-] **From a PC:** Try to ping a simulated internet address like `8.8.8.8`.
Since this is a simulation and the ISP router does not actually know where `8.8.8.8` is, it requires to be configured as a loopback interface. This can then be used to test the configuration of the [[DNS]] server.


