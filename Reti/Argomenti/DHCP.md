	Dynamic Host Configuration Protocol

Ha lo scopo di assegnare automaticamente le configurazioni di rete agli host richiedenti.

> Consente di assegnare automaticamente informazioni sulla rete ed un indirizzo IP ad un dispositivo richiedente. 

Configurazione Minima:
- Indirizzo IP
- Subnet Mask

Extra:
- Default Gateway
- IP di server vari (DNS, NTP, TFTP, ...)


Tipologie di Assegnazioni DHCP:
- Dinamica
	-> alla scadenza del periodo di Lease l'IP viene assegnato ad altri host richiedenti
- Automatica
	-> anche dopo la scadenza del periodo di Lease, il server riassegnerà all'host lo stesso IP quando ne farà nuovamente richiesta
- Statica
	-> niente periodo di lease, l'IP è assegnato all'host in modo permanente (es. stampanti di ufficio)

---

Viene implementato da un Server DHCP, dispositivo che in base alla sua configurazione (intervallo di IP, durata del lease, informazioni sulla rete) soddisfa le richieste di un host appena immesso nella rete.
Tale server manterrà poi una tabella con l'Indirizzo Ip fornito, l'indirizzo MAC della scheda di rete dell'host, ed il tempo di Lease.

---
### Funzionamento:

>Protocollo di Network Management utilizzante UDP su IP.

Server ed Host ascoltano rispettivamente sulle porte 67 e 68,

### DORA 
	Discover, Offer, Request, ACK.

#### DHCP Discover
1.  L'host (chiamato "client DHCP") che si connette alla rete invia un pacchetto di richiesta DHCP in Broadcast per ottenere un indirizzo IP: 
	- IP Sorgente: 0.0.0.0
	- IP Destinazione: 255.255.255.255
	- Type: "DHCP"
	- Transaction ID: numero scelto dall client per riconoscere la comunicazione.

### DHCP Offer
2.  I server DHCP ricevono la richiesta e rispondono inviando sulla rete una proposta di indirizzo IP disponibile, insieme a altre informazioni come la maschera di sottorete, la gateway predefinita e i server DNS.
	Il pacchetto contiene:
	- IP Sorgente: IP del Server
	- IP Destinazione: 255.255.255.25
			-> la trasmissione viene comunque fatta in Unicast basandosi sul MAC address
	- Type: "DHCP"
	- Transaction ID: il valore indicato dal client nella richiesta Discover

### DHCP Request
3.  Il client riceve le offerte, scegliendone una e rispondendo al server a cui è interessato con un pacchetto contenente:
	- IP Sorg: 0.0.0.0
	- IP Dest: 255.255.255.255 
			--> in Broadcast perché in fase di richiesta, il client non conosce ancora l'indirizzo del server DHCP; inoltre comunica a tutti i server DHCP quello scelto.
	- Type: "DHCP"
	- Transaction ID: nuovo valore identificativo (la comunicazione è composta da un messaggio inviato ed uno ricevuto)

### DHCP ACK
4. Il server risponde al messaggio confermando i parametri ricevuti
	- IP Sorg: IP del Server
	- IP Dest: nuovo IP dell'host
	- Type: "DHCP"
	- Transaction ID: valore della Request

Dall'invio del messaggio di DHCP ACK da parte del server l'host può usare le informazioni di rete ottenute (inizia il Periodo di Lease).

### Esempi di Pacchetti DORA

#### 1. Host -> Server
![[Pasted image 20230126172407.png]]

#### 2. Server -> Host
![[Pasted image 20230126172138.png]]

### 3. Host -> Server
![[Pasted image 20230126172237.png]]

### 4. Server -> Host
![[Pasted image 20230126172315.png]]


---

Il DHCP funziona tramite un sistema di "prenotazione" degli indirizzi IP. Il server DHCP mantiene un elenco di indirizzi IP disponibili e assegna un indirizzo IP al client solo per un periodo di tempo limitato, chiamato "periodo di locazione". Dopo il periodo di locazione, il client deve richiedere nuovamente un indirizzo IP al server DHCP.

Il DHCP utilizza un sistema di "leasing" per gli indirizzi IP, in cui gli indirizzi IP possono essere assegnati solo per un periodo di tempo limitato. Ciò consente al server DHCP di riutilizzare gli indirizzi IP non utilizzati e di gestire in modo efficace l'assegnazione degli indirizzi IP sulla rete.

Inoltre il DHCP ha anche una funzione opzionale di assegnazione di un hostname al dispositivo che richiede un indirizzo ip, questo può essere utile per identificare in modo facile il dispositivo all'interno della rete.

---
### Domande

Il DHCP ["Dynamic Host Configuration Protocol"] è il protocollo che permette ad un host di  
ottenere diversi parametri necessari al funzionamento all'interno di una rete (principalmente IP, gateway, indirizzi di server).

Funziona tramite lo scambio di messaggi con un (o più) Server DHCP:  
1) L' host, inizialmente con indirizzo IP 0.0.0.0, invia un messaggio di DHCP Discover in Broadcast (255.255.255.255), specificando il suo indirizzo MAC.  

2) Il DHCP server risponde in unicast all'indirizzo MAC mittente con una DHCP Offer, un messaggio dove propone all'host un indirizzo IP valido e gli altri parametri da assegnargli.  

3) Il client, dopo aver ricevuto una o più offerte, ne seleziona una ed invia un pacchetto di DHCP Accept in broadcast, indicando con il campo "Server Identifier" quale proposta ha accettato.  

4) Il server selezionato conferma l'assegnazione con un pacchetto di DHCP Ack (in unicast).
