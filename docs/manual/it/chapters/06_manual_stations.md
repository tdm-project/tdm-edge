## 6. CONFIGURAZIONE DELLE STAZIONI DI MISURA

Per lo sviluppo del prototipo si sono utilizzate due stazioni di misura remote,
collegate all’Edge Gateway attraverso rete WiFi dell’Edge stesso o rete WiFi
domestica disponibile sul sito di installazione. Di seguito verranno fornite le
specifiche per il collegamento, per ulteriori dettagli sulle stazioni e loro
installazione e configurazione si rimanda alla documentazione propria della
stazione.

### 6.1 STAZIONE METEO ‘STUTTGART FINE DUST SENSOR’

![Stazione IotaWatt con sensori collegati](../../img/outdoor-meteo-station.jpg) |
:---: |
**Stazione SFDS con sensore temperatura/umidità e particolato/polveri sottili** |

La stazione meteo SFDS si collega con l’Edge Gateway attraverso la rete WiFi
propria dell’Edge Gateway o quella domestica, usando il protocollo del database
InfluxDB. I parametri da impostare nella schermata di configurazione sono:

```
Server InfluxDB: <Indirizzo_IP_domestico_dell'Edge>
Porta InfluxDB: 8089
Username: utente impostato per il DB (attualmente 'root')
Password: password impostata per il DB (attualmente 'root')
```

L’indirizzo IP domestico dell’Edge è quello ricavato nella sezione
‘Configurazione WiFi domestica sull’Edge Gateway’. Se si vuole utilizzare la
rete WiFi usando l’Edge Gateway come Access Point:

```
Server InfluxDB: 192.168.2.1
```

Manuale di costruzione e configurazione della stazione di misura meteo (in
lingua inglese): <https://luftdaten.info/en/construction-manual/>.

### 6.2 STAZIONE DI MONITORAGGIO ENERGETICO ‘IOTAWATT’

### ATTENZIONE! L'elettricità è pericolosa.

###  Questo sezione descrive un dispositivo che utilizza sensori in prossimità di circuiti elettrici in tensione. L'installazione dei sensori all'interno dei quadri elettrici di distribuzione deve essere eseguita esclusivamente da un elettricista qualificato. 

### Gli altri lo fanno a proprio rischio e pericolo. 

###Le avvertenze contenute in questa guida non devono essere considerate un sostituto dell'esperienza e della formazione. Si prega di fare attenzione agli errori e di rivolgersi a un tecnico qualificato per tutte le procedure relative ai collegamenti in tensione di rete. Per la documentazione tecnica relativa alla stazione ‘IotaWatt’ si faccia riferimento alla documentazione ufficiale.

![Stazione SFDS con sensore temperatura/umidità e particolato/polveri sottili](../../img/iotawatt_probes.png) |
:---: |
**Stazione IotaWatt con sensori collegati** |

Di seguito sono riportate le configurazioni da inserire per il collegamento
all’Edge Gateway.

```
Server InfluxDB: <Indirizzo_IP_domestico_dell'Edge>
Porta InfluxDB: 8088
Username: utente impostato per il DB (attualmente 'root')
Password: password impostata per il DB (attualmente 'root')
```

L’indirizzo IP domestico dell’Edge è quello ricavato nella sezione
‘Configurazione WiFi domestica sull’Edge Gateway’. Se si vuole utilizzare la
rete WiFi usando l’Edge Gateway come Access Point:

```
Server InfluxDB: 192.168.2.1
```

Documentazione ufficiale della stazione Iotawatt con istruzioni per
l’installazione, configurazione e collegamento agli schemi e sorgenti(in lingua
inglese): <https://guide.openenergymonitor.org/setup/iotawatt/>.
