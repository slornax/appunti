## TCP
	Transmission Control Protocol
> Protocollo di Trasporto Affidabile

TCP consente alle applicazioni di comunicare tra loro in modo affidabile e controllato, fornendo funzionalità come il controllo del flusso, la riallocazione degli errori e la sincronizzazione dei pacchetti.

**+**:
- Affidabile ( [[Livello di Trasporto#ACK|ACK]] / [[TCP#Controllo Errori|Check Errori]] )
	-> Arrivo garantito dei pacchetti nell'ordine di invio
	-> Rilevazione di errori di trasmissione dei segmenti
- [[TCP#3 Way Handshake|Connection Oriented]] (protocollo di tipo Stream)
- [[TCP#Congestion / Flow Control|Controllo stato della rete]]	
	- meccanismi di riscontro dei segmenti, controllo di flusso e controllo della congestione.
- [[Livello di Trasporto#Header|Header più esaustivo]]

**-**:
- Non supporta il Multicast
- Lento e costoso
- Senza funzionalità di sicurezza incorporate

---
## Header

![[Pasted image 20230120163406.png]]

- [[Livello di Trasporto#Socket (o Porta)|Porte]] Sorgente / Destinazione
- [[TCP#Sequence Number / ACK|Sequence Number]]
	-> identificativo dei segmenti in una sequenza
- [[TCP#Sequence Number / ACK|ACK]]
	-> identificativo del segmento riscontrato
- Dimensione Header (in byte)
- Bit riservati
	-> per un futuro sviluppo del protocollo (es. espansione dei bit di flag) e fino ad allora di default a 0
- Flag
	-> NS, CWR, ECE, URG, ACK, PSH, RST, SYN, FIN
- [[TCP#Congestion / Flow Control|Window Size]]
- [[TCP#Congestion / Flow Control|Checksum]]
- Urgent Pointer
- Opzioni

\+ Payload

---
## Sequence Number / ACK

Initial SN:
>Identificativo univoco di ogni nuova connessione TCP.

SN: ISN + segmenti ricevuti
ACK: SN + 1 (il primo byte che il destinatario si aspetta di ricevere)

I Segmenti trasportati dal TCP sono identificati da un valore scelto dall'host mittente, indicante la posizione del segmento nel flusso di dati.
Ciò permette la consegna ordinata dei pacchetti.

Una volta ricevuto il segmento, l'host destinatario lo comunica (il ricevimento) al mittente indicandogli il valore del prossimo segmento che si aspetta di ricevere.

![[Pasted image 20230119171512.png]]

---
## 3 Way Handshake

> Scambio di messaggi tra due Host per instaurare o chiudere una connessione TCP

#### Apertura:
Si compone dallo scambio di pacchetti di richiesta di connessione (Flag "SYN" posta a 1) e dei relativi ACK
- SYN
	-> Sequence Number ontenente un valore scelto dal mittente
- SYN - ACK
	-> SN con valore scelto, ACK pari all'incremento del SN dell'ultimo segmento ricevuto
- ACK	
	-> ACK pari all'incremento del SN dell'ultimo segmento ricevuto
	
A questo punto la connessione è stabilita e i due host possono cominciare a scambiare dati.

#### Chiusura:
- FIN
- FIN - ACK
- ACK

---
## Socket

Ciascuna connessione TCP è associata ad una [[Livello di Trasporto#Socket (o Porta)|Socket]] aperta da un processo, utilizzante la [[Livello di Trasporto#Socket (e Porta)|Porta]] indicata nei 16 bit contenuti nell'[[TCP#Header|Header]].

---
## Congestion / Flow Control

Il protocollo utilizza un meccanismo di controllo del flusso per evitare la sovraccarico del destinatario. Il destinatario informa il mittente della quantità di dati che è in grado di ricevere e il mittente invia i dati in base a questa informazione.

### Controlli della Congestione
> Reazione del protocollo in caso di traffico sul mezzo trasmissivo

Permette di limitare la quantità di pacchetti inviati e non ancora riscontrati dal mittente, adattando il flusso dati allo stato della Rete.



### Controlli di Flusso
> Azione preventiva per limitare la quantità di dati immessi in Rete

Evita che un mittente invii una quantità di dati eccessiva rispetto alla Rete di trasporto (es. mantenendo il flusso entro il [[Throughput]] minimo dei vari router)


### Sliding Window
>Protocollo di Controllo del flusso di dati nella Rete tramite ACK.

Ponendo un limite al numero di pacchetti trasmissibili o ricevibili, lo Sliding Window permette la comunicazione di quantitativi variabli di segmenti inviabili prima della ricezione degli ACK dei pacchetti precedentemente inviati.
 
![[Pasted image 20230120175702.png]]
```
When the endpoints establish a TCP session, the Window size field in the TCP header will be used to signal the receiving buffer capacity and govern the amount of data that can be received and processed. Each endpoint will maintain a local **receive window** (**RWND**). This is the maximum amount of data the receiver can receive for buffering and processing. The endpoint will include this RWND value in the TCP header. The sender uses RWND as input to decide the Sliding window size. It can send TCP segments to a peer of size defined in the window size before waiting for an acknowledgment.
```

Il destinatario comunica ad ogni pacchetto al mittente la sua attuale Finestra di ricezione (**R**eceive**W**i**ND**ow) usando il campo dell'header 
	`The TCP header uses a 16 bit field to report the receiver window size to the sender. Therefore, the largest window that can be used is 216 = 64 kilobytes.`


Viene posto un limite al quantitativo di segmenti che possono venire inviati prima di ricevere gli ACK dei precedenti.
Il nome "Slicing Window" viene dal fatto che il numero di segmenti inviabili viene modificata osservando i comportamenti della rete (in base ai pacchetti inviati e agli ACK ricevuti):
Se vengono riscontrati errori nella ricezione della conferma dei pacchetti, la Finestra viene rimpicciolita;
In caso si osservasse una risposta positiva nella comunicazione alla dimensione della finestra, viene gradualmente incrementata.

 <iframe height="600px" width="800px" src="https://www.youtube.com/embed/zY3Sxvj8kZA" title="TCP sliding window animation" frameborder="0" allowfullscreen></iframe>
 
0. Il destinatario comunica la dimensione della sua RWND al mittente
1. Vengono inviati i primi RWND pacchetti
\2\a. Quando vengono ricevuti, la Finestra viene incrementata
\2\b. Se vengono riscontrati errori, la Finestra viene diminuita
^^^
i punti 2a e 2b variano in base agli algoritmi implementati, principalmente:

Incremento:
- Slow Start
- Congestion Avoidance

Diminuzione:
- "Vanilla"
- Fast Retrasmit / F. Recovery

#### Incremento Finestra

##### Slow Start
> Per ogni ACK ricevuto aumento la Finestra di un segmento.

L'evoluzione della CWND ha andamento esponenziale (1 > 2 > 4 > 8 > 2^n).
I limiti dello Slow Start sono i valori di SSThreshold e della RCVWND.

![[Pasted image 20230122095020.png]]



##### Congestion Avoidance
> Per ogni ACK ricevuto, la finestra aumenta di 1 / CWND

L'evoluzione della CWND ha aumento lineare (dal raggiungimento della Threshold fino alla capacità massima della rete).

![[Pasted image 20230122095050.png]]


#### Riduzione della Finestra
> In caso di scadere del RTO, si pone la CWND a 1 (Vanilla) o a CWND/2 (FRecovery/Fretransmit) e si ritrasmette la finestra dei segmenti persi.

### Fast Retransmit - Fast Recovery

Per un errore di trasmissione su un segmento, il mittente reagisce in maniera sproporzionata con l'algoritmo visto prima.

Vi sono perciò due algoritmi (implementati sempre in coppia) progettati per le perdite singole: 
- Fast Retransmit
	-> il segmento considerato perso viene subito ritrasmesso
- Fast Recovery
	-> la CWND non viene chiusa in maniera eccessiva

Se al mittente arrivano ACK duplicati (sintomo della perdita di un pacchetto su una rete funzionante):
1. si pone  SST = CWND / 2
2. si ritrasmette il segmento senza attendere lo scadere del RTO 
	-> Fast Retransmit
3. si pone CWND = SST + *num di ACK duplicati ricevuti*
	-> Fast Recovery
4. per ogni ACK duplicato successivo la CWND aumenta di 1

Fino a quando il mittente riceve l'ACK del segmento ritrasmesso:
5. si pone CWND = SST
	-> si procede seguendo il Congestion Avoidance

CASI PARTICOLARI
- Se il segmento ritrasmesso viene perso, si attende lo scadere del RTO, la CWND torna a 1 e si riparte in Slow Start
- Se vengono persi più di un segmento, l'algoritmo cerca di recuperare il primo, ma anche in caso di riuscita arriverebberero gli ACK duplicati dei successivi segmenti persi (che l'algoritmo non sa gestire, scadrebbe quindi il RTO).

---
### EVOLUZIONE DELLA CWND

Variabili considerate:
- CWND
	-> Dimensione della finestra di trasmissione
- RECWND
	-> Dimensione della finestra di ricezione annunciata dal Destinatario
	--> Limite massimo della CWND.
- SSTHRESH
	-> Slow Start Threshold, dimensione della finestra che, una volta raggiunta, implica il passaggio da Slow Start a Congestion Avoidance
	-> limite massimo della finestra per l'utilizzo di Slow Start prima di passare a Congestion Avoidance
- [[TCP#RTT|RTT]]
	-> tempo trascorso tra l'invio di un segmento e la ricezione del suo ACK
- [[TCP#RTO|RTO]]
	-> tempo che il mittente attende un ACK prima di considerarlo perso


### Algoritmo di Regolazione CWND

0. ad inizio trasmissione si pone 
	- CWND = 1 segmento (pari in byte al MSS)
	- RCVWND = Indicato dal Destinatario
	- RTO = Calcolato durante l'apertura della connessione
	- SST = RCWND  (o RCWND / 2) 

1. vengono inviati i segmenti della CWND. 
 
2. quando ne arrivano gli ACK: 
	\a. se CWND < SST, la CWND aumenta seguendo lo Slow Start
	\b. una volta raggiunta la SST, la dimensione di CWND è regolata secondo il Congestion Avoidance fino al raggiungimento della RCVWND
	
3. in caso di scadenza dell'RTO:
	- pongo la Threshold pari alla metà della finestra all momento della perdita SST = CWND / 2 
	- RTO = RTO * 2
	\a. CWND = 1 (Vanilla)
	\b. CWND = SST (FRec/FRet)
	
```slide
###### In caso di un mancato ACK
- La trasmissione si interrompe (la finestra non scorre, verranno ritrasmessi gli ultimi segmenti inviati)
- Si attende il RTO
- Allo scadere dell' RTO:
	-> SST viene dimezzata
	-> CWND riparte da 1
- Viene raddoppiato il valore di RTO

##### In caso di perdite consecutive
- SST si pone a 2


#### Cause di perdita di segmenti
- Trasmissione (generalmente influisce un singolo segmento)
- Congestione (influenza più segmenti)
```


---
## Controllo Errori

Utilizza un meccanismo di controllo di errore e di ritrasmissione dei pacchetti per garantire che tutti i pacchetti inviati vengano correttamente ricevuti. In caso di pacchetti persi o danneggiati, vengono inviati nuovamente.

### RTO
	Retransmission Time Out

>Valore della finestra temporale per cui un host aspetta l'ACK di un segmento prima di considerarlo perso.

Viene calcolato da un Host in base al valore del Round Trip Time.

	CGPT:
	"Il RTO (Retransmission TimeOut) è un meccanismo utilizzato dal protocollo TCP per gestire la ritrasmissione dei pacchetti persi o non confermati.

	Quando un host invia un pacchetto di dati, aspetta un certo periodo di tempo per ricevere un pacchetto di conferma (ACK) dall'altro host. Se l'host non riceve il pacchetto di conferma entro il periodo di tempo previsto, assume che il pacchetto sia stato perso e lo ritrasmette. Questo periodo di tempo è chiamato RTO (Retransmission TimeOut).

	Il valore iniziale del RTO viene stabilito in base a una stima iniziale, ad esempio utilizzando un valore predefinito come 1 secondo, oppure utilizzando un algoritmo di stima come il metodo di Jacobson-Karels.

	Il valore del RTO viene aggiornato dinamicamente durante la connessione in base alla latenza effettiva della rete. Ciò significa che se il RTO originale è troppo breve, ci saranno troppe ritrasmissioni e quindi una maggiore perdita di banda. Se il RTO originale è troppo lungo, ci saranno ritardi nella ritrasmissione dei pacchetti persi.

	Il RTO è un fattore importante nella gestione delle prestazioni del protocollo TCP, poiché influisce sulla velocità di trasmissione dei dati, sulla quantità di banda utilizzata e sulla latenza della connessione."
	
### RTT
	Round Trip Time
>Tempo passato dall'invio di un segmento e la ricezione del suo ACK

Viene calcolato come:        

RTT = Tempo di Trasmissione + 2 * Tempo di Propagazione  
             ^^^ (dim. seg. / vel. trasmis.)

Più stime possono essere combinate insieme per calcolare il RTT medio (RTT Stimato) in modo da modellare il valore in base ai pacchetti più recenti, dando loro un peso esponenzialmente decrescente: 

RTTs = a * RTTs + (1 - a) * RTT            ,   0 < a < 1

---

Congestion Control:
https://www.telematica.polito.it/app/uploads/2018/12/tcp_congestion_control_2.pdf



		       _
       .__(.)< (MEOW)
        \___)   
	~~~~~~~~~~~~~~


Congestion Window:
https://www.corsi.univr.it/documenti/OccorrenzaIns/matdid/matdid474698.pdf