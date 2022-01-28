Clearly there’s been an issue with Unifi Network Controller not detecting the AP, so this is lengthy. Towards the end, there will be general troubleshooting and some thought process. Everything will start on 192.168.1.0/24 since pfSense is defaulted to that and DHCP hasn’t completely detected the AP. Just setting everything on .1.0/24 to see if it’s possible. 

FW:
1.	Configure LAN to be 192.168.1.1
2.	Delete static IPs 
3.	Delete FW rules
4.	Restart DHCP server
5.	Connect SW to LAN port 1

SW: 
1.	Factory reset switch
2.	Go to default IP 192.168.0.1
3.	Change the IP
    a.	System > system info > system IP
4.	Connect NSM to port 3
5.	Connect AP to port 8
-	The below link will assist with creating a static IP on SW1: https://www.tp-link.com/us/support/faq/2122/ 
-	The below link will assist with creating a static IP on SW1: https://www.tp-link.com/us/configuration-guides/accessing_the_switch/?configurationId=18231#_idTextAnchor002

AP: 
1.	Factory reset AP
2.	See if FW DHCP server sees it
3.	Log into Unifi console and see if it can be detected
4.	If it does not auto-detect the AP, 
    a.	ssh into the AP via ssh ubnt@192.168.1.1 and ubnt
    b.	set-inform http://unifi:8080/inform or http://192.168.1.1/inform

Results: there have been issues with the controller not detecting the AP, with messages like unresolvable or cannot be reached during set-inform. Factory reset the AP multiple times but no luck in the controller detecting the device. 

Troubleshooting:
-	ping “unifi”, if it doesn’t resolve then it’s a DNS issue
-	ping FW
-	Disable DHCP on the AP
-	Turn off “Uplink Connectivity Monitor”

-	If this fails to detect the AP, please try the following: 
    - The link below will assist with recovering a “bricked” AP: https://help.ui.com/hc/en-us/articles/204910124 
    - The link below will assist with uninstalling the Unifi network application: https://help.ui.com/hc/en-us/articles/360015373734-UniFi-How-to-Uninstall-the-UniFi-Network-Application-
    - The link below will assist with installing Unifi network application on Ubuntu: https://help.ui.com/hc/en-us/articles/220066768-UniFi-Network-How-to-Install-and-Update-via-APT-on-Debian-or-Ubuntu
    - The link below will assist with adapting the device: https://www.youtube.com/watch?v=P_8FdynsfiE&list=PLaagW6hCBws-Q6bGrRyY2AjuwOR-M9a5o&index=7 
    - The link below will assist with creating a static IP on SW1: https://www.tp-link.com/us/support/faq/2122/ 
    - The link below will assist with creating a static IP on SW1: https://www.tp-link.com/us/configuration-guides/accessing_the_switch/?configurationId=18231#_idTextAnchor002
    - The link below will assist with changing the subnet of the AP: https://community.ui.com/questions/Best-way-to-change-IP-address-and-subnet-of-Unifi-AP-and-controller/02770aac-7cd3-4ab4-9734-d0ac1c27a2fa

After following the “bricked” AP recovery instructions, this controller still did not detect the AP. I thought creating a VLAN would be a way to resolve this. This will probably be used in a future change where the ASUS router as an AP. Here are the steps below:

-	The below link will assist with creating VLANs in pfSense: https://www.youtube.com/watch?v=b2w1Ywt081o&t=290s 

FW:
1.	Interfaces > Assignments > VLANs > Edit > VLAN 100 and default parent interface, “VLAN for AP”
2.	Interface > Assignments > Add VLAN 100 on default interface
3.	Interfaces > VLANs > Change interface description to “VLAN for AP on LAN” and enable the interface, setup static as 192.168.100.1/24
4.	Services > DHCP Server > “VLAN for AP on LAN” > enable DHCP > set a range
5.	Firewall > Rules > “VLAN for AP on LAN” > allow all traffic with source “VLAN for AP” and dst any, block all traffic to LAN net

-	The link below will assist with creating VLANs in TP-Link SW: https://www.youtube.com/watch?v=5ohLAFHnOHg 

SW: 
1.	Untag port 8 on VLAN 100. 
2.	Look at PVID Settings and see what VLAN the ports are on
3.	Plug in NSM into port 8 and make sure the IP is on the 100 network
4.	Ping 192.168.50.2
5.	Try to login to 192.168.50.2
6.	Ping 192.168.50.1
7.	Try to login to 192.168.50.1
-	At the end of this video, he was still able to log into the switch even though he was on the VLAN network.
-	Try tagging port 8 to port 3 and see if all traffic is being monitored.

AP:
1.	Wireless Networks > Create Network > use VLAN 100
-	According to the same person, you should be able to skip no devices detected. 

I didn’t follow these steps, but I’m going to leave it as part of the thought process and learning experience. Instead, I connected the error messages like “unresolvable” and “cannot reach” when ssh-ing into the AP via ssh ubnt@192.168.50.8, set-inform http://unifi:8080/inform or http://192.168.50.8/inform. I’m thinking the issue isn’t with the DNS or DHCP in pfSense, but the controller itself. I realized that I was remoting into the AP and trying to set the AP to connect to itself. I’m an idiot. That would be why it can’t resolve – it cannot attach to itself. The controller and Unifi devices must be different. 

What IP should the controller have? Should it be a different IP or the IP of the computer? From https://community.ui.com/questions/Odd-Controller-IP-address/fb37796c-59bb-46f9-a10c-2d93465256b5, it looks the controller IP is the IP of the machine. The controller is also local so the NSM will need to have it up and running.  

What IP should the AP have? 192.168.50.8 since it’s plugged into port 8.

FW: 192.168.50.1 
SW: 192.168.50.2 
Controller IP: 192.168.50.3 (port 3)
NSM IP: 192.168.50.3 (port 3)
AP IP: 192.168.50.8 (port 8)

1.	Change the controller IP:
    a.	Settings > Controller > Change the controller host IP to 192.168.50.3 then select override inform host with controller hostname/IP
    b.	The link below will assist with the controller change and some explanation:  https://www.youtube.com/watch?v=90NGVNTMurs 
    c.	The link below will assist with validation of changing the IP of the AP and the controller:  https://community.ui.com/questions/Controller-Name-versus-Controller-Hostname-IP/5bf802f8-d82e-4a61-b589-8728ac67ff81 
2.	Create a controller static entry:
    a.	Create a static DNS entry in pfSense for unifi with 192.168.50.3. This may also refer to creating a static IP on the LAN. This may not be necessary if we just change the controller IP to 192.168.50.3 and static map the NSM in pfSense.
3.	Log into the controller at the new IP at http://192.168.50.8:8080 
4.	If the new AP doesn’t adopt, ssh into the AP then run 
    a.	ssh ubnt@192.168.50.3
    b.	set-inform http://192.168.50.8/inform 
