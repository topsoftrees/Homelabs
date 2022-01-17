After going through the first phase of considerations, the homelab is going to be re-worked to the following:

<img width="633" alt="Screen Shot 2022-01-17 at 10 37 00 AM" src="https://user-images.githubusercontent.com/74877876/149798921-6853cf7d-ba1c-4cbb-84cc-2a4490a36238.png">


R1 (ASUS):
-	NSM will connect to port 2
-	Home devices will wirelessly connect to external network
-	DHCP will be enabled with range 192.168.50.3-192.168.50.254
-	Modem will connect to WAN port
-	R2 will connect to port 1
-	Port mirror 1 to port 2

R2 (TP-Link):
-	Guest devices will connect to external network
-	DHCP will be disabled and with a static IP of 192.168.50.2
-	R1 will connect to port 1
