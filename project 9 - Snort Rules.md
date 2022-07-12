# LOCAL RULES

This is the first round of rules - I'll still be updating them over time. Since there are so many spam and malware IP addresses, I split this to be 2- and 3- id numbers for readability and easy updates. For a future project, I may try to write a python script to pull the spam and malware IPs from Cisco Talos to automatically add more rules. 


**SSH Rules:**
- Resource: IANA SSH Parameters
> alert tcp any any -> 192.168.50.0/24 22 (msg: "SSH_PUBLICKEY_SUCCESS"; sid:10000001; rev:001;)
> 
> drop tcp !(192.168.50.0/24) any -> 192.168.50.0/24 22 (msg: "SSH_EXTERNAL_ATTEMPT"; sid:10000002; rev:001;)
> 
> alert tcp any any -> 192.168.50.0/24 22 (msg: "SSH_PUBLICKEY_ACCESSED_DENIED"; sid:10000003; rev: 001;)

**ICMP Rules:**
- Resource: IANA ICMP Parameters
> alert icmp any any -> any any (msg: "ICMP Destination Network Unreachable"; itype:3; icode:0; sid:10000004; rev:001;)
> 
> alert icmp any any -> any any (msg: "ICMP Destination Host Unreachable"; itype:3; icode:1; sid:10000005; rev:001;)
> 
> alert icmp any any -> 192.158.50.0/24 any (msg: "ICMP Destination Port Unreachable"; itype:3; icode:4; sid: 10000006; rev:0>

**DNS Rules:**
- Resource: IANA DNS Parameters
> alert udp any any -> 192.168.50.0/24 53 (msg: "DNS Record"; sid: 10000007; rev:001;)

**Port Scans:**
- Resource: IANA TCP parameters
> alert tcp any any -> 192.168.50.0/24 any (msg: "NMAP XMAS Tree Scan"; flags: FPU; sid: 10000008; rev:001;)


**Services that shouldn't be running:**
- Resource: Current Network Setup
> block tcp any any -> 192.168.50.0/24 389 (msg: "LDAP attempt"; sid: 10000009; rev:001;)
> 
> block udp any any -> 192.168.50.0/24 389 (msg: "LDAP attempt"; sid: 10000010; rev:001;)
> 
> block udp any any -> 192.168.50.0/24 161 (msg: "SNMP attempt"; sid: 10000011; rev:001;)
> 
> block udp any any -> 192.168.50.0/24 137 (msg: "NetBIOS Name Services attempt"; sid: 10000012; rev:001;)
> 
> block tcp any any -> 192.168.50.0/24 445 (msg: "SMB over TCP attempt"; sid: 10000013; rev:001;)
> 
> block tcp any any -> 192.168.50.0/24 3389 (msg: "RDP attempt"; sid: 10000014; rev:001;)


**SPAM:**
- Resource: Cisco Talos 
> block tcp 87.253.233.2 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000000; rev:001;)
> 
> block tcp 95.168.184.219 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000001; rev:001;)
> 
> block tcp 156.70.63.221 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000002; rev:001;)
> 
> block tcp 13.111.157.219 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000003; rev:001;)
> 
> block tcp 13.110.227.231 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000004; rev:001;)
> 
> block tcp 13.110.227.239 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000005; rev:001;)
> 
> block tcp 199.15.224.209 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000006; rev:001;)
> 
> block tcp 13.111.74.23 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000007; rev:001;)
> 
> block tcp 159.183.158.211 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000008; rev:001;)
> 
> block tcp 13.111.77.82 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000009; rev:001;)
> 
> block tcp 45.61.162.183 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000010; rev:001;)
> 
> block tcp 23.171.176.114 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000011; rev:001;)
> 
> block tcp 8.12.53.105   any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000012; rev:001;)
> 
> block tcp 13.110.227.233 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000013; rev:001;)
> 
> block tcp 5.104.111.19 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000014; rev:001;)
> 
> block tcp 8.12.53.104   any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000015; rev:001;)
> 
> block tcp 156.70.25.145 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000016; rev:001;)
> 
> block tcp 209.85.216.65 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000017; rev:001;)
> 
> block tcp 209.85.216.67 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000018; rev:001;)
> 
> block tcp 209.85.216.68 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000019; rev:001;)
> 
> block tcp 209.85.216.66 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000020; rev:001;)
> 
> block tcp 159.127.162.136 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000021; rev:001;)
> 
> block tcp 156.70.53.224 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000022; rev:001;)
> 
> block tcp 13.110.227.232 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000023; rev:001;)
> 
> block tcp 159.127.162.133 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000024; rev:001;)
> 
> block tcp 45.61.162.184 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000025; rev:001;)
> 
> block tcp 209.85.214.196 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000026; rev:001;)
> 
> block tcp 209.85.214.193 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000027; rev:001;)
> 
> block tcp 209.85.214.194 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000028; rev:001;)
> 
> block tcp 13.110.227.234 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000029; rev:001;)
> 
> block tcp 209.85.214.195 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000030; rev:001;)
> 
> block tcp 209.85.215.193 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000031; rev:001;)
> 
> block tcp 209.85.215.195 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000032; rev:001;)
> 
> block tcp 209.85.210.194 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000033; rev:001;)
> 
> block tcp 209.85.215.194 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000034; rev:001;)
> 
> block tcp 13.110.224.155 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000035; rev:001;)
> 
> block tcp 167.89.36.103 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000036; rev:001;)
> 
> block tcp 209.85.210.195 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000037; rev:001;)
> 
> block tcp 23.171.176.143 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000038; rev:001;)
> 
> block tcp 209.85.215.196 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000039; rev:001;)
> 
> block tcp 209.85.210.193 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000040; rev:001;)
> 
> block tcp 168.245.117.167 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000041; rev:001;)
> 
> block tcp 216.245.204.37 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 200000042; rev:001;)
> 
> block tcp 209.85.210.196 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000043; rev:001;)
> 
> block tcp 168.245.20.49 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000044; rev:001;)
> 
> block tcp 13.110.224.14 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000045; rev:001;)
> 
> block tcp 194.99.47.202 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000046; rev:001;)
> 
> block tcp 194.99.45.39 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000047; rev:001;)
> 
> block tcp 194.99.47.217 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000048; rev:001;)
> 
> block tcp 194.99.45.41 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000049; rev:001;)
> 
> block tcp 194.99.45.47 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000050; rev:001;)
> 
> block tcp 194.99.45.36 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000051; rev:001;)
> 
> block tcp 193.37.41.37 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000052; rev:001;)
> 
> block tcp 192.142.134.252 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000053; rev:001;)
> 
> block tcp 173.254.246.164 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000054; rev:001;)
> 
> block tcp 173.254.246.193 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000055; rev:001;)
> 
> block tcp 168.235.85.139 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000056; rev:001;)
> 
> block tcp 173.254.246.193 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000057; rev:001;)
> 
> block tcp 168.235.85.139 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000058; rev:001;)
> 
> block tcp 89.163.219.189 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000059; rev:001;)
> 
> block tcp 89.163.132.117 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000060; rev:001;)
> 
> block tcp 89.163.230.186 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000061; rev:001;)
> 
> block tcp 5.199.133.234 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000062; rev:001;)
> 
> block tcp 213.202.238.68 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000063; rev:001;)
> 
> block tcp 89.163.218.132 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000064; rev:001;)
> 
> block tcp 62.141.46.167 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000065; rev:001;)
> 
> block tcp 89.163.214.221 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000066; rev:001;)
> 
> block tcp 84.16.227.104 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000067; rev:001;)
> 
> block tcp 94.130.132.126 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000068; rev:001;)
> 
> block tcp 217.79.178.57 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000069; rev:001;)
> 
> block tcp 81.30.156.23 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000070; rev:001;)
> 
> block tcp 77.90.150.5   any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000071; rev:001;)
> 
> block tcp 77.90.150.30 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000072; rev:001;)
> 
> block tcp 77.90.150.23 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000073; rev:001;)
> 
> block tcp 37.48.115.118 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000074; rev:001;)
> 
> block tcp 95.211.247.174 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 20000075; rev:001;)
> 
> block tcp 62.128.110.144 any -> 192.168.0.0/16 any (msg: "SPAM attempt"; sid: 200000076; rev:001;)


**Malware:**
- Resource: Cisco Talos
> block tcp 168.245.119.24 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000001; rev:001;)
> 
> block tcp 192.228.105.15 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000002; rev:001;)
> 
> block tcp 64.44.101.94 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000003; rev:001;)
> 
> block tcp 159.223.73.217 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000004; rev:001;)
> 
> block tcp 104.168.213.78 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000005; rev:001;)
> 
> block tcp 198.251.79.181 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000006; rev:001;)
> 
> block tcp 104.168.154.242 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000007; rev:001;)
> 
> block tcp 104.168.159.151 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000008; rev:001;)
> 
> block tcp 137.184.196.112 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000009; rev:001;)
> 
> block tcp 137.184.73.65 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000010; rev:001;)
> 
> block tcp 159.223.116.229 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000011; rev:001;)
> 
> block tcp 172.93.128.150 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000012; rev:001;)
> 
> block tcp 67.23.227.192 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000013; rev:001;)
> 
> block tcp 159.223.116.169 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000014; rev:001;)
> 
> block tcp 104.168.200.160 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000015; rev:001;)
> 
> block tcp 96.125.164.115 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000016; rev:001;)
> 
> block tcp 159.223.117.101 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000017; rev:001;)
> 
> block tcp 151.80.161.116 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000018; rev:001;)
> 
> block tcp 151.106.39.222 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000019; rev:001;)
> 
> block tcp 45.141.239.121 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000020; rev:001;)
> 
> block tcp 37.0.8.194 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000021; rev:001;)
> 
> block tcp 37.0.8.253 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000022; rev:001;)
> 
> block tcp 37.0.11.100 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000023; rev:001;)
> 
> block tcp 45.141.239.47 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000024; rev:001;)
> 
> block tcp 84.16.252.70 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000025; rev:001;)
> 
> block tcp 212.192.246.129 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000026; rev:001;)
> 
> block tcp 5.230.72.31 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000027; rev:001;)
> 
> block tcp 185.222.57.247 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000028; rev:001;)
> 
> block tcp 185.222.57.243 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000029; rev:001;)
> 
> block tcp 185.222.57.140 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000030; rev:001;)
> 
> block tcp 185.222.57.229 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000031; rev:001;)
> 
> block tcp 185.222.57.158 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000032; rev:001;)
> 
> block tcp 92.119.113.152 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000033; rev:001;)
> 
> block tcp 92.119.113.91 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000034; rev:001;)
> 
> block tcp 45.130.138.136 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000035; rev:001;)
> 
> block tcp 45.130.138.252 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000036; rev:001;)
> 
> block tcp 45.130.138.175 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000037; rev:001;)
> 
> block tcp 45.130.138.214 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000038; rev:001;)
> 
> block tcp 45.130.138.176 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000039; rev:001;)
> 
> block tcp 216.108.228.98 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000040; rev:001;)
> 
> block tcp 192.34.84.58 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000041; rev:001;)
> 
> block tcp 162.243.109.74 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 300000042; rev:001;)
> 
> block tcp 206.189.225.230 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000043; rev:001;)
> 
> block tcp 194.31.98.44 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000044; rev:001;)
> 
> block tcp 194.31.98.45 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000045; rev:001;)
> 
> block tcp 194.31.98.198 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000046; rev:001;)
> 
> block tcp 45.66.249.32 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000047; rev:001;)
> 
> block tcp 195.88.24.153 any -> 192.168.0.0/16 any (msg: "Malware attempt"; sid: 30000048; rev:001;)

