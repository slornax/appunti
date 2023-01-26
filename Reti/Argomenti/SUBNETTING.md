>Processo di  suddivisione di una rete in sottoreti più piccole

Tecnica attraverso la quale si decide di dedicare alla rete alcuni bit inizialmente di host.
Consente di organizzare la rete in maniera da limitarne la congestione, milgiorarne la sicurezza e gestire in maniera più efficiente gli indirizzi IP.

Per effettuare il subnetting di una rete, si considera il prefisso di rete (bit identificanti la rete a cui appartiene un dispositivo) e si "estende" includendo un numero di bit di host abbastanza grande da contenere il numero di host che dobbiamo includervi (nella sottorete).

1. Considero il numero di Subnet in cui voglio dividere la rete;
2. Considero il numero di host che voglio collegare alle varie Subnet;
3. Prendendo la Subnet con più host, espando l'indirizzo di rete con *n* dei bit di host (2^*n* >= reti)
4. Partendo dalla Subnet con il maggior numero di host e proseguendo in ordine decrescente, calcolo gli indirizzi di rete (bit di host pari a 0) e di broadcast (pai a 1) delle sottoreti create.

### Esempio

Date 3 lan (L1, L2, L3) ed un indirizzo 165.5.1.0/24, creare 3 sottoreti con pari numero di Host collegabili

- ci sono 24 bit di rete = si lavora sull'ultimo ottetto (bit di host)
- le reti dovranno avere pari host = dividendo le reti in esponenziali di due, mi servono 2^2 reti = verranno utilizzati 2 dei bit di host per espandere la rete
- l'ultimo ottetto verrà diviso quindi in:
	1. . 0 0 0 0 0 0 0 0 / . 0 0 1 1 1 1 1 1      ->    165.5.1.0 / 26
	2. . 0 1 0 0 0 0 0 0 / . 0 1 1 1 1 1 1 1       ->    165.5.1.64 / 26
	3. . 1 0 0 0 0 0 0 0 / . 1 0 1 1 1 1 1 1       ->    165.5.1.128 / 26
	4. . 1 1 0 0 0 0 0 0 / . 1 1 1 1 1 1 1 1        ->    165.5.1.192 / 26
- i primi tre range di indirizzi saranno le reti L1-2-3, mentre il quarto saranno indirizzi lasciati liberi.
