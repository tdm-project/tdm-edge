

# Configurazione semplificata del TDM Edge Gateway

Si dà per scontato che il lettore sappia usare gli strumenti Linux/Unix di base.

## Procedura

0. [Prerequisiti](#prerequisiti)
1. [Installazione del software sulla scheda SD](#installazione-del-software-sulla-scheda-sd)
2. [Primo avvio](#primo-avvio)
3. [Collegarsi all'Edge Gateway](#collegarsi-alledge-gateway)
4. [Configurazione](#configurazione)
5. [Collegarsi ai servizi](#collegarsi-ai-servizi)


## Prerequisiti


* Per l'installazione e la configurazione del TDM Edge Gateway occorre:
  * Un Edge Gateway TDM
  * Un PC con connettività WiFi (ad esempio laptop Windows/Mac/Linux) e lettore di schede
    SD (è possibile usare *dongle WiFi USB* e *dongle lettore SD USB*)
  * Scheda microSD con adattatore microSD/SD
  * Rete WiFi con credenziali di accesso (SSID e chiave/password)


## Installazione del software sulla scheda SD


*Se si dispone di un Edge o una scheda microSD preinstallata si può passare
  alla sezione "[Primo avvio](#primo-avvio)"*

Per scrivere l'immagine del sistema operativo sulla scheda microSD suggeriamo
il software **Balena Etcher** disponibile per i sistemi Windows/Mac/Linux:
<http://www.balena.io/etcher/>.

1. Scarica l'immagine del sistema operativo: <http://s.crs4.it/HF>
2. Installa il programma Balena Etcher sul PC
3. Inserire la microSD nel portatile (eventualmente usando l'adattatore microSD/SD)
4. Avvia Balena Etcher ed esegui i seguenti passi:
    1. clicca su ’*Flash from file*’ e seleziona il file immagine
    2. clicca su ’*Select Target*’ e seleziona il lettore di Schede SD (attenzione a selezionare il dispositivo corretto)
    3. clicca su ’*Flash!*’ per avviare la scrittura e attendi il completamento


## Primo avvio

1. Inserisci la scheda SD inizializzata nell'Edge Gateway ed accendi il
  dispositivo. Al primo avvio l'Edge Gateway attiverà una propria rete WiFi.
  Questa avrà un nome simile a `TDM_XXXXXXXX`. Cercala dal tuo PC e collegati.
    * La password per la rete è **tdmedgegateway**.

2. Collegati all'Edge Gateway con un client *ssh*: su sistemi Mac OS e Linux
  usa da terminale il comando `ssh` mentre su sistemi Windows puoi usare il
  programma `PuTTY` <https://www.putty.org/> (per ulteriori informazioni su
  l'uso di PuTTY vedere [Connessione all'Edge Gateway](connect-to-edge.it.md):
    * indirizzo IP: **192.168.2.1**
    * nome utente: **alarm**
    * password:    **alarm**

    ```bash
        ssh alarm@192.168.2.1
    ```

3. Al primo avvio occorre cambiare la password dell'Edge Gateway. Scegli una nuova password. Quando richiesto:
    * digita l'attuale password (*alarm*)
    * digita la nuova password
    * digita nuovamente la nuova password come conferma

     :writing_hand: Annota la nuova password dell'Edge Gatway.

4. Ricollegati all'Edge Gateway usando la nuova password:
    * indirizzo IP: **192.168.2.1**
    * nome utente: **alarm**
    * password:    ***\<nuova_password\>***

    :bulb: Collegando l'Edge Gateway ad un monitor ed una tastiera puoi autenticarti
    direttamente dal terminale saltando il collegamento tramite *ssh*.


## Configurazione


1. Avvia il programma di configurazione dell'Edge Gateway `tdm-config` (inserisci
  la password dell'Edge quando richiesto):
    ```bash
        sudo tdm-config
    ```

2. Prima di effettuare le configurazioni occorre espandere il file system perché
  questa operazione potrebbe cancellare i dati presenti nella microSD:
    1. Premi "OK" quando all'avviso per il fatto di non aver ancora modificato
      la password dell'access point.
    2. Entra nel menù "Espandi il Filesystem" e conferma l'operazione

3. Modifica password AP

    :warning: Se non modifichi la password, l'Edge non attiverà più l'access
    point Wifi -- neanche se non riesce a collegarsi alla Wifi domestica
    
    1. Entra nel menù "*Modifica passphrase AP*" e conferma l'operazione
    2. digita la nuova passphrase
    3. digita nuovamente la nuova passphrase come conferma

     :writing_hand: Annota la nuova passphrase per la wifi dell'Edge Gatway
     `TDM_XXXXXXXX`: questa ti servirà per raggiungere l'Edge qualora questo non
     riuscisse a collegarsi alla WiFi locale (per maggiori dettagli [Connessione
     all'AP dell'Edge](ap-connection-steps.it.md))

4. Entra nel menù "*Configurazione WiFi*"
    1. entra nel sottomenù "*Configurazione WiFi (Scansione Automatica SSID)*" e attendi il termine della ricerca delle reti WiFi locali
    2. seleziona la tua rete WiFi locale
    3. inserisci la passhprase della tua rete WiFi locale (non quella dell'Edge Gateway!)

    :writing_hand: Annota l'indirizzo IP assegnato dal router

5. (OPZIONALE) Se vuoi aggiungere la posizione dell'Edge ai dati trasmessi al
  Cloud TDM Entra nel menù "*Configurazioni Generali*"
    * Entra nel sottomenù "*Coordinate GPS Locali*" e inserisci le coordinate GPS
      dell'Edge (segui le istruzioni della schermata). È relativamente semplice
      recuperare le proprie coordinate GPS da Google Maps, ma ricorda che Maps
      te le presenta invertite (latitudine longitudine invece di longitudine
      latitudine)

6. Per abilitare la trasmissione dei dati al Cloud TDM occorre configurare
  l'indirizzo del Broker MQTT. Entra nel menù "*Configurazioni MQTT*"
    * entra nel sottomenù "*Indirizzo Broker MQTT*" e inserisci l'indirizzo o il
      nome del broker MQTT *che ti è stato fornito*

7. Entra nel menu "*Mostra Configurazioni Edge Gateway*"

    :writing_hand: annota l'*Edge Gateway Hostname* che dovrebbe essere simile a
    *tdm-edge-xxxxxxxx*: ti servirà per collegarti all'Edge Gateway in futuro.
    
8. Riavvia l'Edge Gateway con la voce nel menù `tdm-config`.


## Collegarsi all'Edge Gateway

* Dopo il riavvio, l'Edge Gateway userà la configurazione appena impostata per
  collegarsi alla WiFi domestica.

* Per collegarsi all'Edge si possono usare:

  * l'indirizzo IP salvato durante la configurazione della rete WiFi domestica
    ```bash
    ssh alarm@www.xxx.yyy.zzz
    ```
  * il nome dell'host: **tdm-edge-xxxxxxxx**, es:
    ```bash
    ssh alarm@tdm-edge-xxxxxxxx
    ```
  * il nome dell'host: **tdm-edge-xxxxxxxx.local**, es:
    ```bash
    ssh alarm@tdm-edge-xxxxxxxx.local
    ```


## Collegarsi ai servizi

I parametri e gli indirizzi per accedere a Grafana e configurare i sensori
possono essere recuperati eseguendo `tdm-config` e selezionando il menù "Mostra
Configurazione per i Sensori".

Per dettagli sulla configurazioni dei specifici sensori:
* [Configurazione del sensore EmonTx](emontx-sensor-config.it.md)
* [Configurazione della dashboard](dashboard-emontx.it.md)
* [Configurazione del sensore indoor TDM](indoor-sensor-config.it.md)
* [Configurazione della stazione IoTaWatt](iotawatt-station-config.it.md)


## Collegamento a Grafana

* Per accedere alla Dashboard Web dell'Edge Gateway 'Grafana':
  * aprire il browser web ed inserire l'indirizzo o l'hostname dell'Edge Gateway preceduto da **http://**:
  * l'utente di default è '*admin*'
  * la password di default è '*admin*' e verrà chiesto di cambiarla al primo accesso.


### Default accounts

:warning: È fortemente consigliato cambiare le password preimpostate.

### Edge Gateway Unix account (al primo avvio)

    username: alarm
    password: alarm

### Grafana account

    username: admin
    passoword: admin


