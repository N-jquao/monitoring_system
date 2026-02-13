## Solarwinds Observability Solution

<br>

1. First I setup Solarwinds
2. I then logged in at https://portal.solarwinds.cloud
3. I then installed Solarwinds agent on the Ubuntu server to be monitored

- I had to navigate to settings in online portal --> API --> Generate API
- Then I had to navigate to settings in online portal --> Agents --> Add agent

<br>

4. I then configured monitoring on the Ubuntu server

<br>

- Basic System Monitoring
  The agent automatically collects:
  
  CPU usage
  Memory utilization
  Disk usage and I/O
  Network traffic
  Process information
  System logs


```bash
# Install Docker if not already installed
sudo apt update

sudo apt install -y docker.io

sudo systemctl start docker

sudo systemctl enable docker

# Add your user to docker group
sudo usermod -aG docker $USER

newgrp docker

# Run SolarWinds collector as Docker container
docker run -d \
  --name solarwinds-collector \
  --restart unless-stopped \
  --hostname $(hostname) \
  -v /etc/solarwinds/otel-config.yaml:/etc/otelcol-contrib/config.yaml:ro \
  -v /sys:/hostfs/sys:ro \
  -v /proc:/hostfs/proc:ro \
  -v /etc:/hostfs/etc:ro \
  --pid host \
  --network host \
  otel/opentelemetry-collector-contrib:latest \
  --config=/etc/otelcol-contrib/config.yaml
```
