# NSM Failing (2/9/22)

This is a continuation of the last project, Some Fail to Connect. 

**Issue:**
-	The wifi connection appears to still be up according to some devices and the unifi controller on my phone, but no devices can reach any site. 

**Gathered Information:**
-	Pings are unreachable. 
-	Unifi controller doesn’t detect any clients if the network drops. 
-	Network drops are intermittently and last for hours or will last until troubleshooting can occur. 
-	pfSense detects the AP and NSM. 

**Troubleshooting:**
-	Ran “sudo service network-manager restart” – to restart the network manager – did not resolve issue.
-	Ran “ping 127.0.0.1” – to see if the NIC was having issues. The ping created an error that I won’t be including in this. Appears to be a NIC issue.
-	Investigated further using the following link: https://askubuntu.com/questions/1010309/rtnetlink-answers-input-output-error-given-when-trying-to-bring-interface-up, which states it’s an electrical issue. This makes more sense to me than it being isolated to a NIC issue. NSM device is an old power-hungry MacBook Pro that can’t hold a charge. 

**Resolutions:**
- Replacing NSM device with an 8GB RAM, 256GB SSD, 2.7GHz, Dual Ethernet mini PC. Back to Projects 1 and 2, know when to stop and move on. In this case, it makes more sense to replace the power-hungry NSM before spinning up the software side of the NSM which will deploy software I’m not familiar with. The software setup lab will be long and isn’t worth trying to get the old machine to run all of that on top of this issue.


