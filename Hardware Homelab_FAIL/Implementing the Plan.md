1.	Verified both ASUS and TP-Link have USB ports
2.	Reset ASUS router using method 1
3.	Setup ASUS network 
4.	Enabled DHCP from 192.168.50.3 (need to add static 192.168.50.2
5.	Downloaded pfsense
    a.	Downloaded memstick one
    b.	Ran gunzip /filepathtoimage to decompress the file
    c.	Wiped the disk in Disk util
    d.	Ran the following based on insturctions from netgate found on pfsense’s website
        i.	diskutil unmountDisk /dev/usbstickname
        ii.	dd if=imagefilepath.img of=/dev/usbstickname bs=4m (this took a long time because the file size)
        iii.	diskutil eject /dev/usbstickname
6.	Reset TP-Link router
    a.	Challenge: how should the interfaces connect? --------WAN-FW-LAN ------ WAN or LAN-Router?
        i.	The WAN-Router connection failed. I couldn’t get to the router and the firewall was redirecting to the router.
        ii.	TP-link-192.168.0.1
        iii.	Asus 192.168.50.1
7.	Plugged in pfSense into both TP and ASUS, neither one detected the USB. Unable to implement hardware firewall. Unable to implement a software firewall because NSM will require 2 NICs which I don’t have. 
