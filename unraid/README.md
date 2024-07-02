# Monitoring unRIAD

## Change to this repo:
golift/telegraf
### Extra Parameters:
/bin/bash -c "/entrypoint.sh telegraf"
### Extra Argument:
--user telegraf:$(stat -c '%g' /var/run/docker.sock)
### nivida:
Extra Argument: '--runtime=nvidia'

Create a custom user script which should be executed during the startup with the following content:

'''#!/bin/bash
nvidia-persistenced'''

Source: https://grafana.com/grafana/dashboards/15503-system-information/
