	"Metodo di Trasmissione"

> Trasmissione telematica di dati su apposite linee dedicate.

---
Vi sono due tipologie di Commutazione
1. di Circuito
2. di Pacchetto

## C. di Circuito

> La capacità del canale trasmissivo è interamente dedicata alla comunicazione tra i due host

Le risorse necessarie per la comunicazione dei due host sono riservate per l'intera durata della sessione di comunicazione.

**+** : 
- il canale è sempre disponibile 
- il ritardo è contenuto per comunicazioni costanti

**-** : 
- in caso di utilizzo discontinuo vi è uno spreco di risorse
‎  

	CGPT: "La commutazione a circuito (o circuit switching) è un metodo di inoltro dei dati in cui una connessione fisica dedicata viene stabilita tra due dispositivi prima di iniziare la trasmissione dei dati. La connessione rimane attiva per tutta la durata della sessione di comunicazione e viene liberata solo quando la sessione si conclude."


## C. di Pacchetto

> L'informazione viene distribuita in pacchetti indipendenti, adeguatamente etichettati dai protocolli per garantirne la corretta circolazione sulla rete.

L'informazione viene suddivisa in unità ("pacchetti") più piccoli, e comunicata all'host di destinazione attraverso unità di rete intermedie.

Packet-based systems are based on the idea of sending a batch of data, the _packet_, along with additional data that allows the receiver to ensure it was received correctly, perhaps a Checksum.


**+** :
- Utilizzo efficiente delle risorse di rete
	-> i pacchetti, venendo trattati indipendentemente tra di loro, permettono un utilizzo efficiente delle risorse di rete, poiché possono utilizzare percorsi di rete diversi per raggiungere la destinazione.

- Scalabilità e Tolleranza ai guasti
	-> è possibile aggiungere facilmente nuovi utenti o dispositivi alla rete p gestirne i guasti senza interferire con le comunicazioni esistenti. 

**-** : 
- Latenza variabile
	-> poiché i pacchetti possono essere indirizzati attraverso percorsi diversi per raggiungere la destinazione, la latenza della comunicazione di pacchetto può variare in modo significativo, rispetto alla commutazione a circuito (in cui la latenza è più prevedibile)
    
-  Richiede più elaborazione
	->poiché i pacchetti devono essere divisi in unità più piccole e etichettati con indirizzi di destinazione e mittente, la comunicazione di pacchetto richiede più elaborazione rispetto alla commutazione a circuito. Inoltre lo Store & Forward implica un ulteriore attesa nei vari hop della comunicazione.
    
-  Maggiore possibilità di errore
	-> poiché i pacchetti vengono trasmessi indipendentemente l'uno dall'altro, c'è una maggiore possibilità che i pacchetti vengano persi o che ci siano collisioni tra pacchetti in transito nella rete.

---


##  Extra  - CdP

### Inoltro pacchetti
	I dispositivi di commutazione di pacchetto (come i router) in una rete utilizzano una tabella di inoltro per determinare come inoltrare i pacchetti. La tabella di inoltro contiene informazioni su come raggiungere una determinata destinazione, in base all'indirizzo IP di destinazione contenuto nel pacchetto.

-----------------------------
### Store and Forward
	 I pacchetti vengono inoltrati attraverso la rete utilizzando un metodo di inoltro chiamato inoltro di store and forward. In questo metodo, il dispositivo di commutazione riceve il pacchetto, lo memorizza temporaneamente in una memoria buffer e quindi lo inoltra alla prossima interfaccia di uscita. Ciò consente al dispositivo di controllare la trasmissione del pacchetto e di gestire eventuali errori di trasmissione.
	 
-----------------------------


## Ritardi nella Comunicazione
1. Elaborazione
2. Accodamento
3. Trasmissione
4. Propagazione

### Cause:
E: il tempo richiesto per esaminare l'intestazione del pacchetto per detereminare dove dirigerlo.

A: una volta accodato in un dispositivo, il pacchetto attende la sua trasmissione sul collegamento.

T: dipendente dalla velocità di trasmissione e dalla dimensione del pacchetto
	-> T = dim / Vel

P: tempo di attraversamento fisico del mezzo trasmissivo (hop all'host successivo nella rete)


Se nella rete un dispositivo (es. un Router) iniziasse a ricevere pacchetti più velocemente di quanto non riesca ad inviarne ([[Throughput]] ingresso > [[Throughput|T.]] Uscita) , la coda crescerà fino a riempirsi, e i pacchetti arrivanti in eccesso verranno scartati.

----

## Load Bouncing

> Tecnologia utilizzata per distribuire in modo equilibrato il carico di lavoro tra più dispositivi o server in una rete.

Il Load Balancing consente di migliorare la disponibilità e le prestazioni di un sistema distribuito, riducendo la sovrapposizione del carico su singoli dispositivi o server per garantire che siano tutti utilizzati in modo efficiente.

Consente di:
-   Aumentare la disponibilità del sistema:
		-> se un server non è disponibile, le richieste verranno inoltrate a un altro server.
-   Migliorare le prestazioni del sistema:
		-> le richieste vengono distribuite in modo equilibrato tra i server, riducendo la sovrapposizione del carico su singoli server.
-   Ridurre i tempi di inattività:
		-> se un server si blocca, le richieste verranno inoltrate a un altro server.
-   Scalare il sistema in modo trasparente:
		-> è possibile aggiungere o rimuovere server senza interrompere le richieste in corso.


Se in pacchetti possono attraversare due reti con tempi di trasporto uguali, 
- se vi è solo un pacchetto, il percorso viene scelto a caso;
- altrimenti vengono smistati nei vari percorsi, così da ridurre il carico all'interno della rete.

La velocità di comunicazione su una rete è osservabile tramite i tool `ping` e `traceroute`.

---

### DOMANDE
> Differenza Commutazione di Circuito / Pacchetto

C : 
	viene stabilita una connessione persistente tra i due host, mantenuta indipendentemente dal traffico; 
	permette di avere una connessione più rapida, ma a costo di uno spreco del mezzo di trasmissione quando gli host sono inattivi.  

Si ha quando una parte della capacità trasmissiva totale in uscita al mutilplatore è stabilmente assegnata a ciascun canale tributario in ingresso; ogni utilizzatore ha a disposizione un canale trasmissivo dedicato, con la garanzia di avere sempre disponibile tutta la capacità allocata ad ogni richiesta di servizio.  


P : il mezzo di comunicazione viene utilizzato solamente quando un pacchetto deve essere  
trasportato.  
Permette di ottimizzare il consumo del collegamento solamente in base al traffico, ed essendo i pacchetti identificati singolarmente, è possibile trasmettere pacchetti destinati a comunicazioni diverse su un singolo mezzo fisico condiviso.