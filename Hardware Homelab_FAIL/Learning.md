Part of the issue with this project is not finding test cases and parameters before working on the hardware. Here are the test cases I drew up because I couldn’t keep everything straight: 

TP-Link IP: 192.168.0.1
ASUS IP: 192.168.50.1
Phone (wireless) IP: 192.168.50.83
NSM (wired) IP: 192.168.50.100
NSM (wireless) IP: 192.168.50.199

Desired Outcome:
Access to ASUS? Yes
Access to TP-Link? Yes
Access to ASUS, TP-Link, and Internet? Yes
Does Wireshark collect data from NSM and phone? Yes


*Case 1:* 


Access to ASUS? No
Access to TP-Link? Yes
Access to ASUS, TP-Link, and Internet? No
Does Wireshark collect data from NSM and phone? No
FAIL

*Case 2:* 


Access to ASUS? Yes
Access to TP-Link? Yes
Access to ASUS, TP-Link, and Internet? No
Does Wireshark collect data from NSM and phone? No
FAIL

*Case 3:* 


Access to ASUS? No
Access to TP-Link? Yes
Access to ASUS, TP-Link, and Internet? No
Does Wireshark collect data from NSM and phone? No
FAIL

*Case 4:*


Access to ASUS? Yes
Access to TP-Link? Yes
Access to ASUS, TP-Link, and Internet? No
Does Wireshark collect data from NSM and phone? Yes
FAIL

*Case 5:*


Access to ASUS? Yes
Access to TP-Link? Yes
Access to ASUS, TP-Link, and Internet? No
Does Wireshark collect data from NSM and phone? Yes
FAIL


*Case 6:*


Access to ASUS? Yes
Access to TP-Link? Yes
Access to ASUS, TP-Link, and Internet? No
Does Wireshark collect data from NSM and phone? Yes
FAIL


- We could try test cases where the NSM is attached to R2, but that would be arbitrary. 
