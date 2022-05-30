# NSM Setup - Splunk
I've setup Splunk about 5 times with an identical deployment but haven't clearly documented the steps.   


Indexer will run on 192.168.50.4. Forwarder will run on 192.168.50.3. 

**Commands to run on the Indexer:**
1. sudo -i  
2. apt update && apt -y upgrade 
3. apt install wget
4. wget -o splunkversion.tgz "splunk.com/splunkversion.tgz" 
5. mv "splunkversion.tgz" "splunk.tgz"
6. tar xvzf splunk.tgz -C /opt
7. cd /opt/splunk/bin
8. ./splunk start --answer-yes --accept-license
9. http://localhost:8000


**Commands to run on Forwarder:**
1. sudo -i 
2. apt update && apt -y upgrade 
3. apt install wget
4. wget -o splunkforwarderverision.tgz "splunk.com/splunkforwarderversion.tgz"
5. mv "splunkforwarderverision.tgz" "splunkforwarder.tgz"
6. tar xvzf splunkforwarder.tgz -C /opt
7. cd /opt/splunkforwarder/bin
8. ./splunk start --answer-yes --accept-license
10. Login to Splunk web, select Settings > Forwarding and receiving
11. Click Add new > Configure Forwarding
12. Enter the receiving hostname/IP:9997 (192.168.50.4:9997)
13. Click Save
14. Select Forwarding defaults > Yes to store a local copy of the indexed data on the forwarder


**Commands to run on the Indexer:** 
1. cd /opt/splunk/bin
2. ./splunk enable app SplunkForwarder -auth <username>:<password>
3. ./splunk restart 

  
**Commands to run on Forwarder:**
1. cd /opt/splunkforwarder/bin
2. ./splunk add forward-server 192.168.50.4:9997
3. ./splunk set deploy-poll 192.168.50.4:8089
4. ./splunk restart 
12. ./splunk add monitor /var/log/auth.log
13. ./splunk add monitor /var/log/syslog
14. ./splunk add monitor /var/log/unifi

  
**Important Notes:**
- Actual links have been redacted as they require Splunk logins. 
- Splunk default ports: Web UI: 8000, Management/splunkd: 8089, Forwarders to Indexer: 9997.



**Helpful Links:**
- Splunk Enterprise Install on Linux: https://docs.splunk.com/Documentation/Splunk/8.2.6/Installation/InstallonLinux
- Start Splunk Enterprise for the first time: https://docs.splunk.com/Documentation/Splunk/8.2.6/Installation/StartSplunkforthefirsttime
- About deployment server and forwarder management: https://docs.splunk.com/Documentation/Splunk/8.2.6/Updating/Aboutdeploymentserver
- Configure deployment clients: https://docs.splunk.com/Documentation/Splunk/8.2.6/Updating/Configuredeploymentclients
- How to Forward Data to Splunk Enterprise: https://docs.splunk.com/Documentation/Forwarder/8.2.2/Forwarder/HowtoforwarddatatoSplunkEnterprise
- Monitor Splunk Enterprise files and directories with the CLI: https://docs.splunk.com/Documentation/Splunk/latest/Data/MonitorfilesanddirectoriesusingtheCLI
- Enable forwarding on a Splunk Enterprise instance -https://docs.splunk.com/Documentation/Splunk/8.2.6/Forwarding/EnableforwardingonaSplunkEnterpriseinstance
- Deploy a  heavy forwarder - https://docs.splunk.com/Documentation/Splunk/8.2.6/Forwarding/Deployaheavyforwarder
Configure Deployment Clients - https://docs.splunk.com/Documentation/Splunk/8.2.6/Updating/Configuredeploymentclients 
