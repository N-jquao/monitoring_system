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
sudo hydra -l <username> -P <PASSWD_LIST.txt> <Ubuntu_IP> ssh

#if running hydra on RHEL use this command
sudo podman run --rm -it -v ~/hydra-wordlists:/wordlists:Z docker.io/kalilinux/kali-rolling   bash -c "apt update && apt install -y hydra && hydra -l admin -P /wordlists/passlist.txt 192.168.102.163 ssh"
```

<img width="837" height="98" alt="image" src="https://github.com/user-attachments/assets/85011983-be74-409f-8b3c-f93fb5140e12" />

<br>


3. To visualize the attack I went to the Threat Hunting module on Wazuh dashboard and added these filters to query the alerts

Linux - rule.id:(5551 OR 5712). Other related rules are 5710, 5711, 5716, 5720, 5503, 5504

<img width="953" height="455" alt="image" src="https://github.com/user-attachments/assets/2d66190e-d005-40a4-a160-f249225e7f89" />

