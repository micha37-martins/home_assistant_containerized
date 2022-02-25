This directory hosts zigbe2mqqt config as well as persistent data.

The config.yaml will be filled with the individualized values from the
docker-compose.yaml. These are injected as environment variables.

The config will be split to have a separate file for devices. Devices are
individual and may change from time to time.

The config can optionally be split for grops:

configuration.yaml:
```yaml
groups: groups.yaml
```

groups.yaml
```yaml
'1':
  friendly_name: group_1
  devices:
    - 0x00158d0001d82999
```
https://www.zigbee2mqtt.io/guide/configuration/devices-groups.html#groups
