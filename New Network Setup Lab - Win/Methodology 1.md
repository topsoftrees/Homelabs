Creating two more networks will divide the network into 3. Here is the process:

Configure FW:
1.	Configure DHCP on WAN and enable the interface
    a.	FW: Interfaces > Assignments > WAN  
    b.	Enable > Enable DHCP 
2.	Configure LAN on 192.168.50.0/24 
    a.	FW: Interfaces > Assignments > LAN 
    b.	Static IP: 192.168.50.0/24 with range 192.168.50.10-192.168.50.254
3.	Set pfSense as 192.168.50.1 static
    a.	Terminal > “sudo dhclient -r” to release the current IP > “sudo dhclient” to pull the static IP
4.	Set NSM as 192.168.50.2 static
    a.	Terminal > “sudo dhclient -r” to release the current IP > “sudo dhclient” to pull the static IP
5.	Configure OPT on 192.168.100.0/24
    a.	FW: Interfaces > Assignments > OPT 
    b.	Static IP: 192.168.100.0/24 with range 192.168.100.10-192.168.100.254
6.	Set router to 192.168.100.1 static
    a.	Terminal > “sudo dhclient -r” to release the current IP > “sudo dhclient” to pull the static IP

Configure R1:
1.	Connect FW to WAN
2.	Set a static WAN IP of 192.168.100.1
    a. 	R1: Network > WAN > Static > 192.168.100.1
3.	Set a static LAN IP of 192.168.150.1 (just as a test – WAN and LAN IPs cannot be in the same subnet)
    a.	R1: Network > LAN > Static > 192.168.150.1
4.	Wired connect to LAN
5.	Wirelessly connect. 

Test cases:
1.	OPT-WAN, wired
2.	OPT-WAN, wireless 
3.	OPT-LAN, wired
4.	OPT-LAN, wireless 

