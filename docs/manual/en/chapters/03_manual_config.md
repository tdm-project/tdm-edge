## 3. CONFIGURATION OF TDM SOFTWARE

The configuration file for setting the TDM microservices is:

* ‘***/opt/tdm-edge/configs/tdm/tdm.conf***’.

and has to be created after the installation (refer to the next section)

An example of its content is:

```ini
[EDGE_dispatcher]
mqtt_local_host = mosquitto
mqtt_local_port = 1883
mqtt_remote_host = broker.example.com
mqtt_remote_port = 1883
logging_level = 0
[FEINSTAUB_publisher]
mqtt_host = mosquitto
mqtt_port = 1883
logging_level = 0
influxdb_host = influxdb
influxdb_port = 8086
[IOTAWATT_publisher]
mqtt_host = mosquitto
mqtt_port = 1883
logging_level = 0
influxdb_host = influxdb
influxdb_port = 8086
[HTU21D_publisher]
mqtt_host = mosquitto
mqtt_port = 1883
interval=10
logging_level = 0
```

The fields that are likely to be changed are:

* ‘**EDGE_dispatcher**’ section:
  * *mqtt_remote_host*: address of the TDM broker, it could change, check on TDM website for information;
  * *mqtt_remote_port*:port of the TDM broker, it could change, check on TDM website for information;

### 3.1 CREATION OF THE CONFIGURATION FILE

The configuration file does not exist right after the installation and has to be created by typing:

```bash
cat > /opt/tdm-edge/configs/tdm/tdm.conf <<EOF
[EDGE_dispatcher]
mqtt_local_host = mosquitto
mqtt_local_port = 1883
mqtt_remote_host = broker.example.com
mqtt_remote_port = 1883
logging_level = 0
[FEINSTAUB_publisher]
mqtt_host = mosquitto
mqtt_port = 1883
logging_level = 0
influxdb_host = influxdb
influxdb_port = 8086
[IOTAWATT_publisher]
mqtt_host = mosquitto
mqtt_port = 1883
logging_level = 0
influxdb_host = influxdb
influxdb_port = 8086
[HTU21D_publisher]
mqtt_host = mosquitto
mqtt_port = 1883
interval=10
logging_level = 0
EOF
```

### 3.2 EDITING OF THE CONFIGURATION FILE
The configuration file of the TDM software can be modified in any moment after the creation using the ‘nano’ command

```bash
nano /opt/tdm-edge/configs/tdm/tdm.conf
```

After editing the file, type:

* **CTRL+X**, then **Y** and **ENTER** to save the file and exit.
* **CTRL+X**, then **N** and **ENTER** to exit without save.
