# Grafana
Docker compose featuring Grafana, Prometheus, and Influxdb2

Recommended Dashboards
Docker Monitoring (Flux) - 18389
Proxmox Monitoring (InfluxQL) - 10048

UNRAID - 

/bin/bash -c "/entrypoint.sh telegraf && apt update && apt install -y smartmontools && apt install -y lm-sensors && apt install -y nvme-cli"


--user telegraf:$(stat -c '%g' /var/run/docker.sock) 
