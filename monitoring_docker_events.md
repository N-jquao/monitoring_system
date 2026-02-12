## Use Case - Monitoring Docker Events

- Docker automates the deployment of different applications inside software containers
- The Wazuh module for Docker identifies security incidents across containers and alerts in real-time
- I configured Wazuh to monitor Docker events on an Ubuntu endpoint hosting Docker containers

<br>

1. I installed Docker on the Ubuntu endpoint being monitored and configured Wazuh to monitor Docker events

```bash
sudo apt install python3 python3-pip

sudo apt install --upgrade pip
```

<br>

2. I installed Docker and Python Docker Library

```bash
sudo curl -sSL https://get.docker.com/ | sh
```

<br>

3. I edited the Wazuh agent configuration file /var/ossec/etc/ossec.conf and added this block to enable the docker-listener module

<br>

<img width="432" height="176" alt="image" src="https://github.com/user-attachments/assets/15d7eedc-ed4c-404c-a01e-ab580cc5d77d" />

<br>

```bash
sudo vi /var/ossec/etc/ossec.conf

sudo usermod -aG docker wazuh
```
<br>

4. I restarted the Wazuh agent

```bash
sudo systemctl restart wazuh-agent
```

<br>

5. I then tested the configuration

- I pulled an image (NGINX image) and run a container

<br>

```bash
sudo docker pull nginx

sudo docker run -d -P --name nginx_container nginx

sudo docker exec -it nginx_container cat /etc/passwd

sudo docker exec -it nginx_container /bin/bash
--> exit
```

<br>

- I stopped and removed the container

<br>

```bash
sudo docker stop nginx_container

sudo docker rm nginx_container

sudo docker images
```

<br>

6. I then visualized the alerts using Wazuh dashboard by going to docker module

<br>



