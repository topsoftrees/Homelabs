After trying the implement the plan, these were the results:

Created the FW on 192.168.50.0/24 on LAN and 192.168.100.0/24 on OPT: 
-	Connecting FW to WAN, created a public IP and wasn’t routable. 
-	Connecting FW to LAN, created a 192.168.50.0/24 address and wasn’t routable. R1 wasn’t picking up on the separate network. pfSense DHCP Leases was pulling the hardwire connection of the NSM with a 192.168.50.2 address which I turned into a static IP.
Turned R1 into AP mode only:
-	Connected FW to WAN, created a public IP and wasn’t routable. 
-	Connected FW to LAN, no IP.
Configured R1 to be setup like an AP, but in router mode:
-	Connected FW to WAN, created a public IP and wasn’t routable. 
-	Connected FW to LAN, no IP.

Test cases:
1.	OPT-WAN, wired – fail 
2.	OPT-WAN, wireless – fail 
3.	OPT-LAN, wired – fail  
4.	OPT-LAN, wireless – fail 

Seeing a failed plan, I turned to YouTube. Pretty much the only thing I could find was connecting a switch to a router then having a dedicated AP and hardwiring devices to the switch. I was wanting to avoid creating a wireless network on the firewall, but it turns out, this type of firewall, doesn’t allow creating a wireless network without other equipment. 

Results of this: 
-	The FW will not, by default, create other networks on their physical interfaces (LAN being .50 and OPT being .100). This may need some VLAN configuration, but the routers aren’t capable of that. 
-	Connecting R1 to FW as a router or an AP in OPT, will not create a network no matter the connection.
-	Cannot create wireless network on the SG-1100 and both routers aren’t creating a functioning network. These routers are also not VLAN capable. 

Lessons Learned: 
-	Writing out the specified ports to connect to and the associated IPs. This helps with clearly seeing everything and not having to memorize the addresses. I’m learning how to create a network in this 
-	Just because devices can do certain things, it depends on the model. One of the conclusions from the previous lab was both routers couldn’t boot into pfSense and wouldn’t create two networks. With this lab, a similar situation happened. I knew my router could be turned into an AP and from the previous lab, APs could turn into guest networks. After trying to implement both using the ASUS and TP-Link routers, nothing was routable and I’m concluding it’s not possible on these routers. 
-	Implement the plan and write our lessons learned from the results. Clearly stating what I did, and its outcomes helped immensely. Turning these into lessons learned will help for later homelabs in understanding networking. 
