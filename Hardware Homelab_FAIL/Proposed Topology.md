Initial topology: 

<img width="767" alt="Screen Shot 2022-01-17 at 10 29 26 AM" src="https://user-images.githubusercontent.com/74877876/149797726-d9eac54a-4660-48dd-a960-38c2605bc853.png">


R1 (ASUS):
-	Home devices will wirelessly connect 
-	DHCP will be enabled with range 192.168.50.3-192.168.50.254
-	Modem will connect to WAN port
-	R2 will connect to port 1

R2 (TP-Link):
-	NSM will connect to port 2
-	DHCP will be disabled and with a static IP of 192.168.50.2
-	R1 will connect to port 1
-	Port mirror 1 to port 2
