## Use Case - Blocking a Known Malicious Actor

1. I demonstrate how to block malicious IP addresses from accessing web resources on a web server
<br>

2. I set up an Apache web server on an Ubuntu endpoint, and tried to access it from an RHEL endpoint

- On the Ubuntu endpoint

```bash
sudo apt update

sudo apt install apache2

sudo ufw status

sudo ufw app list

sudo ufw allow 'Apache'
```
<br>

3. I then checked the status of the Apache service to verify the web server is running on the Ubuntu endpoint being monitored

```bash
sudo systemctl status apache2
```
<br>

4. I then used the curl command to view the Apache landing page, and verify the installation on the Ubuntun endpoint being monitored

```bash
curl http://<UBUNTU_ENDPOINT_IP>
```
<br>

5. I then added the following to /var/ossec/etc/ossec.conf file to configure the Wazuh agent and monitor the Apache access logs

```bash
#Add this to the file and save
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/apache2/access.log</location>
</localfile>

sudo systemctl restart wazuh-agent
```
<br>

6. I then needed to perform the following steps on the Wazuh server to add the IP address of the RHEL endpoint to a CDB list, and configure rules and active response

```bash
sudo apt update

sudo apt install -y wget

sudo wget https://iplists.firehol.org/files/alienvault_reputation.ipset -O /var/ossec/etc/lists/alienvault_reputation.ipset

#Add the RHEL Attacker server's IP to the end of this file and save
sudo vi /var/ossec/etc/lists/alienvault_reputation.ipset

#Downloaded this script to convert from the .ipset format to the .cdb list format
sudo wget https://wazuh.com/resources/iplist-to-cdblist.py -O /tmp/iplist-to-cdblist.py

#Converted the alienvault_reputation.ipset file to a .cdb format using the previously downloaded script
sudo /var/ossec/framework/python/bin/python3 /tmp/iplist-to-cdblist.py /var/ossec/etc/lists/alienvault_reputation.ipset /var/ossec/etc/lists/blacklist-alienvault

#Assigned the correct permissions and ownership to the generated file
sudo chown wazuh:wazuh /var/ossec/etc/lists/blacklist-alienvault
```
<br>

7. 
