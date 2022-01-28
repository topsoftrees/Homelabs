Equipment:
-	Netgate SG-1100 pfSense Firewall/Router
-	TP-Link 8-port Switch 
-	Unifi AP

I’m going with a switch because it looks like a VLAN will be required for the 100 network to be used (it’s currently configured on the FW) and I’m wanting a dedicated wireless network. The VLANs will need to be configured on the FW and SW. The dedicated AP is instead connecting an AP-configured router to the switch. The routers have proven to not be effective in the labs and the different test cases. Consumer routers will not be used in this homelab and most likely not future ones – one future change could be connecting an AP-configured router to the switch and creating another network like a guest network.

After going through the first phase of considerations, the homelab is going to be re-worked to the following:

<img width="655" alt="Screen Shot 2022-01-28 at 10 46 10 AM" src="https://user-images.githubusercontent.com/74877876/151577493-64b94072-f861-41f4-bf24-5a7bbe892a8d.png">

Firewall (SG-1100):
-	Modem will connect to WAN port with an ISP assigned IP
-	pfSense: 192.168.50.1 (static)
-	NSM: 192.168.50.2 (static, LAN port)
-	Hardwire NSM - monitor all wired and wireless connections
-	SW1: 192.168.100.1 (OPT port)
-	Setup 100 VLAN for the wireless network

SW (TP-Link):
-	FW: 192.168.100.1 (OPT port)
-	Wired connections: 192.168.100.0/24 (LAN ports)
-	Setup 100 VLAN for wireless network
-	Connect AP to port 8 (AP is PoE and port 8 is the PoE port)
-	Wireless devices should not have internal access

