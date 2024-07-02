# Monitoring Unraid with Telegraf

## Add your configuration
Before we add our Telegraf container we need to add our configuration. In this example we are placneting it the default dectory for Docker comfigurations on Unraid. Change anything here to better fit your setup. When working with my appdata directory in Unraid I generally use the [Dynamix File Manager](https://forums.unraid.net/topic/120982-dynamix-file-manager/) it's an awesome tool that makes navigating shares in the Unraid UI easy.

First download the [telegraf.conf](https://github.com/TechHutTV/server-monitoring/blob/main/unraid/telegraf.conf) file located in this repository and make any changes you'd like. Most everything in the configuration should work as-is Unraid, but you will need to edit the outputs plugin information to properly send data to your InfluxDB 2 bucket. _Note: Use a new bucket specifically for this Unraid instance._

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

### Start and Verify

Other than these aqjustments everything should be able to be left as is included in the template. Check over everything and click on apply. Go to your Docker page in Unraid and check to see if the container is running. Check the log files to ensure there are no issues and that all the plugins are properly actived.

If there are no issues we check check to see if the data is being properly exported to InfluxDB 2. Head over to your dashboard and open the bucket you created for Unraid. It should look like the picture below. Go though the data make sure sure nothing is missing. In my installation I have 7 tags for Docker data under the _messurements ID and another tag for every plugin I have enabled.

![Unraid data preview in InfluxDB 2](https://github.com/TechHutTV/server-monitoring/blob/main/unraid/unraid-data-preview.png?raw=true)

## Nivida Support

When browsing dashboards I [found one](https://grafana.com/grafana/dashboards/15503-system-information/) that included information on adding nvidia-smi, a tool that help monitor Vivida GPUs. I have not been able to test this, but feel free to. The plugin for this is commented out in the telegraf.conf.

```
[[inputs.nvidia_smi]]
  bin_path = "/usr/bin/nvidia-smi"
  timeout = "15s"
```
To get this working a extra argument is needed in the docker template for telegraf on Unraid.

Extra Argument: ```--runtime=nvidia```

Create a custom user script which should be executed during the startup with the following content:

```
#!/bin/bash
nvidia-persistenced
```

## To-Do
- [x] Create a working configuration
- [ ] Create a custom Unraid template for golift/telegraf
- [ ] Test inputs.apcupsd and add steps
- [ ] Test and verify inputs.nvidia_smi steps
- [ ] Add steps on enabling SSL for better security
- [ ] Add steps for connecting this data to Grafana
- [ ] Add tested and recommended Unraid dashboards

I'm more than open to any suggestions and improvements!
