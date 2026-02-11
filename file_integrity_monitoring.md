## Use Case - File Integrity Monitoring

- File Integrity Monitoring (FIM) helps in auditing sensitive files and meeting regulatory compliance requirements
- Wazuh has an inbuilt FIM module that monitors file system changes to detect the creation, modification, and deletion of files
<br>

1. I performed the following steps to configure the Wazuh agent to monitor filesystem changes in the /tmp directory
 
- This is because intruders usually copy their files into the /tmp directory, as it is world-writable, and a good spot to launch symlink attacks from
- Attackers can create files in /tmp with expected names before legitimate programs do, tricking the system into using their malicious file
 <br>
 
2. I Edited the Wazuh agent vm's /var/ossec/etc/ossec.conf configuration file. Then I added the directory for monitoring within the <syscheck> block

```bash
#Added the /tmp directory line to the bottom of this config file and saved
sudo vi /var/ossec/etc/ossec.conf

  <directories check_all="yes" report_changes="yes" realtime="yes">/tmp</directories>
```
<br>

3. I then restarted the Wazuh agent to apply the configuration changes

```bash
sudo systemctl restart wazuh-agent
```

- As an alternative to local configurations on the Wazuh agents, it is possible to centrally configure groups of agents

<br>

4. To test the configuration I created a text file in the /tmp dir called redflag.txt, then waited 5 seconds

```bash
cd /tmp

sudo touch redflag.txt
```

<img width="369" height="81" alt="image" src="https://github.com/user-attachments/assets/2df14992-0061-4a3d-9880-d9fa6fc33f51" />
<br>
<br>

- There was an alert under File Integrity Monitoring Events stating that the "regflag.txt" file was added to the /tmp directory
<img width="951" height="451" alt="image" src="https://github.com/user-attachments/assets/3b1fb136-1ad9-4441-a29b-0fb03871ea78" />


<br>
<br>

5. I then added content to the file and saved it

```bash
cd /tmp

sudo vi redflag.txt

#Added this to the file and saved
 "This is an attack from an intruder"
```

<img width="952" height="478" alt="image" src="https://github.com/user-attachments/assets/c2ed0df8-6215-4b99-b5ee-57812a9e0fc6" />

<br>
<br>

<img width="944" height="491" alt="image" src="https://github.com/user-attachments/assets/788ab0eb-bfc1-4bb9-9201-46cc4fa62b38" />

<br>
<br>

- To visualize the alerts I needed to add the following filters to the events section of File Integrity Monitoring
<br>
--> rule.id: is one of 550,553,554
