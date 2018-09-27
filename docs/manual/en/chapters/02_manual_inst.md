## 2. INSTALLAZIONE

Per procedere all’installazione dell’Edge Gateway occorrono:

* un PC Linux con lettore per schede SD/microSD;
* privilegi di ‘*root*’;
* un adattatore SD/microSD;
* accesso ad una rete WiFi;

La versione corrente del'Edge Gateway consiste di:  * una Raspberry Pi 3;
* un alimentatore 220 V - 5 V microUSB in grado di erogare almeno 2,5 A;
* una microSD da almeno 8 GB di capacità, consigliati 16 GB, classe 10.

I passi dell’installazione seguenti permettono di:

1. installare il sistema operativo personalizzato per l’Edge Gateway;
2. effettuare il primo accesso e la configurazione della rete WiFi casalinga;
3. installare i microservizi;
4. configurare il software TDM e avviare i servizi.

Inoltre viene mostrato il collegamento di un sensore di test per temperatura e umidità HTU21D.

### 2.1 INSTALLAZIONE DEL SISTEMA OPERATIVO SULL’EDGE GATEWAY

***Nota: Per alimentare la Raspberry Pi 3 è raccomandato un alimentatore in grado di erogare almeno 2,5A: una corrente
insufficiente infatti può causare malfunzionamenti e corruzione del filesystem.***

***Nota: La seguente procedura di installazione è basata in parte sulla procedura di installazione (in lingua inglese) di
Arch Linux per architettura ARMv7. La procedura originale è disponibile all’indirizzo:
<https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-3>.***

Le seguenti istruzioni consistono in comandi da digitare in un terminale o una console sul PC Linux. Esse, se impartite
da utente diverso da ‘***root***’ richiedono i privilegi di amministratore. Alternativamente si può usare una shell ‘***sudo***’.
Per avviare una shell sudo da un terminale Linux ed eseguire le successive istruzioni, digitare:

```bash
sudo -s
```

ed inserire la propria password.

### **ATTENZIONE: le operazioni che seguono distruggeranno i filesystem presenti nella scheda microSD, cancellandone il contenuto. Prestare la massima attenzione ai nomi dei dispositivi verso i quali i comandi sono indirizzati.**

Di seguito sostituire ***sdX*** con il nome della periferica SD come mostrata sul computer. Per essere certi di agire sul
dispositivo corretto si può utilizzare il comando *fdisk -l* prima e dopo l’inserimento della scheda SD per identificare il
device id del dispositivo inserito.

1. inserire la microSD nel lettore (usando eventualmente l’adattatore);
2. prima di procedere al partizionamento è opportuno verificare che il sistema operativo non abbia montato
eventuali partizioni già presenti sulla SD. A tal fine si può procedere consultando l’output del comando
*mount*. Se nell’output è presente una o più partizioni con suffisso **sdX** (ad esempio ***sdX1***) smontarla mediante
il comando

  ```bash
umount /dev/sdX{numero_partizione}
  ```

3. Partizionare la microSD con ***fdisk***:

  ```bash
fdisk /dev/sdX
  ```

4. Al prompt di fdisk, eliminare le vecchie partizioni e crearne una nuova:
  
  **a.** Digitare **o + ENTER**. In questo modo si eliminano eventuali partizioni sull'unità.
  
  **b.** Digitare **p + ENTER** per elencare le partizioni. Non ci dovrebbero essere più partizioni.

  **c.** Digitare **n + ENTER**, poi **p** per primario, **1** per la prima partizione sull'unità, premere **ENTER** per
accettare il primo settore predefinito, quindi digitare **+100M** per l'ultimo settore. Nel caso dovesse
essere richiesto, premere **y** per rimuovere la firma *vfat* o *ext4* dalla prima partizione.

  **d.** Digitare **t + ENTER**, quindi c per impostare la prima partizione sul tipo W95 FAT32 (LBA).

  **e.** Digitare **n + ENTER**, poi **p** per primario, **2** per la seconda partizione sull'unità, quindi premere due
volte **ENTER** per accettare il primo e l'ultimo settore predefiniti.
Nel caso dovesse essere richiesto, premere **y** per rimuovere la firma ext4 dalla prima partizione.
  **f.** Digitare **w + ENTER** per scrivere la tabella delle partizioni e uscire da fdisk.

5. Creare e montare il filesystem FAT:

  ```bash
cd /tmp/
mkfs.vfat /dev/sdX1
mkdir boot
mount /dev/sdX1 boot
  ```

6. Creare e montare il filesystem ext4:

  ```bash
mkfs.ext4 /dev/sdX2
mkdir root
mount /dev/sdX2 root
  ```
  
6. Scaricare ed estrarre il filesystem di root (per preservare i permessi dei file, eseguire il comando come utente
*root* e non utilizzando il comando *sudo*):

   ```bash
wget --content-disposition \    
    https://space.crs4.it/s/ywvVDh3tuOBWCZ2/download
tar -xpf TDM-Arm-image-latest.tar.gz -C root
sync
   ```
Il comando ‘*tar*’ potrebbe mostrare messaggi di errore quali:

  * tar: Ignoring unknown extended header keyword 'SCHILY.fflags'
  * tar: Ignoring unknown extended header keyword 'LIBARCHIVE.xattr.security.capability'

  Su alcuni sistemi sono previsti e possono essere ignorati.

7. Spostare i file di avvio nella prima partizione:

  ```bash
mv root/boot/* boot
  ```

8. Smontare le due partizioni:

  ```bash
umount boot root
  ```

  Ora è possibile uscire dalla shell sudo, se in uso, premendo **CTRL+D** o digitando

  ```bash
exit
  ```

### 2.2 PRIMO ACCESSO ALL’EDGE GATEWAY E CONFIGURAZIONE WIFI
Appena avviato l’Edge Gateway si presenta come un Access Point con una sua rete WiFi privata. Attraverso questa è
possibile accedere al dispositivo.

1. Inserire la scheda microSD nella Raspberry Pi e applicare l'alimentazione 5V;
2. attendere alcuni secondi che si avvii il sistema operativo;
3. dal PC, cercare una rete WIFI che abbia un nome simile a ‘TDM_XXXXXXXX’ e collegarsi utilizzando la
password ‘***tdmedgegateway***’;

  ### **ATTENZIONE**: lasciare attiva la rete WiFi locale dell’Edge **senza cambiare la password di default potrebbe permettere l’accesso all’Edge a chiunque conosca tale password**. Si consiglia di modificarla al primo accesso o disabilitare la rete WiFi locale dell’Edge con i commandi mostrati nella sezione '***Cambio password utente e passphrase WiFi***'.

4. Utilizzando un terminale SSH collegarsi all'indirizzo IP ‘***192.168.2.1***’:

  * Effettuare il login come utente predefinito ‘*alarm*’ con password ‘*alarm*’:
  
  ```bash
ssh alarm@192.168.2.1  
  ```
  
  Appena entrati sull’Edge viene richiesto di cambiare obbligatoriamente la password di accesso:

  * digitare la password corrente ‘*alarm*’
  * digitare la nuova password, contenente almeno 8 caratteri
  * ri-digitare la nuova password per conferma

	Il collegamento SSH a questo punto viene terminato e si può procedere a ricollegarsi usando la nuova
password.

#### Configurazione WiFi domestica sull’Edge Gateway

**ATTENZIONE**: per sicurezza, l’utente ‘*root*’ è disabilitato, Le operazioni di amministrazione sono possibili solo attraverso l’uso del comando ‘***sudo***’.

5. Ottenere i privilegi di *amministratore* inserendo la password quando richiesto dopo aver digitato il comando:

  ```bash
sudo -s
  ```

6. Configurare l’Edge Gateway per collegarsi alla rete WIFI casalinga; sostituire <ESSID> con il nome della rete WIFI domestica e dopo aver digitato il seguente comando:

  ```bash
wpa_passphrase "<ESSID>" > /etc/wpa_supplicant/wpa_supplicant-wlan0.conf
  ```
  
7. il precedente comando non fornisce un *prompt*, cioè non stampa nessun messaggio di testo ma resta in
attesa di input da parte dell’utente; inserire la password della rete WiFi domestica e premere **ENTER**;

8. Riavviare l’interfaccia di rete wireless per rendere effettive le modifiche:

  ```bash
systemctl restart wpa_supplicant@wlan0
  ```

9. Se tutto è andato correttamente, l’indirizzo IP assegnato dal router wireless casalingo può essere ricavato col comando:

  ```bash
ifconfig wlan0
  ```
  
  Annotare l’IP in modo da poterlo usare per accedervi successivamente e per la configurazione delle stazioni di rilevamento.

10. Ora è possibile uscire dalla shell sudo, se in uso, premendo **CTRL+D** o digitando

  ```bash
exit
  ```
  
#### Cambio password utente e passphrase WiFi
Per modificare successivamente la password dell’utente ‘alarm’ digitare il comando:

  ```bash
passwd
  ```

e:

* digitare la password corrente
* digitare la nuova password, contenente almeno 8 caratteri
* ri-digitare la nuova password per conferma.

Per modificare la passphrase per la rete wireless creata dall’Edge Gateway, digitare:

  ```bash
sudo hostap_chpsk
  ```

* digitare la nuova passphrase, contenente minimo 8 caratteri e massimo 63
* ri-digitare la nuova passphrase per conferma.

Se non necessaria, è possibile disabilitare la rete wireless creata dall’Edge Gateway coi comandi:

  ```bash
sudo systemctl disable start_ap
sudo systemctl stop start_ap
  ```
  
### 2.3 INSTALLAZIONE SOFTWARE TDM
Per procedere alle operazioni che seguono occorre per prima cosa eseguire il login ssh all’indirizzo annotato in precedenza usando l’utente alarm. Ottenuto l’accesso passare all’utente root digitando il comando:

  ```bash
sudo -s
  ```

e inserendo la password dell’utente corrente.

#### 2.3.1 Versione di sviluppo

  ```bash
cd /opt
git clone https://github.com/tdm-project/tdm-edge.git
cd tdm-edge
git checkout develop
  ```
