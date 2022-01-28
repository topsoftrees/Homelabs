Equipment:
-	Netgate SG-1100 pfSense Firewall/Router
-	TP-Link JetStream 8-port Switch 
-	Unifi AP AC Lite 

This will be the last topology of the plan because I’m going to try to keep everything on a static IP:

<img width="678" alt="Screen Shot 2022-01-28 at 10 46 24 AM" src="https://user-images.githubusercontent.com/74877876/151577560-3131959a-61fd-40fe-8ff2-9615958c7c37.png">


FW (SG-1100):
-	Modem will connect to WAN port with an ISP assigned IP
-	pfSense: 192.168.50.1 (static)
-	SW1: 192.168.50.2 (static over LAN)


SW (TP-Link): 
-	Setup static IP 192.168.50.2 
-	NSM: 192.168.50.3 (static)
-	Hardwire NSM - monitor all wired and wireless connections
-	Connect AP to port 8 (AP is PoE and port 8 is the PoE port)
-	Wireless devices should not have internal access
