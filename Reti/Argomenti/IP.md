>Si occupa dell'indirizzamento e della trasmissione dei pacchetti su una rete.

## Caratteristiche:

- Si basa sull'identificazione di un Host e della sua Rete di appartenenza tramite i 32 bit di Indirizzo e [[IP#Subnet Mask / Sottoreti|Subnet Mask]];

- I pacchetti di dati vengono suddivisi in unità più piccole chiamate "pacchetti IP" prima di essere trasmessi sulla rete.
	-> ogni pacchetto IP contiene l'indirizzo di mittente e destinatario, nonché informazioni sulla rotta da seguire per raggiungere la destinazione; (v. [[IP#Header|Header]])
	
-   Il protocollo IP è un protocollo "non connesso"

- Non garantisce 
	- la consegna dei pacchetti
	- l'ordine di arrivo dei pacchetti.
(sono compiti eventualmente affidati al livello di Trasporto, IP include solamente un meccanismo di controllo degli errori tramite Checksum per assicurare che i pacchetti vengano consegnati integramente).

---
### Pro / Contro

**+** :
- Semplicità
	-> protocollo semplice da implementare e utilizzare.
- Scalabilità
	-> è progettato per funzionare in reti di qualsiasi dimensione
- Interoperabilità
	-> consente a dispositivi di diverse marche e modelli di comunicare tra loro.
- Indipendenza dal mezzo di trasmissione fisico utilizzato per la trasmissione dei dati

**-** :
- Insicurezza
	-> non include meccanismi di sicurezza integrati
-  Non garantisce la qualità del servizio
	-> non garantisce che i pacchetti verranno trasmessi in modo affidabile o in un ordine specifico.

---

## Indirizzi
Un indirizzo IP è costituito da 32 bit, indicati in notazione puntata con 4 blocchi da 8 bit ciascuno;
permettono di identificare un host sulla rete e la sua rete di appartenenza.

Dovendo essere univochi, ma essendoci ben più dispositivi collegati ad Internet rispetto al numero rappresentabile con 32 bit, sono stati divisi in due categorie: indirizzi Pubblici e Privati

### Privati
Un IP PRIVATO viene assegnato solamente all'interno di una rete locale ad un host, raggiungibile tramite quell'indirizzo da macchine interne alla stessa rete, in quanto parte del range di indirizzi non instradati dal protocollo IP su Internet.

Vi sono più range di indirizzi privati, definiti dallo IANA

Per comunicare tra di loro, reti private si interfacciano all'esterno tramite un Host con IP Pubblico (Gateway), affidandogli il compito di gestire il traffico di dati in ingresso / uscita dalla rete;
in questo modo assegnando ad un solo dispositivo (solitamente un Router) un indirizzo globalmente univoco si riduce la necessità di ind. pubblici. (v. [[NAT]])


### Pubblici
Un IP PUBBLICO è un indirizzo valido per l'accesso ad Internet, fornito dal proprio ISP ed identificante in maniera univoca un nodo su Internet.

Gli indirizzi IP privati sono stati introdotti per risolvere il problema delle risorse limitate degli indirizzi IP pubblici: consentono di utilizzare un numero maggiore di dispositivi su una rete privata senza dover utilizzare un indirizzo IP pubblico per ciascuno di essi.
Inoltre, gli indirizzi IP privati possono essere utilizzati per creare reti private virtuali (VPN), consentendo ai dispositivi di una rete privata di comunicare come se fossero sullo stesso fisico LAN.

---

## Subnet Mask / Sottoreti

> Valore a 32 bit che consente di specificare quale parte di un indirizzo IP è destinata all'identificazione della sottorete e quale parte è destinata all'identificazione dell'host.

**Sottoreti**:
	Gruppi di indirizzi IP all'interno della stessa rete. 

Una subnet mask, come un indirizzo IP, consiste in 32 bit disposti in 4 gruppi da 8.
Ogni numero rappresenta un byte della subnet mask.
I bit "1" nella subnet mask indicano quali parti dell'indirizzo IP sono utilizzate per l'identificazione della sottorete, mentre i bit "0" indicano quali parti dell'indirizzo IP sono utilizzate per l'identificazione dell'host.

#### Esempio


	IP: 192.168.1.100   >   11000000 . 10101000 . 00000001 . 01100100
	SM: 255.255.255.0   >   11111111 . 11111111 . 11111111 . 00000000
																		   prefisso rete <┘   └> host
 
	a tutti i dispositivi connessi a quel router verrà assegnato un indirizzo IP compreso nell'intervallo da 192.168.1.2 a 192.168.1.255 per essere in grado di comunicare tra di loro.

---

## Header
	20 byte se senza opzioni

L'Header funziona da "etichetta" del pacchetto, e permette agli host costituenti la rete di instradare l'informazione inviata verso la destinazione.


![[Pasted image 20230123164652.png]]

|Campo|Lunghezza (byte)|Descrizione|
|-----|--------|-----| 
|Versione|4|Indica la versione del protocollo IP in uso (v4/v6)|
|Lunghezza Header|4|Indica la lunghezza dell'header in byte (20b di default, senza "opzioni")|
|TOS|8|"Type Of Service", contiene informazioni sulla qualità del servizio richiesto per la trasmissione del pacchetto|
|||In generale il TOS è utilizzato per indicare il tipo di traffico (ad esempio voce, video, dati) e la relativa priorità, in modo tale che i router possono gestirli in modo più efficiente, garantendo una migliore qualità del servizio per i pacchetti con priorità più alta e gestendo in modo più opportuno i pacchetti con priorità più bassa.
|Lunghezza totale|16|Indica la lunghezza totale del pacchetto IP in byte (header + payload).|
|Identificativo|16|Identifica unicamente il pacchetto all'interno di una serie di pacchetti frammentati.|
|Flags|3|Contiene informazioni sul fragmentation del pacchetto|
|Offset frammento|13|Indica la posizione del frammento all'interno del pacchetto originale|
|TTL|8|Indica il numero massimo di salti che il pacchetto può fare prima di essere scartato.|
|||Ogni volta che un pacchetto attraversa un router, il valore viene decrementato di uno. Se  diventa 0 prima che il pacchetto raggiunga la sua destinazione, il pacchetto viene scartato e un messaggio di errore viene inviato al mittente.|
|Protocollo|8|Indica il protocollo superiore utilizzato dal pacchetto trasportato nel Payload (6 = TCP, 17 = UDP).|
|Checksum|16|Consente di verificare l'integrità dell'header|
|Indirizzo IP mittente|32|Indirizzo IP del mittente del pacchetto|
|Indirizzo IP destinatario|32|Indirizzo IP del destinatario del pacchetto.|
|Opzioni (facoltative)|variabile | Contiene informazioni aggiuntive sulla trasmissione del pacchetto |

---

## Frammentazione

Quando un pacchetto viene ricevuto da un dispositivo di Rete (es. Router) viene controllato il rapporto tra la dimensione del pacchetto e la [[MTU]] del dispositivo.

>L'implementazione del valore dell'MTU per la frammentazione consiste nella verifica della dimensione del pacchetto di dati che deve essere trasmesso e la sua eventuale frammentazione in pacchetti più piccoli utilizzando l'header IP e l'informazione delle Flags e Fragment Offset per poi essere ricomposti dall'host destinatario.

In caso il pacchetto fosse eccessivamente grande, il dispositivo provvederebbe a Frammentarlo per poterlo instradare sotto forma di più pacchetti di dimensione minore ("*Frammenti*").

Il dispositivo destinatario potrà poi provvedere a ricostruire il pacchetto originario dai suoi Frammenti ricevuti (grazie ai campi dell'Header "Identification", "Flags" e "Fragment Offset"):
- Identification : serve per identificare univocamente il pacchetto originale a cui appartengono i frammenti;
	-> Tutti i Frammenti di un pacchetto mantengono l' ID del Pacchetto originario.
	-> Il mittente assegna ai Pacchetti di una comunicazione ID progressivi
- Flag :
	0. Evil Bit, sempre posto a 0;
	1. Don't Fragment : indica (a valore 1) che il pacchetto non possa essere Frammentato. 
		--> se questo pacchetto incontrasse un dispositivo di rete con MTU minore viene scartato. 
			=> utilizzato per monitorare la capacità di gestione dei vari host del percorso di routing.
	2. More Fragment : indica (a valore 0) che il pacchetto è l'ultimo (o il solo) frammento del pacchetto originale.
		-> sempre 0 nei pacchetti non Frammentati;
		-> sempre 1 nei Frammenti non ultimi;
- Fragment Offset : Indica (in blocchi di 8 byte) la posizione del Frammento rispetto all'inizio del Pacchetto originale (il primo Frammento ha F.O. = 0).
	-> Es: 
	- Pacchetto : 5kB
	 - MTU nodo : 1420 B (1400 payload + 20 header)
	 - F.O. Frammento #1 : 0
	 - F.O. Frammento #2 : 175 (1400 / 8)
	 - F.O. Frammento #3 : 350 (2800 / 8)
	
	-> l'offset massimo indicabile è di 65528 byte, ma con un header "pieno" si eccederebbe la dimensione massima di un pacchetto IP

```
Se durante il tragitto un nodo ha [[MTU]] inferiore rispetto alla dimensione del pacchetto, il segmento viene frammentato dal nodo (se la flag dell'header IP "Don't Fragment" è posta a 0, altrimenti il pacchetto viene scartato ed il mittente avvisato tramite ICMP).  
Il payload del pacchetto viene suddiviso in più pacchetti di dimensioni accettabili rispetto alla MTU, mentre l'header viene copiato quasi interamente: vengono modificati il campo di Offset Fragment (indicando quale parte del pacchetto originario è contenuta nel segmento), e la flag More Segment, posta a 1 in tutte le frammentazioni del pacchetto eccetto l'ultima.  
Il payload deve essere frammentato mantenendo i fragm. offs. multipli di 8, in quanto il valore dell'header fa riferimento al valore in byte.

https://packetpushers.net/ip-fragmentation-in-detail/
```

---

## Conclusione

Il protocollo IP è uno dei protocolli fondamentali del modello TCP/IP; si occupa dell'indirizzamento e della trasmissione dei pacchetti su una rete, garantendo l'indirizzamento univoco dei dispositivi su una rete e la comunicazione tra reti diverse. Tuttavia non garantisce la consegna dei pacchetti nè l'ordine di arrivo dei pacchetti.

**+** : permette l'indirizzamento univoco dei dispositivi su una rete 

**-** : Non garantisce la consegna dei pacchetti, nè l'ordine di arrivo
**-** : Non fornisce alcuna garanzia di qualità del servizio

---

## Domande

Un IP PRIVATO viene assegnato all'interno di una rete locale ad un host, raggiungibile tramite quell'indirizzo solamente da macchine interne alla stessa rete, in quanto parte del range di indirizzi non instradati dal protocollo IP su Internet.

Un IP PUBBLICO è un indirizzo valido per l'accesso ad Internet, fornito dal proprio ISP ed identificante in maniera univoca un nodo su Internet.

La comunicazione tra i dispositivi di una rete (aventi IP Privato, identificante in maniera univoca solamente gli host interni a quella rete) con altri posti su reti esterne è gestita dal router, che tramite NAT ["Network Address Translation", Traduzione di Indirizzi di Rete] sostituisce gli indirizzi privati degli host mittenti con il proprio indirizzo pubblico e vice versa, permettendo al pacchetto di venire gestito da altre reti mantenendo contenuto il numero di indirizzi pubblici necessari.