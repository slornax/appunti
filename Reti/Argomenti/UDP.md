	User Datagram Protocol

>Protocollo di Trasporto Connectionless, più leggero e veloce

**+** : 
- Veloce 
- Leggero

**-** :
- Non fornisce alcun controllo (se non l'integrità del segmento con il Checksum)
- Inaffidabile 
	-> non si preoccupa della ritrasmissione dei segmenti persi

Principalmente utilizzato nei servizi con comunicazione real-time, che ammettono perdite di pacchetti.

```
Generally, real-time connections like video straming, VoIP, and some games will use UDP, as it is used where real-time quick communication is crucial, and losing a few frames/packet in the process is acceptable.

Non-real time communicaton most often use TCP as it is well stablished, provides packet ordering, retransmission, and prevent packet loss. TCP is used where transferring every frame/packet is important.
Video Streaming Services use TCP and simply buffer a few seconds of content, instahead of using UDP since the delay is not crucial and TCP transfers can be easly accomplished over HTTP and web browsers without the need for additional plugins and software.

Generally, if the content will be made available later, it is most likely using TCP.
Live TV streams and multicast video conferencing, on the other hand are usually over UDP. Such applications usually require their own protocol on top of UDP (like RTP/RTCP)

```

## Header

|Source Port|Destination Port|
|---|---|
|Length|Checksum|

