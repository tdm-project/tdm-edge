## 1. INTRODUZIONE

Lo scopo di questo documento è di descrivere i passi necessari per
l’installazione e la configurazione di un Edge Gateway TDM, al fine di essere
utilizzato assieme alle stazioni di misura previste nel documento di Design.
L’Edge Gateway consiste in una scheda Single Board Computer Raspberry Pi 3 con
relativa alimentazione e scheda di memoria microSD ed eventuali sensori
fisicamente collegati. Nel prototipo di laboratorio si è usato il sensore I2C
di temperatura e umidità HTU21D, ma è possibile aggiungere altri dispositivi,
dato che le specifiche della scheda e i sorgenti di esempio sono pubblici. La
comunicazione con il resto delle stazioni di misura e il cloud avviene per
mezzo della rete WiFi domestica.  

![](../../img/EG-2018-05.png) |
:------:|
| **Schema a blocchi dell'Edge Gateway** |


![](../../img/DSC_6182.jpg) | ![](../../img/DSC_6172.jpg) |
------|-------|

Scheda Raspberry Pi 3 con microSD visibile sul lato inferiore (foto a sinistra) - Edge Gateway con sensore HTU21D (foto a destra) |
--- |

