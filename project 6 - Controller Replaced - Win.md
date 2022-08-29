# Controller Replaced (2/12/22)

This is due to the previous projects which resulted in a NIC or power failure of the unifi controller. Given the RAM and GHz of the new device, the controller and NSM will be divided between two devices. 

**Contents:**
1.	Objectives
2.	Methodology 
3.	Implementation & Troubleshooting
4.	Resolution
5.	Lessons Learned

**Objectives:**
-	Apply what I’ve learned in Unifi and swap out the controller in much faster time than before.
-	Create a plan before Googling anything.
-	Think for myself.
-	Don’t spend more than 2 hours on setting this up.

**Methodology:**

This was the initial plan before anything was done:
1.	Reimage the computer with Ubuntu. 
2.	sudo apt update && sudo apt upgrade && sudo apt install net-tools.
3.	Set new machine’s IP to 192.168.50.3 in pfSense.
4.	Install unifi controller on the new machine. 
5.	Double check the controller’s IP is set to 192.168.50.3 and the override host is set. 
6.	Disconnect old machine from internet. 
7.	Re-adopt the AP. 

**Implementation & Troubleshooting:**

- When trying to run the controller on the new machine, the site could not be pulled up. Running sudo service unifi start and sudo service unifi restart resulted in the following error: 
```bash
    Active: failed (Result: start-limit-hit)
    unifi systemd[1]: unifi.service: Scheduled restart job, restart counter is at 2.
    unifi systemd[1]: Stopped unifi.
    unifi systemd[1]: Starting unifi...
    unifi unifi.init[6393]:  * Starting Ubiquiti UniFi Controller unifi
    unifi unifi.init[6522]: Cannot locate Java Home
```
which is clearly a Java installation error. 

- Reinstalled Java, received the following error: 
```bash
    Active: failed (Result: start-limit-hit)
    unifi systemd[1]: unifi.service: Scheduled restart job, restart counter is at 2.
    unifi systemd[1]: Stopped unifi.
    unifi systemd[1]: Starting unifi...
    unifi unifi.init[6393]:  * Starting Ubiquiti UniFi Controller unifi
    unifi unifi.init[6522]: Cannot locate monogod
```
- Researched the error message which pretty much came to a full reinstall from a Ubiquity employee’s GitHub, Glenn R. In researching for the previous projects, Glenn R solved a lot of the issues and provided insight to all pages I’ve looked at. 

- Uninstalled Unifi controller via sudo dpkg -P unifi. Fresh install of Unifi controller via Glenn R’s GitHub which installs the necessary packages based on the OS and Java requirements: https://community.ui.com/questions/UniFi-Installation-Scripts-or-UniFi-Easy-Update-Script-or-UniFi-Lets-Encrypt-or-UniFi-Easy-Encrypt-/ccbc7530-dd61-40a7-82ec-22b17f027776 

**Resolution:**

Full uninstall of unifi controller via sudo dpkg -P unifi. Full reinstall via Glenn R’s installer https://community.ui.com/questions/UniFi-Installation-Scripts-or-UniFi-Easy-Update-Script-or-UniFi-Lets-Encrypt-or-UniFi-Easy-Encrypt-/ccbc7530-dd61-40a7-82ec-22b17f027776.

**Lessons Learned:**

In documenting the processes, I’ve learned a lot and have been able to retain knowledge to not repeat mistakes. The planning phases have been getting shorter and shorter because I’m learning the networking foundations and processes. I’m feeling a lot more confident in my abilities to manage this network without relying so heavily on the internet. The troubleshooting projects that have led up to this have helped moving the controller immensely. Although, I needed help with the two error messages, I completed all my objectives. The two error messages are related to the installations so I’m not too worried about running all this project myself and I tried thinking through the error messages on my own. I analyzed the code for Glenn R’s install, and it looked alright, it didn’t appear to save credentials (nor did it prompt), and it only looked like it installed a lower version of Java that what’s being deployed. All-in-all, I feel pretty good about completing all my objectives. 
