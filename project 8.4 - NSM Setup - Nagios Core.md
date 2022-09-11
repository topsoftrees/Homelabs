# Nagios Setup

You might have to change the versions but at the time of this setup, Nagios Core is running 4.4.7 and the plugins run 2.4.0.

**Run the following on the monitor:**
```bash
sudo apt install -y autoconf gcc libc6 make wget unzip apahce2 php libapache2-mod-php7.4 libgd-dev
sudo apt install openssl libssl-dev
sudo apt-get update
sudo apt-get install -y autoconf gcc libc6 make wget unzip apache2 php libapache2-mod-php7.4 libgd-dev
sudo apt-get install openssl libssl-dev
cd /tmp
wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.7.tar.gz
tar xzf nagioscore.tar.gz
cd /tmp/nagioscore-nagios-4.4.7/
sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
sudo make all
sudo make install-groups-users
sudo usermod -a -G nagios www-data
sudo make install
sudo make install-daemoninit
sudo make install-commandmode
sudo make install-config
sudo make install-webconf
sudo a2enmod rewrite
sudo a2enmod cgi
sudo ufw allow Apache
sudo ufw reload
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
sudo systemctl restart apache2.service
sudo systemctl start nagios.service
# Launch http://localhost/nagios
```
- When you first login, you'll get errors so run the plugin commands. 
- It will take about 15 min for Nagios to detect services on the localhost. 

**Install Nagios Core Plugins:**
```bash
sudo apt-get install -y autoconf gcc libc6 libmcrypt-dev make libssl-dev wget bc gawk dc build-essential snmp libnet-snmp-perl gettext
cd /tmp
wget --no-check-certificate -O nagios-plugins.tar.gz https://github.com/nagios-plugins/nagios-plugins/archive/release-2.4.0.tar.gz
tar zxf nagios-plugins.tar.gz
cd /tmp/nagios-plugins-release-2.4.0/
sudo ./tools/setup
sudo ./configure
sudo make
sudo make install
# Launch http://localhost/nagios
```

## Common Nagios Core Commands:
- sudo systemctl start nagios.service
- sudo systemctl stop nagios.service
- sudo systemctl restart nagios.service
- sudo systemctl status nagios.service

## Helpful Links:
- Installing Nagios Core from source: https://support.nagios.com/kb/article/nagios-core-installing-nagios-core-from-source-96.html#Ubuntu
- Monitoring Routers and Switches: https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/monitoring-routers.html
