# Automating Homelab Deployment - Docker
```bash
sudo apt install docker.io -y

# Since we want to monitor thehome network, we'll need to create a network then tie it to the containers
# This method allows for the host MAC address to be shared with the containers which allows the multiple MACS for the containers but the switch port reads the MAC address of everything as the host, not multiple MACS. This would be very similar to running the software on the host instead of docker. 
sudo docker network create -d ipvlan --subnet 192.168.50.0/24 --gateway 192.168.50.1 -o parent=enp0s3 network50


# OpenVAS
sudo docker pull mikesplain/openvas --network network50 --ip 192.168.50.9
sudo docker run -d -p 443:443 --name openvas mikesplain/openvas
# Launch https://localhost, login admin:admin

# Splunk Enterprise 
sudo docker pull splunk --network network50 --ip 192.168.50.10
docker run -d -p 8000:8000 -e "SPLUNK_START_ARGS=--accept-license" -e "SPLUNK_PASSWORD=<password>" --name splunk splunk/splunk:latest
# Launch http://localhost:8000
```



## Types of Networks
```bash
# Since we want to monitor the home network, we'll need to create a network then tie it to the containers
# The problem with this type of network is not all switches allow for port to have multiple MAC addresses like this still allows. 
# We need to find a way around the above issue.
docker network create -d macvlan --subnet 192.168.50.0/24 --gateway 192.168.50.1 -o parent=enp0s3 network50
sudo ip link set enp0s3 promisc on # this will allow for multiple MAC addresses on one switch port 


# Since we want to monitor the home network, we'll need to create a network then tie it to the containers
# This method allows for the host MAC address to be shared with the containers which allows the multiple MACS for the containers but the switch port reads the MAC address of everything as the host, not multiple MACS. This would be very similar to running the software on the host instead of docker. 
docker network create -d ipvlan --subnet 192.168.50.0/24 --gateway 192.168.50.1 -o parent=enp0s3 network50

# If the above doesn't make sense, please check out https://www.youtube.com/watch?v=bKFMS5C4CG0. 
```
