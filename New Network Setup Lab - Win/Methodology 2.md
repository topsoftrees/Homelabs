I didn’t start from the very beginning, but all the steps are included. Here is the process to create only two networks (internal and guest):
1.	Restart Modem

FW:

2.	Create FW static 192.168.50.1

    a.	Interfaces > Assignments > LAN 
    
    b.	Static IP > 192.168.50.1
    
3.	Connect NSM to FW over LAN

4.	Create NSM static 192.168.50.2

    a.	Status > DHCP Leases > White ‘+’ next the the IP > Fill in static info
    
5.	Create SW static 192.168.100.1

    a.	Interfaces > Assignments > OPT 
    
    b.	Static IP > 192.168.100.1
    
6.	Create a SW VLAN

    a.	Interfaces > Assignments > VLANS > 100 
    
    b.	Interfaces > Assignments > Add VLAN 100 to OPT
    
    c.	Firewall > Rules > OPT > Pass Any

SW: 

7.	Connect SW to FW over OPT

8.	Log into SW at 192.168.100.1

9.	Set up 100 VLAN and tag all ports.

    a.	L2 Features > VLAN > ‘+’ > 100 VLAN and tag all ports
