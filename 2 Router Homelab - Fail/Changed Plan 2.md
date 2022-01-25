Changing the plan to the following: 

<img width="672" alt="Screen Shot 2022-01-17 at 10 38 43 AM" src="https://user-images.githubusercontent.com/74877876/149799249-7166ab4e-0ddf-4436-ae04-ccff3bc0b47b.png">


Assuming one of the routers has a USB device, I can boot the router into pfSense. 

Firewall/R1:
-	Running pfSense 
-	Modem will connect to WAN
-	R2 will connect to port 1

R2:
-	Firewall/R1 will connect to WAN/LAN
-	NSM will connect to port 1/2
-	Setup with internal and external networks
-	Devices will connect to external network
