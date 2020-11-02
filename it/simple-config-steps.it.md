

# Configurazione semplificata del TDM edge gateway

Si da per scontato che il lettore sappia usare gli strumenti Linux/Unix di base.

## Procedura

0. [Prerequisiti](#prerequisiti)
1. [Installazione del software sulla scheda SD](#installa-il-software-sulla-scheda-sd)
2. [Primo avvio e configurazione rete](#primo-avvio-e-configurazione-rete)
3. [Collegarsi all'edge gateway](#collegarsi-alledge-gateway)
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

* Scaricare l'immagine del sistema operativo: <http://s.crs4.it/Gk/tdmimage-latest.img.xz>
* Scrivere l'immagine sulla scheda microSD
  * Per sistemi Windows/Mac/Linux suggeriamo il software **Balena Etcher**: <http://www.balena.io/etcher/>
    * Installare il programma sul PC
    * Inserire la microSD nel portatile (eventualmente usando l'adattatore microSD/SD)
    * Segui le istruzioni fornite dal programma


## Primo avvio e configurazione rete


* Inserisci la scheda SD inizializzata nel Raspberry Pi ed accendi il dispositivo.
* Al primo avvio l'Edge Gateway attiverà una propria rete WiFi.  Questa avrà un nome simile a `TDM_XXXXXXXX`.
  Cercala dal tuo PC e collegati.
  * La password per la rete è `tdmedgegateway`.
* Collegati all'edge gateway con un client `ssh`:
  * su sistemi Mac OS e Linux usando da terminale il comando `ssh`
  * su sistemi Windows usando il programma `PuTTY` <https://www.putty.org/>
  * indirizzo IP: 192.168.2.1
  * nome utente: `alarm`
  * password:    `alarm`

    ssh alarm@192.168.2.1

* Al primo avvio occorre cambiare la password dell'Edge Gateway. Scegli una nuova password.
* Quando richiesto:
  * digita l'attuale password (`alarm`)
  * digita la nuova password
  * digita nuovamente la nuova password come conferma
* Ricollegati all'Edge Gateway usando la nuova password:
  * indirizzo IP: 192.168.2.1
  * nome utente: `alarm`
  * password:    *\<nuova_password\>*


* Avvia il programma di configurazione dell'Edge Gateway `tdm-config` (richiede
  la password dell'Edge):

    sudo tdm-config

* Prima di effettuare le configurazioni occorre espandere il file system perché
  questa oprazione potrebbe cancellare i dati presenti nella microSD:

  * Entra nel menù "Espandi il Filesystem" e segui le istruzioni

* Modifica password AP
  * :warning: Se non modifichi la password, l'edge non attiverà più l'access
    point Wifi -- neanche se non riesce a collegarsi alla Wifi domestica

  * :writing_hand: Annota la nuova password per la wifi dell'Edge Gatway `TDM_XXXXXXXX`

* Entra nel menù "Configurazione WiFi" e segui le istruzioni

* :writing_hand: Annota l'indirizzo IP assegnato dal router

* :writing_hand: Seleziona "Mostra Configurazioni Edge Gateway" e annota il hostname dell'edge

* Riavvia l'edge gateway con la voce nel menù `tdm-config`.


:bulb: Se hai collegato l'edge ad un monitor e tastiera, salta i primi passi per
collegarsi via `ssh` e invece autenticati dal terminale, poi per eseguire `sudo
tdm-config`.

## Collegarsi all'edge gateway

* Dopo il riavvio, l'edge gateway userà la configurazione appena impostata per
  collegarsi alla WiFi domestica.

### Trovare l'edge per nome

Se stai lavorando su Linux o MacOSX, puoi trovare l'edge gateway per nome
(attraverso usando mDNS/Zeroconf/Avahi).

* Recupera il hostname dell'edge gateway e usalo per collegarti con `ssh`. E.g.:

    ssh alarm@tdm-edge-xxxxxxxx.local

### Trovare l'edge per indirizzo IP

Se stai lavorando su Windows, puoi trovare l'edge gateway usando l'indirizzo IP
fornito nel passo successivo.

* Recupera l'indirizzo dell'edge gateway e usalo per collegarti con `ssh`. E.g.:

    ssh alarm@192.168.xxx.yyy

:bulb: Anche su Windows puoi usare mDNS se installi il componente necessario.


## Configurazione

Procedi a configurare l'edge gateway.

Esegui `sudo tdm-config` e usa il menù per fare le seguenti cose.

* Imposta le "Configurazioni Generali"
* Se ti è stata fornita chiave MQTT:
  * Selezione la voce "Configurazioni MQTT", poi "Chiave Broker MQTT"
  * Imposta la tua chiave, poi verifica il collegamento

A questo punto la tua edge gateway è configurata.

## Collegarsi ai servizi

Per vedere gli indirizzi per accedere a Grafana e configurare i sensori, nel
menù `tdm-config` seleziona "Mostra Configurazione per i Sensori".


## Default accounts

:warning: È fortemente consigliato cambiare le password preimpostate.

### Edge gateway Unix account

    username: alarm
    password: alarm

### Grafana account

    username: admin
    passoword: admin


