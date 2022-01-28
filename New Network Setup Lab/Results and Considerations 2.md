After trying the implement the plan, these were the results:

Setting the SW to a static IP on the FW,
-	Did not automatically push to the SW. 
-	Restarting the modem and SW did not push to the SW.
Configured the 100 VLAN on FW and SW and tied the interface to OPT and tagged port 8,
-	Did not create the 100 network. 

Lessons Learned:
-	YouTube is a best friend. A lot of the assistance in pfSense configurations came from YouTube. It helped in creating the VLAN, tagging it to the interface, and enabling it in the firewall. Given I’ve never configured anything in a network, I’m rather happy at the use of YouTube I used. 
-	Remember common configurations. In the previous setups, a switch wasn’t a consideration in the initial configuration. In this change, the switch isn’t used effectively. We know that managed switches can use VLANs and VLANs are used to segregate networks. 

