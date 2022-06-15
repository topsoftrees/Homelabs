# Snort

I'll be using an IDS in promiscuous mode to monitor internal and external network to understand logs and detect attacks.

**Run the following on the NSM:**
1. sudo apt -y install snort
2. Select an interface - mine will be enp1s0 - ifconfig the server to find the interface
3. Enter the local network you would like to monitor - 192.168.0.0/16 
4. Will need to answer the prompt to double check the interface 
5. mkdir /usr/local/etc/rules
6. wget https://www.snort.org/downloads/community/snort3-community-rules.tar.gz
7. sudo tar xzf snort3-community-rules.tar.gz -C /usr/local/etc/rules/
8. cd /etc/snort/
9. sudo nano snort.conf 
10. Modify the following:
    > ipvar HOME_NET 192.168.0.0/16
    > 
    > ipvar EXTERNAL_NET any
11. sudo snort -T -i enp1s0 -c /etc/snort/snort.conf **# testing rules on the interface against the rules file** 
12. sudo ip link set enp1s0 promisc on **# enable promiscuous mode on NIC**
13. sudo snort -q -l /var/log/snort -i enp1s0 -A full -c /etc/snort/snort.conf

**Make your own rules:**
1. cd /etc/snort/rules
2. sudo nano local.rules
3. Add rules here

**Helpful Commands:**
- snort —version # confirm snort is installed
- ls /etc/snort # just list all snort files because this is where snort runs
- /etc/snort/rules # where rules are
- /etc/snort/rules/local.rules # site specific rules
- /etc/snort/snort.conf # make a copy of this file so we can learn
- -l # logs by default to /var/log/snort
- .log files aren’t human readable - need to run in Alert mode (-A) for it to be decoded


**Helpful Documentation:** 
