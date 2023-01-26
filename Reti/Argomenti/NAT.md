	Network Address Translation

>Traduzione degli indirizzi di Rete

- Tecnica che consiste nel modificare gli indirizzi Ip contenuti negli Header dei Pacchetti in transito su un sistema che agisce da router all'interno di una comunicazione tra host posti su reti private diverse.

- Solitamente implementato da Router e Firewall;

- Sostituisce in un pacchetto l'indirizzo Privato dell'host mittente con l'indirizzo Pubblico, per poi ristabilirlo per i pacchetti di cui sarà destinatario.

-> Due host posti su due reti Private diverse, comunicanti tramite i rispettivi Gateway, saranno quindi convinti di scambiarsi pacchetti con host aventi come IP gli indirizzi pubblici associati ai Gateway.

---
### SNAT e DNAT
Source NAT e Destination Nat, utilizzati se l'[[IP#Header|Header]] del pacchetto viene modificato in ingresso o in uscita rispetto alla rete.

---
### IP Masquerading
	o Port Address Translation / NAT Dinamico

NAT in cui le connessioni generate da un insieme di Host vengono instradate nella rete Pubblica con un solo indirizzo IP: vengono modificati anche i valori delle porte di Trasporto per dare una mappatura dinamica diversa delle porte.

Questo metodo prevede di individuare una rete "interna" (che tipicamente utilizza indirizzi IP privati) ed una "esterna" (che tipicamente utilizza indirizzi IP pubblici), e permette di gestire solo connessioni che siano originate da host della rete "interna".