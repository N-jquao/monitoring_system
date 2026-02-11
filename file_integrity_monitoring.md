## Use Case - File Integrity Monitoring

- File Integrity Monitoring (FIM) helps in auditing sensitive files and meeting regulatory compliance requirements
- Wazuh has an inbuilt FIM module that monitors file system changes to detect the creation, modification, and deletion of files
<br>

1. I performed the following steps to configure the Wazuh agent to monitor filesystem changes in the /tmp directory
 
- This is because intruders usually copy their files into the /tmp directory, as it is world-writable, and a good spot to launch symlink attacks from
- Attackers can create files in /tmp with expected names before legitimate programs do, tricking the system into using their malicious file
 <br>
 
2. Edited the Wazuh agent vm's /var/ossec/etc/ossec.conf configuration file. Then I added the directory for monitoring within the <syscheck> block

```bash
#Added the /tmp directory line to the bottom of this config file and saved
sudo vi /var/ossec/etc/ossec.conf

  <directories check_all="yes" report_changes="yes" realtime="yes">/tmp</directories>
```
<br>

3. 
