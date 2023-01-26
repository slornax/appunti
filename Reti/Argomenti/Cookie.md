### Cos'è 
>Meccanismo Server di riconoscimento del Client

Un file cookie è un piccolo **file di testo che un server web memorizza sul dispositivo dell'utente** quando si accede a un sito web. 

```cookie_12345
session_id=12345; expires=Thu, 18 Jan 2023 12:00:00 GMT; path=/
user_id=67890; expires=Thu, 18 Jan 2023 12:00:00 GMT; path=/
preferences=dark_mode=on; expires=Thu, 18 Jan 2023 12:00:00 GMT; path=/
```

---
### Cosa fa
> Crea un livello di sessione utente al di sopra dell' [[HTTP]]

Permette ad un server di sapere se ha precedentemente interagito con un host
	-> essendo il protocollo HTTP stateless (privo di stato), da solo non permette l'identificazione dell'utente in uno scambio di Richiesta-Risposta.

---
### Da cosa è costituito
- Sono piccoli file di testo salvati sull'host Client al momento della navigazione su un sito. 

- Il server web può mantenere una copia dei cookie su un database, a seconda della configurazione del sito web e dell'architettura del sistema.

### Componenti:
- Campo "set" nella Risposta HTTP contenente il codice del file
- Campo "cookie" nelle successive Richieste HTTP
- File gestito dal browser (mantenuto nel client)
- DB sul Server

### Tipologie
-> di Sessione
	--> Vengono eliminati dal browser quando l'utente chiude lo chiude

-> Permanenti
	--> Rimangono sul dispositivo dell'utente per un periodo di tempo specificato dal sito web.

---
### Come funziona

Quando un utente visita un sito web per la prima volta, il server invia un Cookie al browser dell'utente; il browser memorizza il cookie e lo invia al server ad ogni richiesta successiva.

1. Il client interagisce con il server (mandando ad esempio la richiesta di una pagina HMTL)
2. Il Server crea un codice identificativo per l'utente (es. 1234), e risponde alla richiesta con una risposta HTTP contenente nell' Header la riga di intestazione "set-cookie:1234"
3. Il Browser del Client registra il Cookie assocciato al sito
4. Ad ogni nuova iterazione, il Client invierà insieme alla richiesta il codice del cookie, per permettere al server di riconoscerlo.
5. Il server ad ogni richiesta ricevuta controlla il valore di un eventuale cookie. se esiste, riconosce l'utente che ha mandato la richiesta e si comporta di conseguenza.

![[Pasted image 20230118112019.png]]

```response
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: session_id=12345; expires=Thu, 18 Jan 2023 12:00:00 GMT; path=/

<html>
...
</html>
```

```request+cookie
GET /index.html HTTP/1.1
Host: www.example.com
Cookie: session_id=12345; user_id=67890
```
