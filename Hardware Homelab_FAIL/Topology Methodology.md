This should keep NSM segregated form the rest of the network while mirroring all traffic through it. However, you can skip this step.

*Reset the networks:* I have to do this because the last time I tried a lab similar to this, I screwed up the network. 
-	The following article will assist with resetting ASUS router: https://www.asus.com/support/FAQ/1000925/ 
1.	Reset ASUS router by Method 1 from the above article.
    a.	If Method 1 fails, please try Method 2.
2.	Setup the home devices network on ASUS router.
-	The following article will assist with resetting TP-Link router: https://www.tp-link.com/latam/support/faq/497/ 
3.	Reset TP-Link router by Method 1 from the above article.

*Configuring the routers to work with one another:*
-	The following YouTube video will assist with configuring the two routers: https://internet-access-guide.com/can-i-have-2-routers-with-spectrum/
1.	Plug ASUS router into modem. 
2.	Log into the ASUS router at 192.168.50.1
    a.	Enable DHCP and set up the range from 192.168.50.5 to 192.168.50.254. This IP will be 192.168.50.1. 
    b.	Disconnect from wifi
    c.	Disconnect router from modem
3.	Plug TP-Link router into modem.
4.	Log into the TP-Link router at 192.168.50.1
    a.	Disable DHCP, give it a static IP address of something that’s not in the range from 1. This will be an IP like 192.168.50.2.
5.	Use an ethernet cable to connect LAN port 1 from the ASUS to LAN 1 port on TP-Link.
-	You should be connected to ASUS wifi and able to log into the interface of TP-Link (access 192.168.50.1 and 192.168.50.2)

*Setup port mirroring on the TP-Link router and connect the NSM:*
-	The following article will assist: https://www.tp-link.com/us/support/faq/528/
1.	Login to TP Link at 192.168.50.2
    a.	Most likely under Network, possibly Switch, then enable Port Mirror
    b.	Mirroring port 1, mirror port 2
2.	Hard wire NSM to port 2 on TP Link 

