Equipment:
-	Netgate SG-1100 pfSense Firewall/Router
-	ASUS Router (consumer)
-	TP-Link Router (consumer)
-	NSM

Initial topology: 

<img width="765" alt="Screen Shot 2022-01-28 at 10 45 38 AM" src="https://user-images.githubusercontent.com/74877876/151577362-14bb66d1-755a-4ada-89ad-d873240d244b.png">


Firewall (SG-1100):
-	Modem will connect to WAN port with an ISP assigned IP
-	pfSense: 192.168.50.1
-	NSM: 192.168.50.2 (LAN port)
-	Hardwire NSM - monitor all wired and wireless connections
-	R1: 192.168.100.3 (OPT port)

Router (ASUS):
-	FW: 192.168.100.3 (WAN port)
-	Wired connections: 192.168.100.0/24 (LAN ports)
-	Wireless connections: 192.168.150.0/24

It might look confusing but there will be 3 networks created - 192.168.50.0/24 for FW, 192.168.100.0/24 for wired, 192.168.150.0/24 for wireless. 
