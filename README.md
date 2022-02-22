<img src="https://raw.githubusercontent.com/Koenkk/zigbee2mqtt/master/images/logo_vector.svg"
       alt="zigbee2mqtt"
       width="50" />
&nbsp;&nbsp;
<img src="https://raw.githubusercontent.com/eclipse/mosquitto/master/logo/mosquitto-text-side.svg"
     alt="mosquitto logo"
     width="200" />

# Home Assistant Docker
Home Asisstant containerized with docker-compose


## Goal of this project
This project aims for easy setup and maintainability.

As Home Asisstant (HA) offers a lot of possibilities out of the box this project
focuses on adding functionality that expands connectivity or adds value by
useful functionality. MQTT and ZigBee allows to connect to IKEA, OSRAM and more
devices. But having a tool like VS Code inside of HA does not add value to the
setup. It will clutter the UI instead and similar add-ons will be avoided.

## Setup
### Secrets
Personal data and secrets for docker-compose are stored in a .env file.
Secrets for mosquitto will be generated by the user and then stored in the 
mosquitto/ folder.

### User and Group (for mosquitto)
Mosquitto will use UID 1883 as default. You can add a corresponding user to your system and add your user to the group.
```bash
 $ sudo groupadd --gid 1883 mosquitto
 $ sudo useradd --no-create-home -g mosquitto --uid 1883 mosquitto
 $ sudo usermod -aG mosquitto USERNAME
```

##
<img src="https://raw.githubusercontent.com/mqtt/mqttorg-graphics/master/svg/mqtt-hor.svg"
     alt="MQTT logo"
     width="150"/>

### mosquitto (MQTT broker)
- The mosquitto setup is inspired by [vvatelot mosquitto-docker-compose](https://github.com/vvatelot/mosquitto-docker-compose)
- mosquitto is used as MQTT broker.
- Config, logs and persistent data is stored in a separate mosquitto/ folder.
- This setup uses usename and password for authentication.
>Optionally you could add more security by adding TLS: [Mosquitto TLS](https://mosquitto.org/man/mosquitto-tls-7.html)
- To set this up run:
```bash
  $ docker-compose up -d mosquitto
```
- And substitute "\<USERNAME\>" "\<PASSWORD\>" to generate an according entry in 
  mosquitto/config/password.txt
```bash
  $ docker-compose exec mosquitto mosquitto_passwd \
    -b /mosquitto/config/password.txt <USERNAME> <PASSWORD>
  $ docker-compose restart
```
- The same command can be used to update a password.

> **NOTE**: \
> "mosquitto_passwd -b" option should be used with care because the
> password will be visible on the command line and in command history.
> Find more details here: [mosquitto_passwd manpage](https://mosquitto.org/man/mosquitto_passwd-1.html)

- Add images to README.md on GitHub - Stack Overflow
- To test the command install a mosquitto client:
```bash
  Arch
  $ sudo pacman -S mosquitto  
  UBUNTU
  $ sudo apt-get install mosquitto-clients
```
- And run the following command in one terminal (nothing will happen
  as it only starts listening):
```bash
  $ mosquitto_sub -h localhost -t "hello/world"
```
- Then run in another terminal:
```bash
  $ mosquitto_pub -h localhost -u <USERNAME> -P <PASSWORD> -t hello/world -m "Hi"
```
_This example uses mosquitto but any other MQTT client should work._

- mosquitto.conf
```
password_file /mosquitto/config/password.txt
allow_anonymous false
user mosquitto
listener 1883
persistence true
persistence_location /mosquitto/data/
log_dest file /mosquitto/log/mosquitto.log
log_dest stdout
```

### zigbee2mqtt
https://www.zigbee2mqtt.io

Allows you to use your Zigbee devices without the vendors bridge/gateway. It act as a "driver" for a Zigbee Adapter. [List of supported devices](https://www.zigbee2mqtt.io/guide/adapters/)

#### Prerequisites
- At first you need to have a supported Zigbee Adapter:
  - https://www.zigbee2mqtt.io/guide/supported-hardware.html
- Then you find the location of the device:
  - https://www.zigbee2mqtt.io/guide/installation/01_linux.html#determine-location-of-the-adapter-and-checking-user-permissions

#### Setup
- We will not use the frontend provided by zigbee2mqtt so no port is exposed in 
  the docker-compose.yaml.
- The setting for zigbee2mqtt can be set in the docker-compose.yaml as env vars
- TODO: evaluate if empty config have to be created

## TODO
- zigbee2mqtt frontend
frontend:
  port: 8080
  host: 10.0.24.9
You should also add the following to the end so a network key is generated:

advanced:
  network_key: GENERATE
