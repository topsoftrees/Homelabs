# A Full Unifi Reset Homelab

I went to install a Docker image of Splunk but it failed to install because there were still bits from a previous installation. I wasn’t able to find any of the files so I wiped the device. Unfortunately, I wasn’t thinking about how troubling Unifi has been in the past. I really don’t like Unifi and wouldn’t recommend it to anyone. The process is a huge pain in the ass and isn’t as user-friendly as Unifi advertises. I’ve documented a similar process in Project 6: Controller Replaced (2/12/22), but it was not successful in this case, controller wasn’t coming online. Hopefully, if this were to ever happen in the future, this will get me through from start to finish.

Pardon the short, incomplete sentences in the troubleshooting, I wrote it as I do work notes in tickets.

## Troubleshooting
- Installed the controller via Unifi, cannot detect any devices.
- Installed the controller via Glenn R, cannot detect any devices.
- Tried to just reset AP via reset button but no luck as button detached from the motherboard and is loose inside the AP shell.
- Unable to remote into the AP because the controller doesn’t exist anymore.
- Broke apart AP apparatus and secured the reset button.
- Installed the controller via Unifi, cannot detect any devices (see bolded below, the controller wasn’t online and button didn’t exist yet).
- Installed the controller via Glenn R, cannot detect any devices (see bolded below, the controller wasn’t online and button didn’t exist yet).
- Reset the AP via reset button, still shows as offline and cannot be detected.
- SSHed into the AP via ssh ubnt@192.168.50.8, ran set-inform [https://192.168.50.3:8443](https://192.168.50.3:8443).
- SSHed into the AP via ssh ubnt@192.168.50.8, ran set-inform [http://192.168.50.3:8080](http://192.168.50.3:8080).
- Reset the AP via TFTP, still shows as offline and cannot be detected.
- SSHed into AP via ssh ubnt@192.168.50.8, ran set-inform [https://192.168.50.3:8443](https://192.168.50.3:8443), controller doesn’t detect device.
- SSHed into AP via ssh ubnt@192.168.50.8, ran set-inform [http://192.168.50.3:8080](http://192.168.50.3:8080), controller doesn’t detect device.
- Using Unifi mobile app, ran the controller, controller shows as offline.
- Using Unifi mobile app, configured AP as standalone then changed AP IP to 192.168.50.8.
- Logged into Unifi mobile app, AP shows as trying to Adopt but switches between Adopting and Offline.
- Reset AP via reset button.
- Using Unifi mobile app, configured AP as standalone then changed AP IP to 192.168.50.8. Logged into app, AP will take time to Adopt the device.
- Went to [https://192.168.50.3:8443](https://192.168.50.3:8443) or [http://192.168.50.3:8080](http://192.168.50.3:8080), follow the prompts ****then skip “Detecting a Device to Create a Network.” The controller is now live.
- Since controller is live, site changes from [https://192.168.50.3:8443](https://192.168.50.3:8443) or [http://192.168.50.3:8080](http://192.168.50.3:8080), to [https://unifi.ui.com](https://unifi.ui.com).
- Using Unifi mobile app, Adopted AP. Successfully able to access controller and AP via controller console.

## Resolution
1. Run TFTP Recovery for a bricked Unifi AP: [https://help.ui.com/hc/en-us/articles/204910124-UniFi-Network-Leveraging-TFTP-Recovery-Advanced-Users-Only-](https://help.ui.com/hc/en-us/articles/204910124-UniFi-Network-Leveraging-TFTP-Recovery-Advanced-Users-Only-). Make sure you pull the latest firmware for the AP. 
2. Installed the Network Controller from [https://www.ui.com/download/unifi/unifi-ap-ac-lite/uap-ac-lite/unifi-network-controller-51019-debianubuntu-linux-and-unifi-cloud-key](https://www.ui.com/download/unifi/unifi-ap-ac-lite/uap-ac-lite/unifi-network-controller-51019-debianubuntu-linux-and-unifi-cloud-key) or Glenn R’s Unifi script.  [https://community.ui.com/questions/UniFi-Installation-Scripts-or-UniFi-Easy-Update-Script-or-UniFi-Lets-Encrypt-or-UniFi-Easy-Encrypt-/ccbc7530-dd61-40a7-82ec-22b17f027776](https://community.ui.com/questions/UniFi-Installation-Scripts-or-UniFi-Easy-Update-Script-or-UniFi-Lets-Encrypt-or-UniFi-Easy-Encrypt-/ccbc7530-dd61-40a7-82ec-22b17f027776)
3. Go to [https://192.168.50.3:8443](https://192.168.50.3:8443) or [http://192.168.50.3:8080](http://192.168.50.3:8080), follow the prompts **then skip “Detecting a Device to Create a Network.”** This will take you to the Unifi Controller. You’ll still probably won’t be able to detect the AP. 
4. Download the Unifi Controller AP to your mobile device. Setup as a standalone AP then scan the QR code on the back of the AP, configure the standalone network, and change the AP’s address to 192.168.50.8. 
5. Sign into your Unifi account on your mobile device, you should then be prompted for the AP to be Adopted. It will take quite a bit of time but it will eventually work. 
    1. Go to the Network Controller, if the AP switches between Adopting and Offline, reset the device via the reset button. 
6. The Controller will change from [https://192.168.50.3:8443](https://192.168.50.3:8443) or [http://192.168.50.3:8080](http://192.168.50.3:8080), to [https://unifi.ui.com](https://unifi.ui.com). Login to [https://unifi.ui.com](https://unifi.ui.com).
7. Go into the Controller settings, scroll all the way down to “Change override inform host” to “192.168.50.3.”
8. [https://unifi.ui.com](https://unifi.ui.com) will no longer work to access the controller, but [https://192.168.50.3:8443](https://192.168.50.3:8443) will be accessible.
