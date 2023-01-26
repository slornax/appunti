Hype Text Transfer Protocol

> Principale protocollo di comunicazione (richiesta/risposta dati) su internet;

> Opera tramite modello connectionless Client-Server:
> -> Il Client richiede dati ad un server (effettua una richiesta HTTP di una pagina HTML ad un indirizzo noto)
> -> Il Server elabora la richiesta, e risponde alla dupla (ip-socket) mittente con i dati richiesti (in questo caso la pagina HTML)
> -> Il Client riceve i dati richiesti e termina la connesione con il server


Il protocollo HTTP (Hypertext Transfer Protocol) è un protocollo di rete utilizzato per la comunicazione tra client e server nella Rete (inizialmente creato per la trasmissione di documenti ipertestuali, poii esteso ad altri tipi di contenuti).

Consiste in un protocollo Client-Server,  in cui l'host Client invia richieste a cui il Server invia la risposta.

---
### Richiesta 

Una richiesta HTTP è composta principalmente da tre campi:

- Una Linea di Richiesta: 
	-> Contiene informazioni riguardanti la richiesta (metodo HTTP, URL della risorsa richiesta, versione del protocollo)

- Una o più Linee di Intestazione:
	-> Header del protocollo, contiene informazioni agguntive sulla richiesta

- Eventuali linee di Corpo
	-> Payload della richiesta
`
```richiesta
POST /paginaSpecifica.html HTTP/1.1

Host: www.linkConosciuto.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 23

name=value&email=value@example.com
```

---
### Risposta

- Linea di stato
	-> Indica che risultato ha prodotto la richiesta sul Server (es. 200: "Ok");

- Intestazioni:
	-> informazioni aggiuntive sulla risposta (es. tipo di contenuto, data e ora);

- Corpo
	-> I dati della risposta (es. codice html della pagina richiesta);


 ```risposta
HTTP/1.1 200 OK
Content-Type: text/html
Date: Mon, 15 Jan 2018 12:00:00 GMT
Content-Length: 12345

<!DOCTYPE html>
< html>
  < head>
    < title>Example Page</title>
  < /head>
  < body>
    < h1>Welcome to my website!</h1>
  < /body>
< /html>
```

----
### Metodi
Il protocollo HTTP definisce diversi metodi per la richiesta delle risorse, tra cui:

-   **GET:** utilizzato per richiedere una risorsa. GET è il metodo più comunemente utilizzato per richiedere informazioni dal server.
-   **POST:** utilizzato per inviare i dati ad una risorsa. Questo metodo è utilizzato per inviare informazioni al server, come ad esempio i dati di un modulo HTML.
-   **PUT:** utilizzato per aggiornare una risorsa esistente.
-   **DELETE:** utilizzato per eliminare una risorsa esistente.
-   **HEAD:** utilizzato per richiedere solo le intestazioni della risposta, senza il corpo della risposta.
-   **OPTIONS:** utilizzato per richiedere le opzioni di comunicazione supportate dal server per una determinata risorsa.
-   **CONNECT:** utilizzato per stabilire una connessione sicura (ad esempio, per l'utilizzo di un tunnel SSL).
-   **TRACE:** utilizzato per richiedere un loopback della richiesta, per debug e test.
-   **PATCH:** utilizzato per apportare modifiche parziali ad una risorsa esistente.
---
### [[Cookie]]