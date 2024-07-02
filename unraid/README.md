# Monitoring Unraid with Telegraf

## Add your configuration
wip


## Setting up the conatiner

Use the offical Telegraf container from the Applications page. We will ber using the golift/telegraf in replacment of the office image as it includes some packages needed for full monitoring of the hardware.
### Change the following

Repository: golift/telegraf

Registry URL: https://hub.docker.com/golift/telegraf

Provides a telegraf docker image with added tools for monitoring disks, sensors and IPMI. This exists because the base telegraf Docker image makes it difficult to monitor some system metrics. Applications added: smartctl (smartmontools), ipmitool, nvme-cli, sensors (lm-sensors), mtr (mtr-tiny), sudo. Sudoers entries are added for smartctl, ipmitool and nvme.

### Add the following

Extra Parameters: /bin/bash -c "/entrypoint.sh telegraf"

Extra Argument: --user telegraf:$(stat -c '%g' /var/run/docker.sock)
## Nivida Support
Extra Argument: '--runtime=nvidia'

Create a custom user script which should be executed during the startup with the following content:

```
#!/bin/bash
nvidia-persistenced
```

Source: https://grafana.com/grafana/dashboards/15503-system-information/
