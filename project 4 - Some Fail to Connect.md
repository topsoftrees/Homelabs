# Some Fail to Connect (2/6/22-2/7/22)

## Issue:
-	Three devices can connect to the Wifi, but one receives “Cannot connect to network error.”

## Gathered Information:
-	Disconnected problem device from Wifi – did not resolve issue. 
-	Restarted problem device – did not resolve issue.
-	Logged into controller – NSM pulled unifi.com instead of https://192.168.50.3:8443.
-	Tried to login to https://192.168.50.3:8443 – cannot open the site, no network detected. 
-	Ran “ifconfig” – no IP pulled.
-	Ran “sudo dhclient -r” to release any potential IP being pulled. 
-	Ran “sudo dhclient” to pull a new IP – received “RTNETLINK answers: Input/output error” then 192.168.50.3 will be pulled within 10 min. 

## Troubleshooting:
-	Troubleshooting “RTNETLINK answers: Input/output error.”
-	Ran “sudo apt-install upgrade” – did not resolve issue
-	Ran “sudo apt-install update” – did not resolve issue. 
-	Ran “sudo dhclient -r” and “sudo dhclient” – did not resolve issue. Received “RTNETLINK answers: Input/output error.”
-	As a temporary fix until I can take a closer look – deleted the wifi network from the controller and re-added it. There were some delays in getting all devices up and running, but all are up and running again. 

## Resolution:
-	TEMPORARY: Deleted the wifi network from the controller and re-added it. There were some delays in getting all devices up and running, but all are up and running again. 

