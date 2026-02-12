## Use Case - Detecting A Brute Force Attack

- Brute-forcing is a common attack vector that threat actors use to gain unauthorized access to endpoints and services
- Services like SSH on Linux endpoints and RDP on Windows endpoints are usually prone to brute-force attacks
- Wazuh identifies brute-force attacks by correlating multiple authentication failure events

1. On the Attacker endpoint (RHEL vm) I installed Hydra - this will be used to execute brute force attack

```bash
sudo apt update
sudo apt install -y hydra
```

<br>

2. Attack emulation

- I created a text file with 10 random passwords on the Attacker endpoint (RHEL vm)
- I ran Hydra from the attacker endpoint to execute brute-force attacks against the RHEL endpoint

```bash
sudo hydra -l <username> -P <PASSWD_LIST.txt> <RHEL_IP> ssh
```

<br>

3. To visualize the attack I went to the Threat Hunting module on Wazuh dashboard and added these filters to query the alerts

Linux - rule.id:(5551 OR 5712). Other related rules are 5710, 5711, 5716, 5720, 5503, 5504
