> Entità di rete che soddisfa le richieste HTTP al posto del Web Server effettivo.

-> si interpone tra Client e Server, intercettando richieste e risposte
-> ha una memoria propria, contenente le copie di oggetti recentemente richiesti dai Client
-> permette di alleggerire il traffico di dati nella rete

---
### Tipologie
Ci sono diversi tipi di cache: browser cache, proxy cache, CDN cache.
- Browser Cache
	-> più comune, consiste nella memorizzazione delle risorse richieste dal browser sull'host dell'utente.

- Proxy Cache
	-> simile al caching del browser, in questo caso le risorse vengono memorizzate in un [[#Proxy Server:|server proxy]] invece che sul dispositivo dell'utente.

- CDN cache
	-> (Content Delivery Network) è simile al caching del proxy, ma in questo caso le risorse vengono memorizzate in server distribuiti geograficamente, in modo che gli utenti possano accedere alle risorse da un server più vicino, riducendo la latenza e migliorando le prestazioni.


---
### Funzionamento

1. L'Utente richiede al Browser una pagina
2. Il Browser chiede la risorsa nella cache:
	-> dell'host
	-> del Proxy Cache configurato

##### Cache Miss:
3. Se non trova la risorsa, la richiede al Server.
4. Il Server risponde normalmente con la risorsa, e la Cache ne mantiene in memoria una copia.

##### Cache Hit:
3. Se trova la risorsa, invia al Server un messaggio di "if-modified-since", un pacchetto con nell'intestazione un campo indicante data e ora in cui la Cache ha aggiornato la risorsa.
4. Il Server confronta la data di ultima modifica della risorsa con quella indicata nella richiesta:
	- se sono uguali (la risorsa in Cache è aggiornata) indica nella risposta il codice di stato 304 (Not Modified), senza inviare la risorsa.
	- se la risorsa risulta modificata, il server invia (prima alla cache, che salverà ed inoltrerà al Client) la risposta con un campo "Last-Modified" contenente data e ora dell'ultima modifica.

---
## Extra
#proxyServer
##### Proxy Server:
> Server che funge da intermediario per le richieste degli utenti da una rete all'altra. Quando un utente invia una richiesta a un server, la richiesta viene inoltrata dal server proxy al server di destinazione per l'elaborazione.

Un server proxy funge da intermediario tra l'utente e il server di destinazione, fornendo un livello aggiuntivo di controllo e sicurezza per le richieste di rete.
