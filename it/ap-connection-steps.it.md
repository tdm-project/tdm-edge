# Connessione all'AP dell'Edge


0. [Introduzione](#introduzione)
1. [Prima configurazione](#prima-configurazione)
2. [Rete locale non trovata](#rete-locale-non-trovata)
3. [Access Point permanente](#access.Point-permanente)


## Introduzione


L'Edge Gateway dispone di una sua propria rete WiFi che gli permette di agire
un vero e proprio "*Access Point*" e quindi di essere raggiunto in assenza di
una rete WiFi locale. 
Il nome di questa rete WiFi è formato dal prefisso "**TDM_**" seguito da un
certo numero di caratteri esadecimali (in genere 8), e.g. "**TDM_XXXXXXXX**".

La modalità "Access Point" viene abilitata:
*  *temporaneamente*
    * quando non è configurata nessuna WiFi locale (ad esempio al primo avvio)
    * quando puer essendo configurata, l'Edge Gateway non riesce a collegarsi alla rete WiFi locale
* *permanentemente*
    * con un opportuno settaggio su file di configurazione.


## Prima configurazione

Appena installato, all'avvio l'Edge Gateway attiva l'Access Point e può essere
raggiunto collegandosi alla rete WiFi "**TDM_XXXXXXXX**" per permettere la
configurazione:
* in questo caso la passphrase è **tdmedgegateway**

:warning: Una volta configurata la rete WiFi domestica/locale l'Edge Gateway
non riavvierà la propria rete WiFi e il proprio Access Point rimarrrà spento in
tutti gli altri casi previsti a meno che non venga **modificata la passhphrase
della rete dell Edge Gateway stesso**.

Per questo motivo si consiglia di cambiarla al primo avvio.


## Rete locale non trovata


Una volta configurata la rete WiFi domestica/locale, se viene riavviato:
* l'Edge Gateway tenta per circa 3 minuti di collegarsi alla rete WiFi locale
* se questa non fosse presente (ad esempio l'Edge è stato spostato) o l'Edge
  non riuscisse a collegarsi (ad esempio è stata modificata la passphrase)
* **se la passphrase dell'Access Point dell'Edge Gateway è stata impostata
  (ossia non è più quella di default)**
* l'Edge Gateway avvia il proprio Access Point e può essere raggiunto
  collegandosi alla rete WiFi **TDM__XXXXXXXX** usando la *passphrase*
  impostata in precedenza.

:bulb: In questo caso l'Edge Gateway rimane in modalità "Access Point" per 10
minuti, dopodichè riproverà per altri 3 minuti circa a collegarsi alla rete
WiFi locale.


## Access Point permanente


L'attivazione dell'Access Point dell'Edge Gateway può essere resa permanente
configurandolo opportunamente. Per far ciò occorre:
* che la *passphrase* dell'Access Point dell'Edge sia stata modificata (ossia non è più quella di default);
* entrare nella console dell'Edge Gateway tramite `ssh`:
```bash
ssh alarm@<ip_o_hostname_dell_edge>
```
* modificare il file `/etc/default/tdm-services`:
  * aprire il file con un editor di testo:
    ```bash
    sudo nano /etc/default/tdm-services
    ```
  * modificare la riga:
    ```ini
    ALWAYS_RUN_AP=0
    ```

    in 

    ```ini
    ALWAYS_RUN_AP=1
    ```

  * salvare ed uscire con la combinazione di tasti:
    * **CTRL-O INVIO**
    * **CTRL-X**

* riavviare l'Edge Gateway
