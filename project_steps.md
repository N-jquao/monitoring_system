## I Created an Automated Monitoring Infrastructure and Alerting System

```bash
1. The first step of the project was to create a droplet server on DigitalOcean
#This would allow me to create a virtual machine with enough ram and ssd space to deploy the complete Wazuh SIEM tool
#Memory: 4GB
#Computing power: 2 Intel vCPUs
#SSD: 120GB disk (minimum 50GB needed)
#OS: Ubuntu 24.04 (LTS) x64

#After setting up my droplet server on DigitalOCean, I added an incoming firewall rule to allow SSH via port 22 to the droplet for the IP address of my host machine.
#I then logged into the droplet server via PuTTY

#The next step was to install Wazuh on the droplet server

curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a

#This installed the Wazuh Indexer, Wazuh Server, and Wazuh Dashboard on the droplet server

#I was then able to access the Wazuh web interface via https://<WAZUH_DASHBOARD_IP_ADDRESS> using the login details provided when the installation completed
#I had to look in the Wazuh Dashboard config file for the IP address: /etc/wazuh-dashboard/opensearch_dashboards.yml


2. The next step was to deploy the Wazuh Agent on the endpoint I wanted to monitor





```
