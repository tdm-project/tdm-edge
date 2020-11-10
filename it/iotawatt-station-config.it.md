# Configurazione della stazione IoTaWatt

La stazione di rilevazione dei parametri di consumo elettrico o *IoTaWatt* una
estesa documentazioni raggiungibile alla pagina
"[IoTaWatt documentation](https://docs.iotawatt.com/en/master/). Si rimanda ad
essa per la dicumentazione e le istruzioni di configurazione.  Di seguito sono
forniti i dettagli per la condfigurazione dei parametri utili alla trasmissione
dei dati sull'Edge Gateway TDM.

1. [Parametri per la configurazione con l'Edge Gateway TDM](#parametri-per-la-configurazione-con-ledge-gateway-tdm)
2. [Convenzione per i nomi delle misure](#convenzione-per-i-nomi-delle-misure)

## Parametri per la configurazione con l'Edge Gateway TDM

Per configurare la IoTaWatt affinché trasmetta le misure rilevate dai sensori all'Edge Gateway:
* accedere tramite browser all'interfaccia di configurazione dell'IoTaWatt
  * o all'indirizzo **http://iotawatt.local** se sul PC è disponibile il servizio DNS-SD/mDNS/Bonjour
  * o all'indirizzo IP assegnato durante la configurazione iniziale (vedere [IoTaWatt - Device Configuration](https://docs.iotawatt.com/en/master/devConfig.html))
* portare il mouse sul bottone "*Setup*" e premere sul bottone "*Web Server*" quando compare

![Setup Menu](img/iotawatt_menu.png)

* cliccare su "*select server*" nel box "*Web Service*" e selezionare
  "*InfluxDB*" dal menu a tendina

![Web Service](img/iotawatt_webserver.png)

* nel form che appare:
  * nel campo "*server URL*" inserire **l'hostname** dell'Edge Gateway o il suo **indirizzo ip** preceduto da "**http://**" e seguito da "**:8088**"
    * es **http://tdm-edge-xxxxxxxx.local:8088**
  * nel campo "*database*" inserire il valore "**iotawatt**"
  * nel campo "*measurement*" inserire il valore "**iotawatt**"
  * nel campo "*field-key*" inserire il valore "**$name**" (non dimenticare il carattere dollaro '**$**')

* aggiungere un "*tag set*" con valori:
  * "*tag key*": "**node**"
  * "*tag value*": "**$device**" (non dimenticare il carattere dollaro '**$**')

* configurare le *misure* da inviare.

![InfluxDB](img/iotawatt_influxdb.png)

## Convenzione per i nomi delle misure
