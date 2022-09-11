# NSM Setup - Splunk
I've setup Splunk about 5 times with an identical deployment but haven't clearly documented the steps.   


- Indexer will run on 192.168.50.4. Forwarder will run on 192.168.50.3. 

- Splunk shouldn't be running as root so we'll be escalating admin each time. 

**Commands to run on the Indexer:**
```bash
sudo apt update && apt -y upgrade 
sudo apt install wget
wget -o splunkversion.tgz "splunk.com/splunkversion.tgz" 
mv "splunkversion.tgz" "splunk.tgz"
sudo tar xvzf splunk.tgz -C /opt
cd /opt/splunk/bin
sudo ./splunk start --answer-yes --accept-license
# Launch http://localhost:8000
# Login to Splunk Web > Settings > Forwarding and Receiving
# Configure Forwarding > Add new > receiving hostname/IP:9997 (192.168.50.4:9997) 
# Configure Receiving > Add new > receiving port (9997) 
# Forwarding Defaults > Yes to store a local copy of the indexed data on the forwarder
sudo ./splunk enable app SplunkForwarder -auth username:password
sudo ./splunk restart
sudo ./splunk add monitor /var/log/snort/alert **# Snort running in promiscuous mode on the monitor**
```

    
**Commands to run on Forwarder:** 
```bash 
sudo apt update && apt -y upgrade 
sudo apt install wget
wget -o splunkforwarderverision.tgz "splunk.com/splunkforwarderversion.tgz"
mv "splunkforwarderverision.tgz" "splunkforwarder.tgz"
sudo tar xvzf splunkforwarder.tgz -C /opt
cd /opt/splunkforwarder/bin
sudo ./splunk start --answer-yes --accept-license
# Enter Splunk Credentials
```

_Light Forwarder - on the Forwarder:_
```bash
sudo ./splunk add forward-server 192.168.50.4:9997 -auth username:password
sudo ./splunk restart
sudo ./splunk add monitor /var/log/auth.log
```

_Heavy Forwarder - on the forwarder:_
```bash
sudo ./splunk add forward-server 192.168.50.4:9997
sudo ./splunk restart
sudo ./splunk set deploy-poll 192.168.50.4:8089
sudo ./splunk restart
```

      
**Add the Inputs:**
```bash
sudo -i **# the local directory in the next line needs root privileges**
cd /opt/splunk/etc/apps/search/local **# the local directory will show up when you start forwarding a file**
nano inputs.conf 
# Add the following to /opt/splunk/etc/apps/search/local/inputs.conf:
    [splunktcp://9997]
    connection_host = 192.168.50.4
    [monitor:///var/log/snort/syslog]
    disabled=false
    index=main
    sourcetype = controller_syslog
    source = syslog
    [monitor:///var/log/snort/auth.log]
    disabled=false
    index=main
    sourcetype = controller_auth
    source = auth
cd /opt/splunk/bin
sudo ./splunk restart
```


## Important Notes:
- Actual links have been redacted as they require Splunk logins. 
- Splunk default ports: Web UI: 8000, Management/splunkd: 8089, Forwarders to Indexer: 9997.

## Helpful Links:
- Splunk Enterprise Install on Linux: https://docs.splunk.com/Documentation/Splunk/8.2.6/Installation/InstallonLinux
- Start Splunk Enterprise for the first time: https://docs.splunk.com/Documentation/Splunk/8.2.6/Installation/StartSplunkforthefirsttime
- About deployment server and forwarder management: https://docs.splunk.com/Documentation/Splunk/8.2.6/Updating/Aboutdeploymentserver
- Configure deployment clients: https://docs.splunk.com/Documentation/Splunk/8.2.6/Updating/Configuredeploymentclients
- How to Forward Data to Splunk Enterprise: https://docs.splunk.com/Documentation/Forwarder/8.2.2/Forwarder/HowtoforwarddatatoSplunkEnterprise
- Monitor Splunk Enterprise files and directories with the CLI: https://docs.splunk.com/Documentation/Splunk/latest/Data/MonitorfilesanddirectoriesusingtheCLI
- Enable forwarding on a Splunk Enterprise instance -https://docs.splunk.com/Documentation/Splunk/8.2.6/Forwarding/EnableforwardingonaSplunkEnterpriseinstance
- Deploy a  heavy forwarder - https://docs.splunk.com/Documentation/Splunk/8.2.6/Forwarding/Deployaheavyforwarder
- Configure Deployment Clients - https://docs.splunk.com/Documentation/Splunk/8.2.6/Updating/Configuredeploymentclients 
