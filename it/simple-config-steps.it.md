

# Configurazione semplificata del TDM Edge Gateway

Si da per scontato che il lettore sappia usare gli strumenti Linux/Unix di base.

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
  alla sezione "[Primo avvio e configurazione rete](#primo-avvio-e-configurazione-rete)"*

Per scrivere l'immagine del sistema operativo sulla scheda microSD suggeriamo
il software **Balena Etcher** disponibile per i sistemi Windows/Mac/Linux:
<http://www.balena.io/etcher/>.

* Scarica l'immagine del sistema operativo: <http://s.crs4.it/Gk/tdmimage-latest.img.xz>
* Installa il programma Balena Etcher sul PC
* Inserire la microSD nel portatile (eventualmente usando l'adattatore microSD/SD)
* Avvia Balena Etcher ed esegui i seguenti passi:
  * clicca su ’*Flash from file*’ e seleziona il file immagine
  * clicca su ’*Select Target*’ e seleziona il lettore di Schede SD (attenzione a selezionare il dispositivo corretto)
  * clicca su ’*Flash!*’ per avviare la scrittura e attendi il completamento


## Primo avvio


* Inserisci la scheda SD inizializzata nell'Edge Gateway ed accendi il
  dispositivo. Al primo avvio l'Edge Gateway attiverà una propria rete WiFi.
  Questa avrà un nome simile a `TDM_XXXXXXXX`. Cercala dal tuo PC e collegati.
  * La password per la rete è **tdmedgegateway**.

* Collegati all'Edge Gateway con un client *ssh*: su sistemi Mac OS e Linux
  usa da terminale il comando `ssh` mentre su sistemi Windows puoi usare il
  programma `PuTTY` <https://www.putty.org/>
  * indirizzo IP: **192.168.2.1**
  * nome utente: **alarm**
  * password:    **alarm**

```bash
    ssh alarm@192.168.2.1
```

* Al primo avvio occorre cambiare la password dell'Edge Gateway. Scegli una nuova password. Quando richiesto:
  * digita l'attuale password (*alarm*)
  * digita la nuova password
  * digita nuovamente la nuova password come conferma

:writing_hand: Annota la nuova password dell'Edge Gatway.

* Ricollegati all'Edge Gateway usando la nuova password:
  * indirizzo IP: **192.168.2.1**
  * nome utente: **alarm**
  * password:    ***\<nuova_password\>***

:bulb: Collegando l'Edge Gateway ad un monitor ed una tastiera puoi autenticarti
direttamente dal terminale saltando il collegamento tramite *ssh*.


## Configurazione


* Avvia il programma di configurazione dell'Edge Gateway `tdm-config` (inserisci
  la password dell'Edge quando richiesto):

```bash
    sudo tdm-config
```

* Prima di effettuare le configurazioni occorre espandere il file system perché
  questa operazione potrebbe cancellare i dati presenti nella microSD:

  * Entra nel menù "Espandi il Filesystem" e conferma l'operazione

* Modifica password AP
  * :warning: Se non modifichi la password, l'Edge non attiverà più l'access
    point Wifi -- neanche se non riesce a collegarsi alla Wifi domestica

  * Entra nel menù "*Modifica passphrase AP*" e conferma l'operazione
  * digita la nuova passphrase
  * digita nuovamente la nuova passphrase come conferma

:writing_hand: Annota la nuova passphrase per la wifi dell'Edge Gatway `TDM_XXXXXXXX`: questa ti servirà per raggiungere l'Edge qualora questo non riuscisse a collegarsi alla WiFi locale ([Collegarsi all'AP dell'Edge](it/ap-connection-steps.it.md))

* Entra nel menù "*Configurazione WiFi*"
  * entra nel sottomenù "*Configurazione WiFi (Scansione Automatica SSID)*" e attendi il termine della ricerca delle reti WiFi locali
  * seleziona la tua rete WiFi locale
  * inserisci lla passhprase della tua rete WiFi locale (non quella dell'Edge Gateway!)

:writing_hand: Annota l'indirizzo IP assegnato dal router

* Riavvia l'Edge Gateway con la voce nel menù `tdm-config`.


## Collegarsi all'Edge Gateway

* Dopo il riavvio, l'Edge Gateway userà la configurazione appena impostata per
  collegarsi alla WiFi domestica.

### Trovare l'Edge per nome

Se stai lavorando su Linux o MacOSX, puoi trovare l'Edge Gateway per nome
(attraverso usando mDNS/Zeroconf/Avahi).

* Recupera il hostname dell'Edge Gateway e usalo per collegarti con `ssh`. E.g.:

    ssh alarm@tdm-edge-xxxxxxxx.local

### Trovare l'Edge per indirizzo IP

Se stai lavorando su Windows, puoi trovare l'Edge Gateway usando l'indirizzo IP
fornito nel passo successivo.

* Recupera l'indirizzo dell'Edge Gateway e usalo per collegarti con `ssh`. E.g.:

    ssh alarm@192.168.xxx.yyy

:bulb: Anche su Windows puoi usare mDNS se installi il componente necessario.


## Configurazione

Procedi a configurare l'Edge Gateway.

Esegui `sudo tdm-config` e usa il menù per fare le seguenti cose.

* Imposta le "Configurazioni Generali"
* Se ti è stata fornita chiave MQTT:
  * Selezione la voce "Configurazioni MQTT", poi "Chiave Broker MQTT"
  * Imposta la tua chiave, poi verifica il collegamento

A questo punto la tua Edge Gateway è configurata.

## Collegarsi ai servizi

Per vedere gli indirizzi per accedere a Grafana e configurare i sensori, nel
menù `tdm-config` seleziona "Mostra Configurazione per i Sensori".


## Default accounts

:warning: È fortemente consigliato cambiare le password preimpostate.

### Edge Gateway Unix account

    username: alarm
    password: alarm

### Grafana account

    username: admin
    passoword: admin


