
### Cos'è 

>Un insieme di protocolli di comunicazione utilizzati per la trasmissione dei dati su reti di computer

### Cosa fa

Divide la comunicazione in diversi livelli o "layer" per poter gestire efficacemente la trasmissione dei dati su una rete.
Ciò permette di isolare e gestire specifiche funzionalità della comunicazione su differenti livelli, rendendo il sistema più flessibile e facile da gestire.

**IP** (Internet Protocol) si occupa dell'indirizzamento e della trasmissione dei pacchetti su una rete. 

**TCP** (Transmission Control Protocol) si occupa della gestione del flusso di dati e della correzione degli errori;

I livelli superiori gestiscono funzioni come l'autenticazione, la crittografia e la gestione dei servizi di rete.

### Livelli

5. [[Livello Applicativo|Applicativo]]
4. [[Livello di Trasporto|Trasporto]]
3. [[Livello di Rete|Rete]]
2. [[Livello di Collegamento|Collegamento]]
1. [[Livello Fisico|Fisico]]



### Approccio verso la comunicazione
Il protocollo TCP IP utilizza l'approccio Divide et Impera verso le informazioni da trasmettere:
i dati vengono divisi in pacchetti più piccoli ed etichettati con l'aggiunta di Header (intestazioni) per permettere ai protocolli di gestirli correttamente.