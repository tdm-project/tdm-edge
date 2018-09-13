## 1. INTRODUZIONE

Uno degli obiettivi principali del progetto TDM è di realizzare un’architettura
scalabile per l’acquisizione, l’integrazione e l’analisi di dati provenienti da
sorgenti eterogenee in grado di gestire i dati generati da un’area
metropolitana estesa. Una parte di questi dati proverrà da sensoristica diffusa
sul territorio. Al fine di gestire al meglio questa sensoristica, il progetto
prevede la realizzazione di un *reference design*, di uso generale, per una
piattaforma per la gestione periferica dei sensori e la trasmissione dei
segnali da essi raccolti. Questa piattaforma è denominata *Edge Gateway*.

Questo documento costituisce la descrizione dell’architettura hardware/software
dell’Edge Gateway e delle sue componenti preposte all’acquisizione e
pre-processamento delle misure acquisite dai sensori distribuiti del progetto,
e il loro inoltro verso il sistema centrale di raccolta, memorizzazione e
analisi. 

L’Edge Gateway qui descritto è di uso generale, ed è stato specificatamente
realizzato per essere utilizzato con altre infrastrutture basate su FIWARE
diverse da TDM. Può inoltre essere utilizzato su altri sistemi quali, ad
esempio, i cloud IoT Amazon AWS, IBM Watson, Microsoft Azure per costruire
soluzioni OASC-compliant.

In particolare, il design descritto si basa su piattaforme hardware facilmente
reperibili sul mercato e delle quali è disponibile liberamente tutta la
documentazione per il loro funzionamento e integrazione in altri prodotti (la
scheda BeagleBone Black, descritta dopo, ad esempio è distribuita come Open
Hardware, ossia il suo intero design può essere usato nell’architettura di
altri oggetti). Anche le stazioni di misura utilizzate assieme all’Edge Gateway
sono distribuite, oltre che come prodotto finito, anche come Open Hardware
mentre i sorgenti dei firmware relativi sono liberamente disponibili e
distribuiti sotto licenza Open Source. L’architettura software pensata per
l’Edge Gateway è a sua volta basata su Open Source, protocolli standard e il
codice d’esempio è liberamente utilizzabili e modificabile. In particolare, le
comunicazioni avvengono attraverso il protocollo standard MQTT, mentre il
formato dei messaggi è compatibile con gli standard FIWARE e OASC.

Tutto il software realizzato è reso disponibile su ***GITHUB*** all’indirizzo:
<https://github.com/tdm-project>.

Di seguito verrà presentata una breve panoramica dell’architettura generale del
progetto, l’architettura sviluppata per il software, i requisiti per la
piattaforma hardware i componenti scelti per il prototipo.

