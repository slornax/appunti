
>Processo di scoperta del miglior percorso su cui instradare un pacchetto.

	‎ es. valore migliore = minor numero di Hop necessari ad arrivare a destinazione

### Instradazione diretta
Se due host A e B appartengono alla stessa rete (= hanno stesso prefisso) i pacchetti possono raggiungere la destinazione senza dover uscire da un Gateway.

Se invece il Mittente confrontando gli indirizzi in base alla Subnet Mask identifica il Destinatario su una rete esterna (default case) invia il pacchetto al Router, che lo instraderà verso la rete di destinazione (entrambi i Router "[[NAT|NATteranno]]" il pacchetto).


### Astrazione di una rete con Modello a Grafi

	I Nodi vengono rappresentati come Vertici, ed i collegamenti come archi

![[Pasted image 20230126125231.png]]

Ad ogni collegamento tra due Nodi è assegnato un *Costo*, in maniera tale da poter osservare il tragitto più conveniente su cui instradare il pacchetto.


COSTO: 
	C = (A, B)      --> Costo del tragitto tra i nodi A e B 
```Costo
valore su cui si quantifica il costo del percorso verso il nodo
```
Costo Tragitto Mittente-Destinatario = Somma dei costi tra i nodi attraversati.

	Se un algoritmo di Routing tiene conto del numero di hop come valore del costo, ogni collegamento ha peso 1; per la  velocità di trasmissione, i pesi sono proporzionali alle dimensioni delle Bande tra i nodi.
	

es. con un nodo n1 di separazione:
	T = (M, D) = (M, n1) + (n1, D)
 
---
# AS
	Autonomus System
> Gruppo di Router e Reti sotto il controllo di una singola autorità amministrativa.

Esempio lampante sono gli ISP: grandi reti (nell'ordine di blocci CIDR /8) comunicanti tramite Gateway di Bordo.

---
## Gateway di Bordo

>[[Gateway]] per il traffico con una rete esterna.

---

# INTRA - INTER AS


Per un Router di un ISP la Tabella di Routing sarà composta da destinazioni interne allo stesso AS + destinazioni esterne, calcolate in base ai dati comunicati dai Gateway dell'AS (esterno).

## IGP - Routing Intra-AS
	Interior Gateway Protocol
> Tipologia di Classe di Protocolli di Routing Interno ad una rete

Permette lo scambio delle tabelle di Routing tra i Nodi in maniera autonoma.
I due Algoritmi principali / sottoClassi di Protocolli sono:
- [[Routing#Distance Vector Routing|Distance Vector]] 
- [[Routing#Linked State|Linked State]]

|Distance Vector||Linked State|
|---|---|---|
|Broadcast|metodo di scambio informazioni|Multicast|
|costo, destinazione hop|memorizzano|informazioni sulla banda|
|lenta; soffre di Routing Loop|convergenza|veloce grazie alla conoscenza della rete|
|limiti di hop count; inadatto a grandi reti|scalabilità|scalabile|

---
### Distance Vector Routing

Ogni tabella di Routing indica per ogni destinazione destinazione "Next Hop" (nodi con cui il dispositivo comunica) il costo del salto.
Comunicandosi le proprie Tabelle, ogni Nodo riuscirà a riconoscere il tragitto migliore su cui instradare un pacchetto.

> Sistema che permette ai Nodi di autoconfigurarsi all'avvio, apprendendo la disposizione della rete dai vari Router, tramite l'algoritmo Bellman Ford.

 - Bellman-Ford distribuito:
	1. All'avvio il Router memorizza solamente la distanza con i dispositivi adiacenti e le interfacce su cui raggiungerli;
	2. Invia ai vicini la propria tabella, e ne riceve le loro;
	3. Se nelle tabelle ricevute risulta un aggiornamento della rete, aggiorna la sua;
	4. Comunica la nuova tabella ai vicini;

	Terminati gli aggiornamenti tra i nodi, ogni Router avrà un Modello della rete completo e stabile
		--> la rete è configurata.

	Si può dimomstrare che per un grafo completamente connesso con archi di peso > 0, l'algoritmo DV converge alla souzione ottima, ed ha un numero di iterazioni necessarie alla stabilizzazione della rete proporzionale al diametro del grafo.


- Un Router si comporta seguendo la propira Tabella di Routing, indicante per ogni destinazione raggiungibile i nodi di Next Hop ed il costo per raggiungerli.


**+** :
	\+ Semplice
	\+ Possibile trasmettere modifiche alla rete
**-** :
	\- I nodi si scambiano molti dati
	\- La convergenza tra N Router può richiedere NxN messaggi
	\- Difficoltà nel riconoscere [[Routing#Routing Loop|Routing Loop]]


---
### Linked State
	o "Routing Dinamico"

Ogni Router partecipa alla definizione di una Tabella delle Adiacenze, informando gli altri nodi sulla rete dei costi di Hop verso i suoi vicini.

>Algoritmi centralizzati in cui ogni Router ha una mappa della rete da cui poter calcolare la strada migliore.

Ogni Nodo mantiene una tabella dettagliata di tutti i link presenti nella rete e le loro caratteristiche, in modo da avere informazioni riguardanti tutta la rete ed utilizzarle per calcolare il percorso ottimale verso la destinazione con l'algoritmo di Dijkstra.

```chatGPT
Gli algoritmi di routing Link State utilizzano una rappresentazione dettagliata della topologia della rete per calcolare il percorso ottimale verso una destinazione.

Sono molto precisi e scalabili, ma richiedono una maggiore quantità di memoria e potenza di elaborazione rispetto agli algoritmi di routing a stato di protocollo.
```

 **+**:
	\+ Stabile
	\+ Convergenza veloce
	\+ Ogni tipo di cambiamento può essere comunicato facilmente
**-** :
	- Complessi
	- Richiede maggior quantità di memoria 
		-->(deve essere memorizzata tutta la rete)
	- Richiede maggiroe potenza di elaborazione
		--> per calcolare il miglior tragitto a Real-Time

I Router si scambiano pacchetti LSP per la configurazione:
ogni Nodo annuncia i costi dei suoi hop verso gli host adiacenti, creando così una Tabella di Adiacenze, una effettiva Mappatura della rete.

### LSP
	Linked State Packets
>Pacchetto di informazioni generato da un Router in un Protocollo di tipo Linked-State contenente l'elenco dei nodi adiacenti.


---
Entrambe le classi di algoritmi possono soffrire, a causa di guasti di nodi, di [[Routing#Routing Loop|Routing Loop]]. (v. [[IP#Header|IP TTL]])

---
## EGP 
	External Gateway Protocol

I protocolli di instradamento Inter-AS sono necessari per la comunicazione tra host posti su sistemi autonomi diversi.

### BGP
	Border Gateway Protocol
> Protocollo di Routing Inter-AS a Path Vector ("indicazione di percorso")

Attuale standard di routing tra AS, instrada i pacchetti verso subnet (prefissi di CIDR) 
- Dispone un Router a:
	\- Ottenere informazioni sulla raggiungibilità dei prefissi di sottorete dei dispositivi confinante
		-> consente ad una sottorete di comunicare la sua esistenza al resto di Internet
	\- Seguire politiche per determinare i percorsi ottimali verso le sottoreti
	\- Propagare le informazioni a tutti i Router dell'AS

- Supporta il Routing indipendente dalle Classi

- Usa un meccanismo di aggregazione degli instradamenti per diminuire le dimensioni delle tabelle

		La sua decentralizzazione ha permesso ad Internet di diventare un sistema completamente decentralizzato



### Se due AS comunicano da più Router di Bordo?
Viene scelto il percorso verso il R.d.B. più vicino all'host Mittente, seguendo la Hot Potato Routing.

## Hot Potato Routing

>il percorso scelto  è quello con il minor costo per il router NEXT-HOP che lo inizia.

Trasmettere i dati immediatamente al next hop, senza attendere che il pacchetto sia completamente elaborato.

```appuntiAmos

L’idea alla base è che il router butti fuori i pacchetti dal suo AS quanto prima (minor costo possibile) senza preoccuparsi del costo delle restanti tratte del percorso al di fuori del suo AS.
```


---


### Routing Loop
> Tempo di stabilizzazione della rete durante cui una tabella aggiornata viene propagata sulla rete, durante cui un Router può avere (e divulgare) una Routing Table errata.
