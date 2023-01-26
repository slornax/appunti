> Si occupa della comunicazione tra due applicazioni poste su host different

Una volta che il [[Livello Applicativo|processo]] ha fornito il payload da comunicare, il protocollo del livello di trasporto lo impacchetta, e lo affida al [[Livello di Rete]] per instradarlo verso la porta del processo dell'host della rete di destinazione.

Permette la comunicazione di più applicazioni su uno stesso host utilizzando il concetto di Socket.

---
### Dispositivi

- Gateway
- Server Proxy

---
## Protocolli
- [[TCP]]
- [[Livello di Trasporto#UDP|UDP]]

---
### Socket (e Porta)
#Socket
Porta:
> Valore numerico rappresentante endpoint per la comunicazione tra due dispositivi in una rete.

Socket:
> Interfaccia di comunicazione tra due host utilizzato dal protocollo di trasporto appropriato.

> Strumento offerto dal Sistema Operativo alle applicazioni per usare le funzionalità della rete.

> Portale magico da cui un host butta dati e che compaiono dall'altro.

Insieme all' [[IP|Indirizzo IP]] permette a due processi di comunicare via rete, conoscendo oltre agli host comunicanti anche i processi che stanno utilizzando quei pacchetti.

#### Tipi di Porte
- Well Known (o "Porte di Sistema")
	0 -> 1023 : assegnate dall' Internet Assigned Numbers Authority, sono standardizzate per vari servizi (generalmente assegnate al Sistema Operativo o a processi di sistema), solitamente in ascolto per applicazioni con funzioni di Server. 
- "Porte Utente" : 1024 -> 65535
	- Registrate
		1024 -> 49151 : utilizzate come riferimento fra applicazioni, assegnate dallo IANA.
	- Dinamiche
		49152 -> 65535 : liberamente utilizzabili da tutte le applicazioni utente. Utilizzate per servizi privati o personalizzati, o come porta effimera (utilizzata solo per la durata della comunicazione).

In generale, i socket possono essere utilizzati per creare diverse tipologie di connessioni di rete, tra cui:

-   Connessioni a socket client-server: in questo caso, un'applicazione client si connette a un'applicazione server utilizzando un socket. Il client invia richieste al server, che le elabora e invia le risposte appropriate.
-   Connessioni a socket peer-to-peer: in questo caso, due applicazioni si connettono tra loro direttamente utilizzando socket.
-   Connessioni multicast: in questo caso, un'applicazione invia un pacchetto a un gruppo di destinatari utilizzando un indirizzo multicast.

![[Pasted image 20230118113534.png]]

---
