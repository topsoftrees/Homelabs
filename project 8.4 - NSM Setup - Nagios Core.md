# Nagios Setup

You might have to change the versions but at the time of this setup, Nagios Core is running 4.4.7 and the plugins run 2.4.0.

**Run the following on the monitor:**
1. sudo apt install -y autoconf gcc libc6 make wget unzip apahce2 php libapache2-mod-php7.4 libgd-dev
2. sudo apt install openssl libssl-dev
3. sudo apt-get update
4. sudo apt-get install -y autoconf gcc libc6 make wget unzip apache2 php libapache2-mod-php7.4 libgd-dev
5. sudo apt-get install openssl libssl-dev
6. cd /tmp
7. wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.7.tar.gz
8. tar xzf nagioscore.tar.gz
9. cd /tmp/nagioscore-nagios-4.4.7/
10. sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
11. sudo make all
12. sudo make install-groups-users
13. sudo usermod -a -G nagios www-data
14. sudo make install
15. sudo make install-daemoninit
16. sudo make install-commandmode
17. sudo make install-config
18. sudo make install-webconf
19. sudo a2enmod rewrite
20. sudo a2enmod cgi
21. sudo ufw allow Apache
22. sudo ufw reload
23. sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
24. sudo systemctl restart apache2.service
25. sudo systemctl start nagios.service
26. http://localhost/nagios
- When you first login, you'll get errors so run the plugin commands. 
- It will take about 15 min for Nagios to detect services on the localhost. 

**Install Nagios Core Plugins:**
1. sudo apt-get install -y autoconf gcc libc6 libmcrypt-dev make libssl-dev wget bc gawk dc build-essential snmp libnet-snmp-perl gettext
2. cd /tmp
3. wget --no-check-certificate -O nagios-plugins.tar.gz https://github.com/nagios-plugins/nagios-plugins/archive/release-2.4.0.tar.gz
4. tar zxf nagios-plugins.tar.gz
5. cd /tmp/nagios-plugins-release-2.4.0/
6. sudo ./tools/setup
7. sudo ./configure
8. sudo make
9. sudo make install
10. http://localhost/nagios


**Common Nagios Core Commands:**
- sudo systemctl start nagios.service
- sudo systemctl stop nagios.service
- sudo systemctl restart nagios.service
- sudo systemctl status nagios.service

**Add monitors:**



**Helpful Links:**
- Installing Nagios Core from source: https://support.nagios.com/kb/article/nagios-core-installing-nagios-core-from-source-96.html#Ubuntu
- Monitoring Routers and Switches: https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/monitoring-routers.html
