
# FAQ

## Non vedo la mia Wifi di casa

Verifica il canale che la tua Wifi sta utilizzando. Gli adattatori Wifi dei
dispositivi [Raspberry Pi
3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) utilizzati dai
dispositivi edge sono in genere:
* compatibili solo coi canali da 1 a 11;
* non compatibili coi più recenti standard Wifi a 5GHz (e.g., ieee 802.11ac).

Consultare il [datasheet](https://pdf1.alldatasheet.com/datasheet-pdf/view/1018493/CYPRESS/BCM43438.html) per le specifiche complete.

### Soluzione

Configurare la Wifi con impostazioni compatibili con l'edge gateway.  Proprio
per il motivo di facilitare la compatibilità, router/modem offrono anche la
possibilità di creare due reti Wifi diverse -- una coi più moderni standard come
802.11ac a 5GHz e una coi più compatibili standard 802.11n/b/a.  Se si opta per
quest'ultima soluzione si consiglia di usare nomi (SSID) diversi per le due reti
(e.g., "casa" e "casa-5g").

