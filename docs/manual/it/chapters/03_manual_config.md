## 3. CONFIGURAZIONE SOFTWARE TDM

Il file di configurazione unica per i microservizi TDM è:

* ‘***/opt/tdm-edge/configs/tdm/tdm.conf***’.

Tale file non esiste al momento dell’installazione e va creato al primo avvio con i comandi presenti nella prossima sezione.

Come esempio si riporta il suo contenuto completo:

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

Gli unici campi che possono necessitare di modifiche sono:

* Sezione ‘**EDGE_dispatcher**’:
  * *mqtt_remote_host*: indirizzo del broker TDM, potrebbe cambiare, previa indicazione sul sito TDM;
  * *mqtt_remote_port*: posta del broker TDM, potrebbe cambiare, previa indicazione sul sito TDM;

### 3.1 CREAZIONE DEL FILE DI CONFIGURAZIONE
Il file di configurazione non è presente all’installazione e va creato successivamente:

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

### 3.2 MODIFICA DEL FILE DI CONFIGURAZIONE
Il file di configurazione del software TDM può essere modificato successivamente alla creazione col comando ‘nano’:

```bash
nano /opt/tdm-edge/configs/tdm/tdm.conf
```

Una volta terminate le modifiche digitare:

* **CTRL+X**, successivamente **Y** e **INVIO** per salvare il file ed uscire
* **CTRL+X**, successivamente **N** e **INVIO** per uscire senza salvare il file.

