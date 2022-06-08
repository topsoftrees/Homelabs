# NSM Setup - Splunk
I've setup Splunk about 5 times with an identical deployment but haven't clearly documented the steps.   


- Indexer will run on 192.168.50.4. Forwarder will run on 192.168.50.3. 

- Splunk shouldn't be running as root so we'll be escalating admin each time. 

**Commands to run on the Indexer:**
1. sudo apt update && apt -y upgrade 
2. sudo apt install wget
3. wget -o splunkversion.tgz "splunk.com/splunkversion.tgz" 
4. mv "splunkversion.tgz" "splunk.tgz"
5. sudo tar xvzf splunk.tgz -C /opt
6. cd /opt/splunk/bin
7. sudo ./splunk start --answer-yes --accept-license
8. http://localhost:8000
9. Login to Splunk Web > Settings > Forwarding and Receiving
10. Configure Forwarding > Add new > receiving hostname/IP:9997 (192.168.50.4:9997) 
11. Configure Receiving > Add new > receiving port (9997) 
12. Forwarding Defaults > Yes to store a local copy of the indexed data on the forwarder
13. sudo ./splunk enable app SplunkForwarder -auth username:password
14. sudo ./splunk restart
15. sudo ./splunk add monitor /var/log/snort/alert **# Snort running in promiscuous mode on the monitor**
16. sudo -i **# the local directory in the next line needs root privileges**
17. cd /opt/splunk/etc/apps/search/local **# the local directory will show up when you start forwarding a file**
18. nano inputs.conf
19. Add the following to /opt/splunk/etc/apps/search/local/inputs.conf:
      > [splunktcp://9997]
      > 
      > connection_host = 192.168.50.4
      > 
      > [monitor:///var/log/snort/alert]
      > 
      > disabled=false
      > 
      > index=main
      > 
      > sourcetype = snort_alert_full
      > 
      > source = snort
19. cd /opt/splunk/bin
20. sudo ./splunk restart

    
    
**Commands to run on Forwarder:** 
1. sudo apt update && apt -y upgrade 
2. sudo apt install wget
3. wget -o splunkforwarderverision.tgz "splunk.com/splunkforwarderversion.tgz"
4. mv "splunkforwarderverision.tgz" "splunkforwarder.tgz"
5. sudo tar xvzf splunkforwarder.tgz -C /opt
6. cd /opt/splunkforwarder/bin
7. sudo ./splunk start --answer-yes --accept-license
8. Enter Splunk Credentials
9. sudo ./splunk add forward-server 192.168.50.4:9997
10. sudo ./splunk add monitor /var/log/syslog
11. sudo ./splunk add monitor /var/log/auth.log
12. sudo -i **# the local directory in the next line needs root privileges**
13. cd /opt/splunk/etc/apps/search/local **# the local directory will show up when you start forwarding a file**
14. nano inputs.conf 
15. Add the following to /opt/splunk/etc/apps/search/local/inputs.conf:
    > [splunktcp://9997]
    > 
    > connection_host = 192.168.50.4
    > 
    > [monitor:///var/log/snort/syslog]
    > 
    > disabled=false
    > 
    > index=main
    > 
    > sourcetype = controller_syslog
    > 
    > source = syslog
    > 
    > [monitor:///var/log/snort/auth.log]
    > 
    > disabled=false
    > 
    > index=main
    > 
    > sourcetype = controller_auth
    > 
    > source = auth
16. cd /opt/splunk/bin
17. sudo ./splunk restart



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
- Configure Deployment Clients - https://docs.splunk.com/Documentation/Splunk/8.2.6/Updating/Configuredeploymentclients 
