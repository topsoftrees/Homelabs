# Slow Wireless (3/15/22-3/19/22)

Since implementing the new controller, the wireless has been slow. 

**Contents:**
1.	Issue
2.	Background Info
3.	Troubleshooting – Channels
4.	Troubleshooting – VLAN Setup
5.	LAN vs Guest
6.	Resolution
7.	Lessons Learned

**Issue:**
- Wireless speeds have been running slow since the old NSM’s battery went dead (project 5). The speeds have been ~150 Mbps. 


**Background Info:**
-	Unifi AP-AC Lite runs a guest network
-	Wireless devices connect to the guest network
-	Restarted router – does not resolve issue. 


**Troubleshooting – Channels:**

There’s one device that’s been having issues connecting to the 192.168.100.1/24 network and is prompting for the network’s channel to be changed. 

-	Changed the 2.4 GHz and 5 GHz channels for both 192.168.50.1/24 and 192.168.100.1/24 networks. 2.4 GHz to channels 1, 6, or 11. 5 GHz to VHT40 or optional VHT80 (but less reliability) – does not resolve issue. 
-	Reset the network settings – does not resolve issue.


**Troubleshooting – VLAN Setup:**

Since the AP is connected to a switch, a VLAN should be used so the AP cannot access the internal network. Ideally, the internal network would be 192.168.50.1/24 and the external is 192.168.100.1/24. 

_In pfSense:_ 
1. Interfaces > Assignments > VLANs > add VLAN 100 (wireless) 
2. We should add the VLAN to LAN since the switch and AP are connected through the LAN port of SG-1100. If the AP was connected through the OPT port, we should add the VLAN to OPT interface.
3. Interfaces > Assignments > VLANs > VLAN 100 > add
4. Interfaces > Assignments > VLANs > VLAN 100 > enable, wireless, 192.168.100.1/24
5. Services > DHCP Server > VLAN 100 > enable, 192.168.100.10-192.168.100.254
6. Firewall > Rules > VLAN 100 > add, pass any protocol (this is temporary)
7. Interfaces > Switch > VLANs > edit, VLAN tag 100, guest network, 0 tagged and 2 tagged. 
8. In Interfaces > Switch > Ports, LAN is associated with 0 and 2. Going back to 1a, we need to assign the VLAN to the interface and tag 0 and 2. 

_In TP-Link Switch:_
1.	Add a VLAN 100, tag port 1 and 8. 

- **This brought up the question, can the Unifi Controller and AP be on different subnets?** Go to settings and make sure the controller isn’t just called “unifi,” but the IP itself.

- Pinged 192.168.100.1 from 192.168.50.3, successfully reached the remote host. 

_In Unifi:_
1.	Settings > Network > Add a network > Test Network, VLAN 100, turn off Auto Scale Network, Gateway IP/Subnet: 192.168.100.1/24
2.	Settings > Wifi > Add a Guest Network > Test 100, Test Network, Wifi Type: Guest, Security: WPA-3

I was able to connect to the new network but there was no internet connection. Later, I realized no internet connection sounded like the VLAN was created, but not enabled. I didn’t connect the dots. 

There was a failure for a device to connect to the 100 VLAN when it was created, so a 192.168.150.1/24 network was created and enabled using the same steps as above. 

The below links assisted with setting up a VLAN:
-	Purpose of VLANs in a Switched Network: https://www.ciscopress.com/articles/article.asp?p=2208697&seqNum=4 
-	SG-1100 VLAN Switch Configuration: https://www.youtube.com/watch?v=Bp_B79-WLlU
-	How to Setup VLANs with pfSense and Unifi: https://www.youtube.com/watch?v=b2w1Ywt081o
-	Unifi Network connecting to TP-Link Switch: https://www.youtube.com/watch?v=5oJXcPJwmRk
-	Access Points and Creating Wifi VLANs Explained Using Unifi Wireless: https://www.youtube.com/watch?v=6wcbkE3TF3c
-	Unifi – L3 Adoption for Remote Unifi Network Applications: https://help.ui.com/hc/en-us/articles/204909754-UniFi-Device-Adoption-Methods-for-Remote-UniFi-Controllers#6


**LAN vs. Guest:**

After creating the other network, the internet was still running slow. In Unifi research, there’s supposed to be a considerable speed difference between the internal and external network. I’ve been able to verify this with the NSM speed and the speed of our personal devices. I moved the 150 VLAN to an internal network then went to pfSense to disable internal access using firewall rules. 

_In pfSense:_
1.	Firewall > Rules > VLAN 150 > Reject any protocol destination LAN and pass any protocol
- The above two rules were not taking affect, so I instead implemented the below rule. 
1.	Firewall > Rules > VLAN 150 > Pass any protocol that is NOT destination LAN
- Pinged 192.168.150.1 from 192.168.50.3, successfully reached the remote host. 
- Pinged 192.168.50.1, 192.168.50.2, 192.168.50.3 from a host on 192.168.150.1/24 which was unreachable. 


**Resolution:**

Created the VLAN in pfSense, tagged port 1 and 8, and created a VLAN network in Unifi:

_In pfSense:_ 
1. Interfaces > Assignments > VLANs > add VLAN 150 (wireless) 
2. We should add the VLAN to LAN since the switch and AP are connected through the LAN port of SG-1100. If the AP was connected through the OPT port, we should add the VLAN to OPT interface.
3. Interfaces > Assignments > VLANs > VLAN 150 > add
4. Interfaces > Assignments > VLANs > VLAN 150 > enable, wireless, 192.168.150.1/24
5. Services > DHCP Server > VLAN 150 > enable, 192.168.150.10-192.168.150.254
6. Interfaces > Switch > VLANs > edit, VLAN tag 150, guest network, 0 tagged and 2 tagged. 

_In TP-Link Switch:_
1. Add a VLAN 150, tag port 1 and 8. 

_In Unifi:_
1. Settings > Network > Add a network > Test Network, VLAN 150, turn off Auto Scale Network, Gateway IP/Subnet: 192.168.150.1/24
2. Settings > Wifi > Add a Guest Network > Van 150, VAN Network, Wifi Type: Guest, Security: WPA-3

_In pfSense:_
1.	Firewall > Rules > VLAN 150 > Pass any protocol that is NOT destination LAN

**Lessons Learned:**

I’ve made lots of progress in documentation and thinking through the process. In everything I’ve learned from these projects, it been beneficial in my job too. 
1.	**VLANs should be used with switches** – You’re specifying which networks to be routed through which ports. In this project, it’s being done – therefore, VLANS need to be used. I tried pushing them off for as long as possible because there was just a guest network configured. Since the internal network runs significantly fast than the external, it pretty much forced my hand to create a VLAN and another network. 
2.	**Error messages aren’t always reliable** – One device was prompting for incorrect channels. After doing trying the first few troubleshooting attempts and them failing, I started researching the error message. Apparently, this is a common issue with the version the device is on. 
3.	**YouTube and the Unifi community is a friend** – To configure pfSense, there’s been a lot of YouTube videos used. Unifi has primarily been going through all the different options to speed up the network. A lot of the links brought up new ideas or prompted for more thorough thoughts (i.e. why we’re tagging and which interface the VLAN needs to be connected to). 
4.	**Slowing down and taking breaks is always helpful** – I feel like I’ve made lots of progress in each project. This project took so long because I was trying to brute force the fix during the 1-2 hours I had to work on it each day. This has been such a poor way of thinking and working through the issue. I avoid this in my job but somehow forgot it when I left work, in this case. I should’ve double checked that the pfSense VLAN was enabled because I wouldn’t have duplicated the work. 
