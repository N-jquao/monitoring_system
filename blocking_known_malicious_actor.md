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
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/apache2/access.log</location>
</localfile>

sudo systemctl restart wazuh-agent
```
