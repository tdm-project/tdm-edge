## 5. THE HARDWARE PLATFORM OF EDGE GATEWAY
### 5.1 EVALUATED HARDWARE SOLUTIONS
In order to prototype the Edge Gateway three embedded platforms have been evaluated: the BeagleBone Black and Raspberry PI (2-B and 3-B model).
All three platform satisfy low cost and availability requirements, being
easily purchased on e-commerce with costs between 30 and 80 euros. Moreover they have a large community of developers and users. Countless forums,
dedicated portals and blogs also provide documentation, assistance and examples
of applications that make it ideal platforms for educational purposes.

#### 5.1.1 BEAGLEBONE BLACK
The BeagleBone Black is a single-board computer (SBC) manufactured by Texas Instruments along with Digi-Key and Newark Element14. It is provided with a 32 bit ARM Cortex-A8 CPU bit with a maximum frequency of 1GHz and 512 MB DDR3 RAM. The single core processor has two programmable embedded Real-Time units. A large set of low level peripherals such as UART, CAN, I2C/SPI, ADC, USB are available.
The board storage resources are composed by 4GB onboard memory and a microSD player card.  
Network connectivity is provided by an integrated 100 Mbit/s ethernet network card. Wireless connectivity is missing but can be added through an external USB device. The estimated power consumption is 210-460 mA with a 5V power supply.

#### 5.1.2 RASPBERRY PI 2
The Raspberry Pi are a series of SBCs developed in the United Kingdom by the
Raspberry Pi Foundation. The main goal of the project was to provide a simple platform to teach computer science and programming basics in schools and deveoping countries. The Raspberry PI 2 features a Broadcom System-on-Chip (SoC) based on a 4 core 32-bit ARM Cortex-A7 processor with a maximum frequency of 900 MHz and 1 GB of DDR2 RAM. The storage consists of only a microSD card reader. No on-board storage device is present. The low level peripherals provided by the SoC are: GPIO, UART, I2C/SPI, USB and interfaces for Camera and Display. Unlike the BeagleBone Black, Analog/Digital converters (ADC) and the CAN Bus controller are missing. 

Network connectivity is provided by an integrated 100Mbit/s ethernet network card. No built-in wireless capabilty is available but it can be easily added via an external USB device. Estimated current consumption is about 800 mA with 5V power supply.

#### 5.1.3 RASPBERRY PI 3
The Raspberry Pi 3 is an evolution of the Raspberry PI 2. It features a SoC based on a quad-core 64 bit ARM Cortex- A53 processor and provides both a WiFi 802.11n chip and a BLE integrated Bluetooth. The
maximum CPU frequency increases up to 1.2GHz. The 1 GB DDR2 RAM and SoC low level peripherals do not change with respect to the Raspberry PI 2.
As the RPi 2, the RPi 3 storage resources consist of a microSD card reader without on-board storage devices. 
Network connectivity is provided by a 100 Mbit/s network integrated card (NIC). Estimated current consumption ranges from 300mA, 1.5W at rest,
up to 1.34 A, 6.7 W under load (5V supply voltage). Nevertheless, 
manufacturers suggest using power supplies capable of delivering
at least 2.5 A to avoid file system corruption and odd behaviour.

### 5.2 BENCHMARKS AND CHOICE OF HARDWARE PLATFORM
To make the Edge Gateway software working properly it was mandatory to evaluate the capabilities provided by the three platforms by means of different benchmarks.
A foundamental requirement was the compatibility of the SBCs with with the system of Docker container. To check both this feature and the computational capability the Linpack and Coremark benchmark have been used. Linpack measures the performance of floating-point operations while the Coremark tests algorithms used in embedded systems such as CRCs. As shown by the two following graphs, tests ran both in the native environment (Arch Linux) and within a Docker container.

![](../../img/linpack.png)  |
-------------------------|
![](../../img/coremark.png) |

Both benchmarks show that no overhead is present if the execution of the code if performed within a Docker container.
Moreover the Raspberry PI 3 appears to be the most powerful SBC among the three competitor. For this reason all the next benchmark results are reported only for this platform. 

The performance of
memory have been measured with the STREAM benchmark. STREAM belongs to the suite
HPCC and is used to evaluate the bandwidth of RAM memory
than different types of access (for example, Triad refers to a
access of the type: a[i] = b[i] + c[i]*scale and represents the most onerous case).
From the diagram, there results a substantial equivalence between the operations performed
in a native environment and inside a Docker container.

![](../../img/stream_rpi3.png) |
----------------------------|

# ---------ITA-------

Il test successivo è riferito al trasferimento di dati da e verso la memoria di
massa, in questo caso una SD card di classe 10. Il test è stato effettuato
utilizzando il benchmark sintetico FIO, un generatore di workload molto
flessibile che permette di simulare vari scenari nel trasferimento dei dati. I
grafici sono relativi ad operazioni di lettura e scrittura sia sequenziale che
casuale. L’asse orizzontale indica il blocksize usato per le operazioni, mentra
l’asse verticale l’ampiezza di banda espressa in kB/s. In questo test si ha
solo una leggera differenza nell’uso di Docker per letture casuali con
blocksize da 4KB fino a 64KB.

![](../../img/disk_io_rpi3.png) |
-----------------------------|

L’ultimo test si riferisce alle prestazioni di rete. Nei diagrammi di seguito è
visualizzata l’ampiezza di banda e la latenza del benchmark NetPIPE. NetPIPE
esegue le misure in modo indipendente dal protocollo di rete facendo una
semplice comunicazione andata-ritorno tra due processi con messaggi di
dimensioni crescenti. Dai risultati si ha la conferma dell’impatto
sostanzialmente nullo di Docker sulle prestazioni della piattaforma scelta.

![](../../img/netpipe_rpi3.png) |
-----------------------------|

In conclusione, delle tre schede prese in esame, quella dotata di maggior
capacità computazionale, anche alla luce dei test, è risultata la Raspberry Pi
3. Questa ha anche la connettività wireless integrata e non necessita quindi
dell’acquisto di un dongle USB aggiuntivo. Manca però di convertitori A/D e di
storage on-board, presenti nella BeagleBone Black. Le performance migliori
costano però in termini di alimentazione, richiedendo la RPi 3 almeno 2.5 A di
corrente.

L’utilizzo di container Docker non sembra aggiungere un sovraccarico
significativo al sistema operativo ospitante, soprattutto a fronte dei vantaggi
che offre. O, alternativamente, un processo in esecuzione all’interno di un
container Docker su sistemi embedded ARM, non percepisce la differenza.

### 5.3 SENSORISTICA
Per poter avviare lo sviluppo dell’Edge Gateway, così come per le altre
attività verticali del progetto, sono stati
identificati sensori e sistemi di misura iniziali da integrare con l’Edge Gateway.

#### 5.3.1 STAZIONE DI MISURA METEO/AMBIENTALE
Tra i vari progetti open source e open hardware, sono state individuate tre
differenti piattaforme per la sensoristica
dei parametri ambientali e l’integrazione con l’Edge Gateway:

* lo Smart Citizen Kit, SCK (<https://smartcitizen.me/>), device portatile per
  la misura di temperatura, umidità, luce, suono, CO, NO2;
* l’Arduino Weather Station Project, AWSP
  (<http://cactus.io/projects/weather/arduino-weather-station>), piattaforma di
misura fissa di tipo “tradizionale” con sensori per pressione atmosferica,
temperatura, umidità, velocità e direzione de vento e precipitazione;
* lo Stuttgart Fine Dust Sensor, SFDS
  (<https://luftdaten.info/en/construction-manual/>) per la misurazione di
umidità relativa, temperatura, e particolato PM10 e PM2.5.

Per lo sviluppo e il test di laboratorio dell’Edge Gateway si è scelto di usare
la SFDS in quanto la sua architettura e software ha permesso una rapida
integrazione con il prototipo dell’Edge Gateway. Inoltre, data la versatilità
della scheda, parte dei sensori della AWSP sono stati integrati nella SFDS, in
modo da avere una piattaforma unica, versatile ed espandibile.

La connettività della SFDS è rappresentata dal chip WiFi integrato, il quale
può agire sia come Access Point per la prima configurazione, che come WiFi
client per l’invio dei dati verso server di raccolta e visualizzazione remota.
La SFDS supporta diversi server remoti tra cui il database InfluxDB e sistemi
basati su API Rest. Non disponendo di uno storage locale, in assenza di
connettività i dati non possono essere salvati e ri-trasmessi. In rete locale
questo può essere superato dall’’integrazione con l’Edge Gateway al quale si
connette nativamente.

Grafici dei dati meteo come visualizzati da *Grafana* |
------------------------------------------------------|
![Grafici dei dati meteo come visualizzati da Grafana](../../img/sfds-grafana.png) |

#### 5.3.2 ENERGY MONITOR
Come strumento iniziale per il monitoraggio dei consumi energetici si è scelto
di utilizzare il sistema IotaWatt. Tale sistema è stato utilizzato anche nello
sviluppo e test dell’Edge Gateway.

IotaWatt è un sistema di monitoraggio elettrico accurato, multicanale,
open-hardware, open-source, a basso costo e facile da usare. Si basa su
piattaforma IoT dotata di connettività WiFi ed impiega per il campionamento di
tensione e corrente convertitori Analogici/Digitali a 12 bit capaci di
raggiungere elevate frequenze di campionamento. Il dispositivo dispone di un
Real Time Clock integrato e di una scheda SD per la memorizzazione delle
misure. Supporta la funzione di interrogazione dei dati con il server web
integrato e la trasmissione verso cloud.

Le caratteristiche principali:

* 14 canali di acquisizione;
* API REST per l'estrazione dei dati
* supporta sensori di corrente di marca, modello e capacità differenti
* supporta la definizione generica di qualsiasi sensore di corrente
* configurazione e visualizzazione basate su browser LAN locale
* Open Hardware/Software
* cloud supportati influxDB e Emoncms.org

Il software campiona i canali di ingresso alla velocità di 35-40 canali al
secondo, registrando la tensione (V), la potenza (Watt) e l'energia (kWh) su
scheda SD locale ogni cinque secondi. I dati possono essere visualizzati su
rete WiFi o inviati su server remoto Emoncms o database influxDB. In caso di
interruzioni del WiFi o del servizio Internet, IotaWatt continuerà a registrare
localmente, per poi aggiornare il server quando il sistema WiFi viene
ripristinato.

Il sistema IoTaWatt |
------------------------------------------------------|
![Il sistema IoTaWatt](../../img/iotawatt-system.png) |