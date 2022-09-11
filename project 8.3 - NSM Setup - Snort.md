# Snort

I'll be using an IDS in promiscuous mode to monitor internal and external network to understand logs and detect attacks.

**Run the following on the NSM:**
```bash
sudo apt -y install snort
# Select an interface - mine will be enp1s0 - ifconfig the server to find the interface
# Enter the local network you would like to monitor - 192.168.0.0/16 
# Will need to answer the prompt to double check the interface 
mkdir /usr/local/etc/rules
wget https://www.snort.org/downloads/community/snort3-community-rules.tar.gz
sudo tar xzf snort3-community-rules.tar.gz -C /usr/local/etc/rules/
cd /etc/snort/
sudo nano snort.conf 
# Modify the following: ipvar HOME_NET 192.168.0.0/16 & ipvar EXTERNAL_NET any
sudo snort -T -i enp1s0 -c /etc/snort/snort.conf **# testing rules on the interface against the rules file** 
sudo ip link set enp1s0 promisc on **# enable promiscuous mode on NIC**
sudo snort -q -l /var/log/snort -i enp1s0 -A full -c /etc/snort/snort.conf
```

**Make your own rules:**
```bash
cd /etc/snort/rules
sudo nano local.rules
# Add rules here
```

## Helpful Commands:
- snort —version # confirm snort is installed
- ls /etc/snort # just list all snort files because this is where snort runs
- /etc/snort/rules # where rules are
- /etc/snort/rules/local.rules # site specific rules
- /etc/snort/snort.conf # make a copy of this file so we can learn
- -l # logs by default to /var/log/snort
- .log files aren’t human readable - need to run in Alert mode (-A) for it to be decoded


## Helpful Documentation:
- Rules: https://snort.org/downloads/#rule-downloads
- Snort Documentation: https://snort.org/documents
- man snort
