# Unexpected Power Outage (2/1/22-2/2/22)

**Issue:**
-	There was an unexpected power outage sometime on 2/1/22. Since then, the Wifi has been disconnected. 

**Gathered information:**
-	Wireless is disconnected.
-	AP indicates it’s up and running with a blue light.
-	Unifi Controller says it’s offline and AP isn’t detected. 
-	Successfully able to ping 192.168.50.1, 192.168.50.3, 192.168.50.8 from 192.168.50.3.
-	pfSense says 192.168.50.3 and 192.168.50.8 are online and active.


**Troubleshooting:**
-	Uninstalled and reinstalled Unifi controller – did not resolve issue.
-	Tried to SSH into the AP – unable to SSH into the AP.
-	Knowing the controller is 192.168.50.3, I think the network doesn’t associate unifi with 192.168.50.3. Went to https://192.168.50.3:8443 > Settings> Admin > and changed the controller host to 192.168.50.3 instead of mydomain.unifi.com. After this, the 192.168.50.3 controller detected the AP and started adopting. 

**Resolution:**
-	Changed the controller from mydomain.unifi.com to https://192.168.50.3:8443.

