# Dashboard EmonTx

Accedendo all'indirizzo dell'edge nella LAN viene presentata una schermata di login per Grafana, il sistma utilizzato per la visualizzazione delle proprie misurazioni conservate nel database dell'Edge. Le credenziali standard sono \textbf{admin, admin}, ma se ne consiglia la modifica per evidenti esigenze di sicurezza. 

## Definizione delle sorgenti di dati

Effettuato l'accesso Grafana prevede come primo passo la definizione delle sorgenti dei dati da visualizzare. Queste sorgenti sono preconfigurate come mostrato in figura seguente.

![Sorgenti di dati in Grafana]{img/grafana-sources.png}

Si tratta delle sorgenti di dati:
- Meteo, associata al database \textit{luftdaten},
- IotaWatt associata al database: \textit{iotawatt}, 
- EmonTX associata al database \textit{Emon}, 
- EdgeGateway associata al database \textit{edgedevicehandler}.

Tra questa sorgenti IotaWatt è associata al sistema di misurazione avanzato, ed EmonTX è associato al sistema di misurazione base. Ci concentreremo su quest'ultimo.

## Definizione di una dashboard

Il secondo passo consiste nella definizione di una visualizzazione. Questa può essere definita totalmente dall'utente utilizzando componenti standard, ma per facilitare l'utilizzo da un utente non esperto sono state predisposte dei cruscotti standard liberamente distribuiti nella repository di Grafana.

Le visualizzazioni sono gestite attraverso il menu Dashboards-Manage come mostrato in figura seguente

![Gestione delle dashboards]{img/grafana-manage.png} 

Tramite il pulsante \textbf{Import} mostrato in figura è possibile una dashboard preconfigurata dall'archivio pubblico di Grafana \footnote{\url{https://grafana.com/grafana/dashboards}}. Ad ogni dashboard è associato un numero identificativo che basterà inserire nel campo apposito, ed in seguito premere il pulsante \textbf{Load}. Nel caso mostrato nella figura seguente viene importata la dashboard \textbf{11717} che costituisce il riferimento per il sistema di monitoraggio base, e che verrà presentata in maggiore dettaglio nella prossima sezione. 

![Importazione dashboard]{img/grafana-import.png}

Per completare l'importazione della dashboard occorre associare la sorgente di dati corretta tra quelle disponibili. La data source corretta è quella denominata EmonTX che deve essere selezionata come mostrato nella figura seguente.

![Definizione delle sorgenti di dati]{img/grafana-select.png}

La dashboard è a questo punto totalmente configurata, e mostra i dati conservati nel database locale. Il risultato è mostrato nella figura seguente.

![Dashboard configurata]{img/grafana-emontx.png} 

## Utilizzo della dashboard

