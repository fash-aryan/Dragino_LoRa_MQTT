# MessagePack Encoded LoRa Packets

<img src="mini-project/figures/mini-project.jpg">

LoRa project features:

* Send and Receive MessagePack or ASCII encoded JSON measurement data.
* Dockerised InfuxDB, Mosquitto MQTT Broker, Node-RED, and Grafana.
* InfluxDB as the time series database.
* Grafana as the visualization platform.
* Mosquitto MQTT Broker to subscribe and publish LoRa Node measurement and actuation data.
* Node-RED as the flow programming language for creating visual flows.

## Table of Contents

1. [About the Project](#about-the-project)
1. [Project Status](#project-status)
1. [Getting Started](#getting-started)
    1. [Network and Communication Topology](#network-and-communication-topology)
    1. [Docker Images and Containers](#docker-images-and-containers)
    1. [Node-RED Flows](#node-red-flows)
    1. [InfluxDB Database](#influxdb-database)
    1. [Grafana Visualization](#grafana-virtualization)

# About the Project

In this project, the DHT11 temperature and humidity sensor data is collected and sent in JSON format to LG01 LoRa gateway. The JSON Format can be encoded as ASCII or MessagePack.

* The [ArduinoJson](https://arduinojson.org/) library is used for de/serializing JSON.
* ArduinoJson library includes a MessagePack de/serializer.
* Visit [msgpack.org](https://msgpack.org) to learn more.

```
serializeJson(doc, buf_send, RH_RF95_MAX_MESSAGE_LEN);          //ASCII Encoded JSON buf_send
serializeMsgPack(doc, buf_send, RH_RF95_MAX_MESSAGE_LEN);    //MessagePack Encoded JSON buf_send
```

# Project Status

- The provided LoRa-MQTT implementation should not be used in production and can only be used for proof-of-concept validation.
- LoRa network redundant packet filteration is not implemented when multiple LoRa gateways is used.

# Getting Started

### Prerequisites
Before starting this project:
- [Docker](https://www.docker.com/) has to be up and running on your device.
- Installing [Dragino](http://www.dragino.com/downloads/downloads/UserManual/LG01_LoRa_Gateway_User_Manual.pdf) libraries in Arduino IDE.

## Network and Communication Topology
![alt text](https://github.com/fash-aryan/EEET2371-WSNs/blob/master/mini-project/figures/topology.jpg?raw=true)

## Docker Images and Containers

Run the following commands in your terminal to download the required images and then build the containers.

### Eclipse Mosquitto MQTT Broker


```
docker pull eclipse-mosquitto
docker run -it -p 1883:1883 --name <influxdb container name> eclipse-mosquitto
```

### Node-RED
```
docker pull nodered/node-red
docker run -it -p 1880:1880 --name <nodered container name> nodered/node-red
```

### InfluxDB
The '-v' option stores InfluxDB data into a docker volume in $PWD directory.
```
docker pull influxdb
docker run -p 8086:8086 --name <influxdb container name> -v $PWD:/var/lib/influxdb influxdb

```
$PWD is replaced by the current directory that your terminal run in. You can change this directory by replacing it from $PWD to your preferred directory.

### Grafana
```
docker pull grafana/grafana
docker run -d -p 3000:3000 --name=mygrafana grafana/grafana
```

## Node-RED Flows

<img src="mini-project/figures/node-red.png">

## InfluxDB Database
| time          | id               |humidity            | temperature               |
|---------------|----------------- |--------------------|---------------------------|
| 1569988251974 |100               |26.00               |27.00                      |
| 1570594982609 |200               |24.00               |30.00                      |

## Grafana Visualization

<img src="mini-project/figures/grafana.png">

**[Back to top](#table-of-contents)**
