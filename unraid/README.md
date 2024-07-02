# Monitoring unRIAD

## Change to this repo:
golift/telegraf
## Extra Parameters:
/bin/bash -c "/entrypoint.sh telegraf"
## Extra Argument:
--user telegraf:$(stat -c '%g' /var/run/docker.sock)

For nivida devices add '--runtime=nvidia' added as an Extra Argument
Create a custom user script which should be executed during the startup with the following content:
''''#!/bin/bash
nvidia-persistenced''''
