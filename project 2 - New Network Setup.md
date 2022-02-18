# New Network Setup (1/21/22, 1/23/22-1/28/22) 

This is basically a continuation of the previous lab. The previous lab resulted in both routers not having the capability for a firewall and to connect to each other. A different, more advanced piece of equipment is needed – I purchased the Netgate SG-1100 pfSense firewall/router.

*Contents:*

1. Objectives
2. Proposed Topology
3. Methodology 1
4. Results and Considerations 1
5. Topology 2
6. Methodology 2
7. Results and Considerations 2
8. Topology 3
9. Methodology 3
10. Results and Considerations 3
11. Conclusions

**Objectives:**
-	Be more efficient in the planning and implementation. 
-	Create fast test cases. 
-	Know the equipment and what it’s capable of. 
-	Make it easier on yourself without over-spending.
-	Make educated moves based on what you know. 
-	Build configuration skills and try not relying so heavily on YouTube for configurations.
-	Document the troubleshooting process.

**Proposed Topology:**

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

**Methodology 1:**

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


**Results and Considerations 1:**

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


**Topology 2:**

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


**Methodology 2:**

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

**Results and Considerations 2:**

After trying the implement the plan, these were the results:

Setting the SW to a static IP on the FW,
-	Did not automatically push to the SW. 
-	Restarting the modem and SW did not push to the SW.
Configured the 100 VLAN on FW and SW and tied the interface to OPT and tagged port 8,
-	Did not create the 100 network. 

Lessons Learned:
-	YouTube is a best friend. A lot of the assistance in pfSense configurations came from YouTube. It helped in creating the VLAN, tagging it to the interface, and enabling it in the firewall. Given I’ve never configured anything in a network, I’m rather happy at the use of YouTube I used. 
-	Remember common configurations. In the previous setups, a switch wasn’t a consideration in the initial configuration. In this change, the switch isn’t used effectively. We know that managed switches can use VLANs and VLANs are used to segregate networks. 


**Topology 3:**

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

**Methodology 3:**
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

**Results and Considerations 3:**

I tried a similar topology, but the switch had a dynamic IP, these were the results:

Installing the controller on the NSM using https://help.ui.com/hc/en-us/articles/220066768-UniFi-Network-How-to-Install-and-Update-via-APT-on-Debian-or-Ubuntu automatically detected the AP. I setup the AP as a static IP with 192.168.50.8 in pfSense. During the adaptation of the AP, the IP was corrected from dynamic to static. No other steps or troubleshooting was required for the AP to function. Setup a guest network within 192.168.50.0/24 on the AP. Verified 192.168.50.1, 192.168.50.2, and 192.168.50.3 cannot be reached from the guest network. Verified the static IPs are correct, DHCP is correct, and the firewall rules are correct. 


Lessons Learned:
1.	Every device, wired or wireless, is on the same network. The whole point of this homelab is to create a strong network and not having a different ‘guest’ network defeats that purpose. 
2.	The AP is having issues creating a guest network. The Unifi Controller is unable to login via the web interface, only the mobile app. Even then, a guest network cannot be created. 
3.	If the AP were to be setup on it’s own VLAN, will the NSM detect it? Since it’s a separate network, I doubt the NSM will detect it. Try tagging port 8 to port 2 and see if all traffic is monitored.
    a.	VLAN tagging: more than one VLAN is handled on a port. Not necessary in this case. 
    b.	VLAN untagging: the AP is unaware of any VLAN configuration. AP sends its traffic without any VLAN tag on the frames. When the frame reaches the switch port the switch will add the VLAN tag. 
4.	When trying to further troubleshoot adapting the AP, the FW wasn’t creating a new DHCP lease for the AP. I factory reset the AP multiple times, no luck. Restarted the DHCP server, no luck. Ran the TFTP recovery brick instructions, no luck. 
5.	Because there’s an issue, don’t just go to YouTube, think about the issue. I saw the unresolvable message and thought DNS. It didn’t look to me like there would be a DNS issue according to pfSense, so I assumed DHCP because it didn’t appear to be fully configured. I was slow in realizing this, but I eventually figured it out (and on my own) that the controller and AP will be on different IPs, and the controller is local so it should be based on the NSM IP. 
6.	Try not to be overwhelmed when looking at new equipment. I should’ve trusted the networking basics instead of panicking and googling how to do everything in Unifi. This led me to not connecting controller needs an IP and so does the AP.

**Conclusions:**

Since the previous lab, 
-	I learned when to stop and know when to move on from a step. In the initial topology, I came up with the topology, the IPs I wanted, and the test cases. I didn’t try to just make something work, but I had a plan and I worked on working the plan. 
-	I was more process oriented and didn’t make as many assumptions, like assuming pfSense could be loaded onto the routers or too much over-planning. In this lab, I assumed a VLAN should’ve been created. I did better in planning the setup with the ports and the connections. 

Going forward, 
-	Don’t relay too much on YouTube. It takes away from what you learn. I only actually learned when I had to struggle to find options myself. I connected the controller and AP need to be on different IPs.
-	Step away from the project and think for yourself. I should’ve been relaxed and confident in my abilities in this lab. I would’ve connected that the controller and AP need to be on different APs.  
-	Find the official documentation. Initially, I tried to install the controller on Ubuntu using a YouTube video. The installation ended up failing. I should’ve tried harder to find the official documentation for the install. It took some digging, but I should’ve started there. 
-	Sleep is important. There were a few days in there were I worked on this project all day long, didn’t take a break, and got only a few hours of sleep. 
-	Work the problem and stay calm. Slow down. Manage the chaos yourself. If you’re scared of it, jump in because that’s the only way you’ll learn. 

Future Considerations:
-	Connect an AP-configured router to the switch and creating another network like a guest network. This will only be after getting port 8 on a different VLAN and the AP on a different network. 
-	Ping 192.168.50.1, 192.168.50.2, 192.168.50.3 and see if they can be reached from the the AP.

