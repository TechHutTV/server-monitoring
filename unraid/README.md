# Monitoring Unraid with Telegraf

## Add your configuration
Before we add our Telegraf container we need to add our configuration. In this example we are placneting it the default dectory for Docker comfigurations on Unraid. Change anything here to better fit your setup. When working with my appdata directory in Unraid I generally use the [Dynamix File Manager](https://forums.unraid.net/topic/120982-dynamix-file-manager/) it's an awesome tool that makes navigating shares in the Unraid UI easy.

First download the [telegraf.conf](https://github.com/TechHutTV/server-monitoring/blob/main/unraid/telegraf.conf) file located in this repository and make any changes you'd like. Most everything in the configuration should work as-is Unraid, but you will need to edit the outputs plugin information to properly send data to your InfluxDB 2 bucket.

```
[[outputs.influxdb_v2]]
  urls = ["http://ip:8086"]
  token = "yourtoken"
  organization = "yourorg"
  bucket = "unraidbucket"
```

In your appdata directory on Unraid make a new folder called telegraf. Within your telegraf folder upload the edited telegraf.conf file. The file path will look like this if your following the default Unriad setup. ```/mnt/user/appdata/telegraf/telegraf.conf ```


## Setting up the conatiner

Use the offical Telegraf container from the Applications page. We will ber using the golift/telegraf in replacment of the office image as it includes some packages needed for full monitoring of the hardware.
### Change the following

Repository: ```golift/telegraf```

Registry URL: ```https://hub.docker.com/golift/telegraf```

Provides a telegraf docker image with added tools for monitoring disks, sensors and IPMI. This exists because the base telegraf Docker image makes it difficult to monitor some system metrics. Applications added: smartctl (smartmontools), ipmitool, nvme-cli, sensors (lm-sensors), mtr (mtr-tiny), sudo. Sudoers entries are added for smartctl, ipmitool and nvme.

### Add the following

Extra Parameters: ```/bin/bash -c "/entrypoint.sh telegraf"```

Extra Argument: ```--user telegraf:$(stat -c '%g' /var/run/docker.sock)```
## Nivida Support
Extra Argument: ```--runtime=nvidia```

Create a custom user script which should be executed during the startup with the following content:

```
#!/bin/bash
nvidia-persistenced
```

Source: https://grafana.com/grafana/dashboards/15503-system-information/
