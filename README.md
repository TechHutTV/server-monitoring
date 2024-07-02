# Grafana
Docker compose featuring Grafana, Prometheus, and Influxdb2

Recommended Dashboards
Docker Monitoring (Flux) - 18389
Proxmox Monitoring (InfluxQL) - 10048

UNRAID - 

https://github.com/golift/telegraf-docker

/bin/bash -c "/entrypoint.sh telegraf"

--user telegraf:$(stat -c '%g' /var/run/docker.sock) 
