# 2 Router Homelab (1/11/22-1/15/22)
Contents:

1. Objectives
2. Proposed topology
3. Topology Methodology
4. Considerations 1
5. Changed Plan 1
6. Considerations 2
7. Changed Plan 2
8. Implementing the Plan
9. Lessons Learned
10. Learning
11. Conclusions

**Objectives:**
-	Deploy a NSM
-	Understand more about networking the equipment I have
-	Understand network tap vs port mirroring
-	Monitor all network traffic via Splunk
-	Monitor all SSH
-	Monitor all DNS
-	Monitor whenever someone connects to one of my networks

**Proposed Topology:**

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

**Topology Methodology:**

This should keep NSM segregated form the rest of the network while mirroring all traffic through it. However, you can skip this step.

*Reset the networks:* I have to do this because the last time I tried a lab similar to this, I screwed up the network. 
   
   -	The following article will assist with resetting ASUS router: https://www.asus.com/support/FAQ/1000925/ 
   
   1.	Reset ASUS router by Method 1 from the above article.
        
        a. If Method 1 fails, please try Method 2.
   
   2.	Setup the home devices network on ASUS router.
    
   -	The following article will assist with resetting TP-Link router: https://www.tp-link.com/latam/support/faq/497/ 
   
   3.	Reset TP-Link router by Method 1 from the above article.


*Configuring the routers to work with one another:*
   
   -	The following YouTube video will assist with configuring the two routers: https://internet-access-guide.com/can-i-have-2-routers-with-spectrum/
    
   1.	Plug ASUS router into modem. 
   
   2.	Log into the ASUS router at 192.168.50.1.
       
       a. Enable DHCP and set up the range from 192.168.50.5 to 192.168.50.254. This IP will be 192.168.50.1. 
        
       b. Disconnect from wifi
      
       c. Disconnect router from modem
    
   3.	Plug TP-Link router into modem.
    
   4.	Log into the TP-Link router at 192.168.50.1

        a. Disable DHCP, give it a static IP address of something that’s not in the range from 1. This will be an IP like 192.168.50.2.
    
   5.	Use an ethernet cable to connect LAN port 1 from the ASUS to LAN 1 port on TP-Link.

   -	You should be connected to ASUS wifi and able to log into the interface of TP-Link (access 192.168.50.1 and 192.168.50.2)


*Setup port mirroring on the TP-Link router and connect the NSM:*
   
   -	The following article will assist: https://www.tp-link.com/us/support/faq/528/
   
   1.	Login to TP Link at 192.168.50.2
   
        a. Most likely under Network, possibly Switch, then enable Port Mirror
        
        b. Mirroring port 1, mirror port 2
   
   2.	Hard wire NSM to port 2 on TP Link 

**Considerations 1:**

One of the goals of this lab is to get the proof-of-concept correct or close to correct before implementing it so I’ll be making some notes in the process. 

After reviewing everything so far, there are a few concerns that come to mind: 
1.	*Is the NSM going to monitor the ASUS router since it’s connected to the TP-Link router?* Initially, I was thinking the port mirroring will copy all traffic passing through ASUS will be copied over to TP-Link since it’s port mirrored. After trying to piece together what I relatively understand about networking, I don’t think that’s right. From my understanding, 2 routers will segment but the intial proposal will defeat the purpose of the NSM and already going against one of our objectives. 
    a.	Instead, create a guest network on the second router or an AP. This will end up being an internal and external network on ASUS, then a possible internal and external network or just one on TP-Link for guests to connect. I would only use the external ASUS network. The NSM would then be hardwired on ASUS and port mirroring of TP-Link. It looks like the methodology instructions will work besides the port mirroring and connecting the NSM. 
2.	*Is my ISP going to charge me for two public IPs?* I know it’s a dumb question, but this is the level of networking understanding I’m working with currently and I want to document everything right now. I’m thinking no because the port connections, we’ll be connecting ASUS LAN to TP-Link LAN. If we were to connect both routers to the ISP modem, then yes. If we were to connect ASUS LAN to TP-Link WAN, the network would just break in my experience.
3.	*Is having multiple networks going to delay my internet speeds?* I don’t know if adding another router to the existing will _increase_ its speed but if there was different setup, including switches, then yes, two routers should increase the network speeds. I doubt the number of networks will change the speed. The number of devices won’t change throughout this process.
4.	The NSM can’t just be setup in promiscuous mode (it would also make this way too easy). Promiscuous mode is to monitor every device on the network you’re scanning from. If we were to set Wireshark into promiscuous mode, it will scan the devices that are also connected to the same network as Wireshark. Since there will be multiple networks setup in this, promiscuous mode won’t work. 

*Lessons Learned (it was a lot):*
-	*The importance of planning before doing.* The concept and proof-of-concept did not align. I tried something similar to this a while back and it backfired in my face. I connected ASUS LAN to TP-Link WAN and broke my network. That was fun… I thought maybe a collision occurred or just human error in the configuration. Each router should’ve broken up the collision and broadcast domain so that shouldn’t be the issue. Maybe the there was a collision due to the duplex speeds? 
-	With the same initial methodology, but the outcome is the TP-Link router turns into an access point. With that access point, I’d be able to connect to my external network on ASUS router and have everyone else connect to the AP just to prevent people from getting into my internal-internal network.
-	APs can be configured as Wifi-extenders or guest networks.
-	You can connect a USB stick into your router, have the router recognize it, and treat it as a NAS or a network drive! I didn’t even know a NAS was a network drive, it makes sense, I just didn’t connect the two -_-. 

**Changed Plan 1:**

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

**Considerations 2:**
1.	*How will the AP be monitored?* Will this just happen because we port mirror R2 to the NSM? Port mirroring is sending a copy of the network packets seen on one port to another switch port. Since TP-Link is part of ASUS LAN, I believe port forwarding will be enough to monitor both networks.

2.	When trying to setup how the networks will be monitored, I came across this video: https://www.youtube.com/watch?v=J5QJb3O19zI. It looks like this is what I’m wanting to setup. Since I don’t have experience in networking and I’m trying to understand it more, it feels good that I’ve done the research, came up with a plan myself, then went looking, instead of just looking for the answer. In this video, he states to change the channels to something that won’t overlap – channels 1, 6, and 11 don’t overlap. I haven’t learned about channels yet, but it looks like if networks are setup on the same channel, there will be collisions, and the less the channels overlap, the less collisions and faster the network will be. This came up in Considerations #1, if the network speed will decrease with multiple networks – I’ll change the channels to increase the network speed. 

3.	*Is port mirroring safe?* It’s as safe as you make the device that’s connected to WAN. 

4.	*How is everything going to be protected if I don’t have a firewall?* I’m going buy another device if necessary and implement a firewall. 

5.	*How can I implement a firewall with two routers and a computer with one NIC?*These two videos helped:
https://cs.wmich.edu/~rhardin/cs4540/fpSense.pdf, https://www.youtube.com/watch?v=FPgPHJvLmh0 

**Changed Plan 2:**

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

**Implementing the Plan:**
1.	Verified both ASUS and TP-Link have USB ports
2.	Reset ASUS router using method 1
3.	Setup ASUS network 
4.	Enabled DHCP from 192.168.50.3 (need to add static 192.168.50.2
5.	Downloaded pfsense
    
    a.	Downloaded memstick one
    
    b.	Ran gunzip /filepathtoimage to decompress the file
    
    c.	Wiped the disk in Disk util
    
    d.	Ran the following based on insturctions from netgate found on pfsense’s website
        
        i. diskutil unmountDisk /dev/usbstickname
        
        ii. dd if=imagefilepath.img of=/dev/usbstickname bs=4m (this took a long time because the file size)
     
        iii. diskutil eject /dev/usbstickname
6.	Reset TP-Link router
   
   a. Challenge: how should the interfaces connect? --------WAN-FW-LAN ------ WAN or LAN-Router?
   
        i.	The WAN-Router connection failed. I couldn’t get to the router and the firewall was redirecting to the router.
        
        ii.	TP-link-192.168.0.1
        
        iii. Asus 192.168.50.1
7.	Plugged in pfSense into both TP and ASUS, neither one detected the USB. Unable to implement hardware firewall. Unable to implement a software firewall because NSM will require 2 NICs which I don’t have. 

**Lessons Learned:**
1.	*Know what’s your equipment before planning too in depth.* I didn’t even know if one of the routers had a USB port. I should’ve plugged each router in and seen if it picks up on pfSense. 
2.	Draw out test cases and parameters to meet before testing the hardware.
3.	Predict the result of the test cases. 

**Learning:**

Part of the issue with this project is not finding test cases and parameters before working on the hardware. Here are the test cases I drew up because I couldn’t keep everything straight: 

    TP-Link IP: 192.168.0.1

    ASUS IP: 192.168.50.1

    Phone (wireless) IP: 192.168.50.83

    NSM (wired) IP: 192.168.50.100

    NSM (wireless) IP: 192.168.50.199

*Desired Outcome:*

    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? Yes

    Does Wireshark collect data from NSM and phone? Yes

    =PASS

*Case 1:* 

<img width="682" alt="Screen Shot 2022-01-17 at 10 43 44 AM" src="https://user-images.githubusercontent.com/74877876/149800127-a23cfee1-959a-4d25-97d0-02fe65974d70.png">


    Access to ASUS? No

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No

    Does Wireshark collect data from NSM and phone? No

    =FAIL

*Case 2:* 

<img width="683" alt="Screen Shot 2022-01-17 at 10 43 54 AM" src="https://user-images.githubusercontent.com/74877876/149800161-d6a0e5bf-0ad3-4048-80f5-9554e233f0f9.png">


    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No

    Does Wireshark collect data from NSM and phone? No

    =FAIL

*Case 3:* 

<img width="682" alt="Screen Shot 2022-01-17 at 10 44 13 AM" src="https://user-images.githubusercontent.com/74877876/149800185-097236a3-7111-4888-baa2-ca3a1187340b.png">


    Access to ASUS? No

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No

    Does Wireshark collect data from NSM and phone? No

    =FAIL

*Case 4:*

<img width="681" alt="Screen Shot 2022-01-17 at 10 44 50 AM" src="https://user-images.githubusercontent.com/74877876/149800247-dafae785-eddd-43d0-93c2-64402d2c39ad.png">


    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No

    Does Wireshark collect data from NSM and phone? Yes

    =FAIL

*Case 5:*

<img width="686" alt="Screen Shot 2022-01-17 at 10 44 56 AM" src="https://user-images.githubusercontent.com/74877876/149800256-9ba019fc-1ccd-434b-b88b-88c82928ece7.png">


    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No
  
    Does Wireshark collect data from NSM and phone? Yes
  
    =FAIL


*Case 6:*

<img width="686" alt="Screen Shot 2022-01-17 at 10 45 18 AM" src="https://user-images.githubusercontent.com/74877876/149800349-5cea5537-ca11-4ea3-a829-b1ce2c662ab3.png">


    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No
  
    Does Wireshark collect data from NSM and phone? Yes
  
    =FAIL


- We could try test cases where the NSM is attached to R2, but that would be arbitrary. 

**Conclusions:**

In doing this failure of a lab, I’m considering it a win. I haven’t forced myself to document this extensively or explain my thought process before. Here are some conclusions I came to in doing this lab: 
1.	Plan but don’t over-plan. Know your equipment and if you can produce the result you want. I should’ve checked to see if my routers had USB ports and if they did, see if the router detects the USB. Even though I didn’t know you could just boot into the USB, I should’ve done some more intense digging to see if my desired goal was possible. 
2.	Make considerations and document them. Some of the biggest things I learned came from the considerations. Asking questions, searching for the answer, and answering them was the greatest help to piece together some of my knowledge and see where I struggle. 
3.	Plan out test cases and the expected outcomes. I should’ve drawn out test cases to begin with to help keep everything straight. When I came up with test cases, some of them I didn’t think through. Certain outcomes should’ve been predicted, such as network segmentation or the placement of the NSM. 
4.	Keep security in mind. In one of the initial plans, port forwarding, or port mirroring was planned. In doing research, I came across ‘the device being mirrored to is only as safe as the device you’re mirroring.’ I want to keep the NSM safe so I decided there will be a firewall even if it means failing. 
5.	Documentation is everything. I didn’t have to try to keep everything straight in my mind and I won’t make the same mistakes in later homelabs.
6.	Things are (slowly) coming together. I’ve mostly done book/certification studying, and software homelabs, but haven’t really done much with hardware. This helped me see some fundamental connections I haven’t seen before now. 
