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
