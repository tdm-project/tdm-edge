

# Configurazione semplificata del TDM edge gateway

Si da per scontato che il lettore sappia usare gli strumenti Linux/Unix di base.

## Procedura

1. [Installa il software sulla scheda SD](#installa-il-software-sulla-scheda-sd)
2. [Primo avvio e configurazione rete](#primo-avvio-e-configurazione-rete)
3. [Collegarsi all'edge gateway](#collegarsi-alledge-gateway)
4. [Configurazione](#configurazione)
5. [Collegarsi ai servizi](#collegarsi-ai-servizi)


## Installa il software sulla scheda SD


* Scarica l'immagine del sistema operativo: [http://s.crs4.it/Gk/tdmimage-latest.img.xz]
* Usa un software adeguato per scrivere l'immagine sulla scheda
  * Suggeriamo Balena Etcher: [http://www.balena.io/etcher/]
  * Segui le istruzioni fornite dal programma


## Primo avvio e configurazione rete


* Inserisci la scheda SD inizializzata nel Raspberry Pi ed accendi il dispositivo.
* La edge gateway attiverà una propria rete WiFi.  Avrà un nome tipo `TDM_12345678`.
  Cercala dal tuo PC e collegati.
  * La password per la rete è `tdmedgegateway`.
* Collegati all'edge gateway con un client `ssh`:
  * indirizzo IP: 192.168.2.1
  * nome utente: `alarm`
  * password:    `alarm`

    ssh alarm@192.168.2.1

* Configura l'accesso alla rete Wifi domestica con il comando `tdm-config`:

    sudo tdm-config
* Entra nel menù "Configurazione WiFi" e segui le istruzioni
* :warning: Annota l'indirizzo IP assegnato dal router
* :warning: Seleziona "Mostra Configurazioni Edge Gateway" e annota il hostname dell'edge
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

    ssh alarm@tdm-edge-12345678.local

### Trovare l'edge per indirizzo IP

Se stai lavorando su Windows, puoi trovare l'edge gateway usando l'indirizzo IP
fornito nel passo successivo.

* Recupera l'indirizzo dell'edge gateway e usalo per collegarti con `ssh`. E.g.:

    ssh alarm@192.168.1.34

:bulb: Anche su Windows puoi usare mDNS se installi il componente necessario.


## Configurazione

Procedi a configurare l'edge gateway.

Esegui `sudo tdm-config` e usa il menù per fare le seguenti cose.

* Modifica password AP
  * :warning: Se non modifichi la passoword, l'edge non attiverà più l'access
    point Wifi -- neanche se non riesce a collegarsi alla Wifi domestica
* Espandi il file system
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


