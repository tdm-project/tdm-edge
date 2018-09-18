## 3. CARATTERISTICHE GENERALI DELLLA PIATTAFORMA HARDWARE DELL’EDGE GATEWAY
### 3.1 CONSIDERAZIONI GENERALI

La piattaforma hardware per l’Edge Gateway, l’Edge Device, deve essere in grado
di poter far eseguire correttamente le funzionalità di aggregazione,
processamento e trasmissione sopra descritta, e permettere l’eventuale
integrazione di ulteriori componenti future.

Oltre a questi che sono requisiti tecnici, la scelta della piattaforma hardware
deve tenere conto anche di altri requisiti legati all’utenza prevista. Il
target di utenza per i sensori, oltre a quella istituzionale, è, infatti,
sempre più costituito da studenti di scuola superiore e privati cittadini che
volontariamente intendono acquistare o assemblare i dispositivi da sé.
Specifiche sperimentazioni in questo senso saranno fatte nel progetto TDM, ma
questa modalità di utilizzo è sempre più diffusa a livello mondiale.

E’ necessario, quindi, che la piattaforma hardware abbia un costo ridotto e sia
facilmente reperibile, possibilmente con una ampia comunità di sviluppatori e
utenti possibile. Deve inoltre essere libera da vincoli commerciali su
royalties e licenze, e possibilmente avere una connotazione *‘didattica’*.

In particolare due di questi requisiti generali, il basso costo e l’ampia
comunità di utenti restringono il campo delle scelte a:

* Single Board Computer (SBC) hobbistiche per la piattaforma, e
* distribuzione Linux come sistema operativo.

### 3.2 REQUISITI I/O E CONNETTIVITÀ
All’Edge Gateway è assegnato come ruolo principale quello di raccogliere e
aggregare misure e i dati acquisiti localmente dai diversi sensori e stazioni
di rilevamento sul campo e predisporli per l’invio verso il cloud per la
successiva archiviazione ed elaborazione.

Dal punto di vista delle capacità di I/O, la piattaforma hardware deve essere
quindi in grado di comunicare con i sensori utilizzando sia semplici bus
elettrici, sia protocolli di rete più sofisticati. In particolare, per i
sensori direttamente collegati al dispositivo Edge, è possibile usare
principalmente i protocolli I2C, USART e custom implementati su GPIO, mentre
per le unità di rilevazione remote quali i misuratori di consumo energetico e
le stazioni meteo/qualità dell’aria, la modalità più semplice ed efficace è la
connettività WiFi attraverso rete domestica, qualora presente nel luogo di
installazione, o eventualmente creandola attraverso l’Edge Gateway stesso.
Aspetto da tenere in considerazione inoltre è quello della possibilità di
future espansioni: requisito desiderato e non vincolante per la piattaforma
hardware è quindi la presenza di ulteriori bus elettrici e protocolli di
comunicazione tra dispositivi embedded.

Per quanto riguarda le comunicazioni dall’Edge Gateway verso un Cloud, quale
quello del progetto TDM, si è scelto di usare connettività Internet nelle
installazioni da questa raggiunta mentre per installazioni in aree rurali o non
ancore servite è possibile prevedere l’uso di connettività LTE attraverso chip
specifici o modem/dongle USB.

Riassumendo, la piattaforma hardware per l’Edge Gateway deve avere le seguenti
periferiche:

* bus/porta **I2C**;
* bus/porta **UART/USART**;
* esporre pin **GPIO**.

La presenza dei seguenti protocolli, sebbene non vincolante, è un desiderata:

* bus/porta **CAN**;
* porte/periferiche **ADC**;
* porte con uscita **PWM**;
* uscite **Video** e/o controller **LCD**;

***Deve*** avere le periferiche di rete:

* ***WiFi***;
* ***Ethernet***;
* ***USB*** Host.

### 3.3.REQUISITI COMPUTAZIONALI
Per quanto riguarda i requisiti computazionali, ossia quei requisiti che
permettono all’architettura software dell’Edge Gateway di poter essere eseguita
efficacemente, questi sono di tre tipi: il processore e la potenza di calcolo,
la quantità di memoria RAM, e la capacità di memorizzazione.


I requisiti per il processore dell’Edge Gateway sono i seguenti:
* deve essere supportato dal kernel Linux ed essere in grado di eseguire un
  sistema operativo Linux completo;
* avere un basso consumo energetico per permettere un utilizzo tramite
  accumulatore/batteria a bassa tensione;
* avere un buon supporto software e possibilmente un’ampia comunità di utenti e
  sviluppatori;
* avere SDK e compilatori disponibili gratuitamente e royalty free;
* avere un basso costo e schede di sviluppo complete accessibili;
* data la scelta architetturale di usare più microservizi in esecuzione
  concorrente, il processore deve possedere più di un core.

Molti processori per applicazioni embedded sono in grado di eseguire un sistema
operativo Linux completo. Tra questi, la famiglia degli ARM-CORTEX offre
diverse scelte, sia come processori che come schede di sviluppo, che soddisfano
questi requisiti.

Per quanto riguarda la memoria RAM, valgono diversi tra i punti precedenti. La
dotazione di RAM per l’Edge Gateway deve:

* essere sufficiente per eseguire un sistema operativo Linux completo;
* avere un ridotto consumo energetico;
* avere una dimensione sufficiente per servire più microservizi concorrenti in
  esecuzione su container Docker;
* supportare l’accesso da più core.

Per storage su Edge Gateway si intende il supporto di memorizzazione sia per il
sistema operativo e gli applicativi Edge, sia per la memorizzazione dei dati
raccolti nel tempo dai vari sensori e stazioni di misura collegati. Più in
dettaglio, i requisiti per lo storage sono:

* economico, accessibile e flessibile, ossia soluzioni reperibili facilmente
  nel mercato, in diverse capacità e facilmente rimpiazzabili e aggiornabili;
* capacità sufficiente per contenere il sistema operativo, gli applicativi Edge
  e uno storico ragionevolmente ampio di dati raccolti;
* ampia velocità di trasferimento dei dati.

La soluzione più diffusa che risponde a queste caratteristiche è quella della
tecnologia SD, e, pertanto si considera che la soluzione hardware per l’Edge
Gateway dovrà possedere il supporto per schede SD/microSD. Per via empirica si
è stabilito che una dimensione di 8GB, classe 10, sia sufficiente per gli scopi
dell’Edge Gateway, tuttavia, data la disponibilità di SD di capacità superiori
con costi ridotti, si consiglia la dimensione minima da 16GB, classe 10.

### 3.4 DISCUSSIONE
I requisiti sopra indicati possono essere posseduti da molte soluzioni
hardware, con vari rapporti costo-prestazioni.  Alla seguente sezione,
descriviamo l’architettura software realizzata, che si appoggia sui requisiti
sopra esposti e non ha dipendenze sulla soluzione specifica scelta. Alla
sezione 5 discuteremo, invece, l’implementazione hardware del prototipo di
riferimento.
