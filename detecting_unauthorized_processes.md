## Use Case - Detecting Unauthorized Processes

- I used the Wazuh command monitoring capability to detect when Netcat is running on the Ubuntu endpoint

1. I added the following configuration block to the Wazuh agent /var/ossec/etc/ossec.conf file. This allows to periodically get a list of running processes

<br>

```bash
#Add to this file and save
sudo vi /var/ossec/etc/ossec.conf
   <localfile>
    <log_format>full_command</log_format>
    <alias>process list</alias>
    <command>ps -e -o pid,uname,command</command>
    <frequency>30</frequency>
  </localfile>

sudo systemctl restart wazuh-agent
```

<br>

2. I then installed Netcat and the required dependencies

<br>

```bash
sudo apt install ncat nmap -y
```

<br>

- I then had to configure the following steps on the Wazuh server to create a rule that triggers every time the Netcat program launches

3. I added the following rules to the /var/ossec/etc/rules/local_rules.xml file on the Wazuh server

<br>

```bash
sudo vi /var/ossec/etc/rules/local_rules.xml
  <group name="ossec,">
    <rule id="100050" level="0">
      <if_sid>530</if_sid>
      <match>^ossec: output: 'process list'</match>
      <description>List of running processes.</description>
      <group>process_monitor,</group>
    </rule>
  
    <rule id="100051" level="7" ignore="900">
      <if_sid>100050</if_sid>
      <match>nc -l</match>
      <description>netcat listening for incoming connections.</description>
      <group>process_monitor,</group>
    </rule>
  </group>

sudo systemctl restart wazuh-manager
```

<br>

4. On the monitored Ubuntu endpoint, I then ran nc -l 8000 for 30 seconds

<br>

```bash
sudo nc -l 8000
```

<br>
5. I then visualized this on Wazuh dashboard using **rule.id:(100051)** in Threat Hunting module

<br>

<img width="950" height="488" alt="image" src="https://github.com/user-attachments/assets/a9c82a16-65e0-483b-84e8-0c1d1f8a1dac" />
