1.	*How will the AP be monitored?* Will this just happen because we port mirror R2 to the NSM? Port mirroring is sending a copy of the network packets seen on one port to another switch port. Since TP-Link is part of ASUS LAN, I believe port forwarding will be enough to monitor both networks.

2.	When trying to setup how the networks will be monitored, I came across this video: https://www.youtube.com/watch?v=J5QJb3O19zI. It looks like this is what I’m wanting to setup. Since I don’t have experience in networking and I’m trying to understand it more, it feels good that I’ve done the research, came up with a plan myself, then went looking, instead of just looking for the answer. In this video, he states to change the channels to something that won’t overlap – channels 1, 6, and 11 don’t overlap. I haven’t learned about channels yet, but it looks like if networks are setup on the same channel, there will be collisions, and the less the channels overlap, the less collisions and faster the network will be. This came up in Considerations #1, if the network speed will decrease with multiple networks – I’ll change the channels to increase the network speed. 

3.	*Is port mirroring safe?* It’s as safe as you make the device that’s connected to WAN. 

4.	*How is everything going to be protected if I don’t have a firewall?* I’m going buy another device if necessary and implement a firewall. 

5.	*How can I implement a firewall with two routers and a computer with one NIC?*These two videos helped:
https://cs.wmich.edu/~rhardin/cs4540/fpSense.pdf, https://www.youtube.com/watch?v=FPgPHJvLmh0 
