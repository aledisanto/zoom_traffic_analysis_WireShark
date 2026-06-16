# Inspection of Zoom Network Traffic using Wireshark

Questo repository contiene lo studio e l'analisi del traffico di rete generato dall'applicazione **Zoom**, sviluppato come progetto d'esame per il corso di **Advanced Internet Technologies** presso l'*Università degli Studi di Trieste*.

## Obiettivo del Progetto
Il lavoro si concentra sull'attività di **packet sniffing e network analysis** attraverso l'uso di **Wireshark**. L'obiettivo è mappare e comprendere il comportamento dei flussi di rete di Zoom durante le diverse fasi operative (apertura, autenticazione e comunicazione audio/video), identificando l'architettura dei server contattati e i protocolli di trasporto e sicurezza utilizzati.

## Metodologia e Fasi di Analisi della Rete
L'analisi del traffico acquisito (`.pcapng`) ha permesso di isolare tre fasi cruciali del ciclo di vita dell'applicazione:

1. **Fase di Avvio (App Opening):** Tracciamento della query DNS iniziale verso il Name Server di default per l'ottenimento dell'IP pubblico del server di destinazione (`us05web.zoom.us`), seguita dall'handshake TCP (SYN, SYN-ACK, ACK) sulla porta 443 e dall'invio del pacchetto *Client Hello* tramite TLS v1.3.
2. **Autenticazione:** Analisi dello scambio massivo di pacchetti diretti verso i server di terze parti (`accounts.google.com`) a seguito della scelta di autenticazione tramite credenziali Google.
3. **Comunicazione Audio/Video:** Studio dei flussi multimediali e identificazione dei nodi di rete. È stata rilevata la presenza costante di flussi UDP attivi anche in condizioni di microfono e telecamera disattivati.

## Protocolli e Server Identificati

### Protocolli di Rete Analizzati
* **DNS, TCP, UDP, TLS v1.3**.
* **DTLS-SRTP:** Identificazione dell'architettura di cifratura per i flussi multimediali in tempo reale. Nel README evidenziamo anche la risoluzione di un problema di *mislabeling* di Wireshark, che classificava erroneamente i pacchetti DTLS-SRTP come protocollo *WireGuard* sulla porta UDP 8801.

### Classificazione dei Server Zoom (Cloudflare & AWS)
Attraverso lo studio degli endpoint più frequentati e delle tecniche di geo-IP, sono stati mappati i seguenti nodi distribuiti:
* **Signaling Servers:** Gestione dell'avvio/chiusura dell'app e autenticazione (es. `170.114.52.5` - San Jose, CA).
* **Media Servers:** Smistamento di audio, video e screen sharing (es. IP `206.247.42.42` su porta UDP 8801).
* **CDN (Content Delivery Network):** Distribuzione dei contenuti statici del servizio (es. blocco IP `170.114.45.1 / 170.114.52.38`).

## Contenuto della Repository
* **[zoom_traffic_analysis.pptx](./zoom_traffic_analysis.pptx)**: La presentazione PowerPoint completa utilizzata in sede d'esame, contenente i grafici di I/O di Wireshark, tabelle di conversazione, filtri di visualizzazione applicati (`tls && ip.addr`) e dettagli sui server.
* **[zoom_traffic_analysis.pdf](./zoom_traffic_analysis.pdf)**: La presentazione PowerPoint completa (esportata in PDF) utilizzata in sede d'esame, contenente i grafici di I/O di Wireshark, tabelle di conversazione, filtri di visualizzazione applicati (`tls && ip.addr`) e dettagli sui server.
---
*Progetto realizzato da: Alessandro Di Santo*  
*Professore: Martino Trevisan*  
*Corso: Advanced Internet Technologies* 
*Dipartimento di Ingegneria e Architettura — Università degli Studi di Trieste*
