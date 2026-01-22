CSMA/CA
carrier sense fisico, e virtuale (NAV: Network Allocation Vector).

Aloha
Slotted Aloha (GSM RACH)

WiFi
BSS, ESS, IBSS (Independent BSS)

cronologicamente le versioni sono:
b  1999 11Mbps
g  2003 54M
n  2009 600   (WiFi 4)
ac 2013 3.47Gbps (WiFi 5)
ax 2020 14    (WiFi 6)

e af e ah che sono usate per le lunghe distanze, in bande di frequenza diverse.
af 2014 ~40-550 Mb
ah 2017 ~350 Mb


DCF
DIFS
backoff
ACK SIFS

MACAW
RTS/CTS (Clear To Send), DS
PIFS PCF mode
EIFS error
RIFS burst


802.11n, compatibilità
Legacy Mode,
Mixed Mode (CTS-to-self)
Greenfield Mode

PCF: Point Coordination Function
Contention-Free Period, in cui opera
vs Contention Period, in cui è off
ScarsaM adottato (rispetto alla sola DCF).

IBSS:
senza AP,
sincronizzazione distribuita, tramite TSF (Time Sync Function) [...]


Routing nelle reti wireless
reti cablate: Distance Vector, e Link State algorithms.
In MANET, Mobile Ad-hoc NETworks:
protocolli proattivi (eager), reattivi (lazy), ibridi.


Mobile IP:
MN: Mobile Node
HA: Home Agent
FA: Foreign Agent
CN: Correspondent Node (il nodo con cui MN parla, e che non sa niente che MN sia mobile)



Protocolli Reattivi (lazy)

DSR: Dynamic Source Routing
per reti senza infrastruttura.
RREQ: Route REQuest
RREP: Route REPly
non scala bene oltre 5-10 hops.


AODV: Ad-hoc On-demand Distance Vector
routing on node basis.
Destination Sequence Number (novità dell'informazione).
Mobilità peggiora prestazioni (sensibly enuf).


LAR: Location-Aided Routing
usa il GPS ~ distanza euclidea.


LRA: Link-Reversal Algorithm
reti di sensori, verso un sink.
DAG che si rompe, backtracking 'distibuito' per ripristinare i collegaM da DAG.


TORA: Temporally-Ordered Routing Algorithm
prioritarizza up-time/stabilità dei collegaM, non minimo #hops.
AltriM, come LRA.


Power-Aware Routing:
DSR ma battery-friendly.




Protocolli proattivi (eager)


DSDV: Destination Sequenced Distance Vector
basato su DV (il nome dell'algoritmo di Bellman-Ford)
sequence number pari ( = stabili ) vs dispari ( = instabili ).
full update vs incremental update.
scala male, drains battery (no sleep state), ma 'real-time'.


CGSR: Clusterhead Gateway Switch Routing
tipica astrazione: Cluster, comandato da un nodo Clusterhead,
i Gateway sono i nodi di confine, di raccordo tra 2 o più Clusters.
Dentro un cluster, DSDV.


OLSR: Optimized Link State Routing
backbone formata da `Multipoint Relay [node]s`.




Protocolli ibridi


ZRP: Zone Routing Protocol
parametro d, di neighborhood/località.
Inter-zone routing, proattivo (eager): DSDV-like.
vs
Intra-zone routing, reattivo (lazy).
ipotesi di località tra nodi del data transfer.





DTN: Delay Tolerant Networks
tollerano ritardi anche di ore e giorni.
reti di sensori.
i collegaM possono non esistere (ma si spera che si (ri)stabiliscano eventually).
Bundle Layer sopra/accanto ad IP.
Routing Opportunistico
    [praticamente conoscenza on a hop-to-hop basis].
metriche varie... sociali, somiglianza alla destinazione, storia (di cooperazione), gerarchia, clusters, ... flooding.

Epidemic Routing:
flooding.
ma TTL, memoria limitata dei nodi.


Spray and Wait:
come Epidemic Routing ma flooding limitato
a fase di Spray, e a nodi "spraying".
ad albero (binario), i nodi spraying aumentano in numero.
quelli che hanno già sparato, rimarranno waiting.


ProPHET: Probabilistic Routing Protocol using History of Encounters and Transitivity
metrica: la probabilità di incontro tra nodi:
`Delivery Predictability`.

dati A e B nodi che stabiliscono una connessione diretta:
P(A,B) è aggiornata positVM per ogni incontro,
ma è sottoposta anche ad aging.

La probabilità di incontro è transitiva:
si usa la probabilità composta per P(A,C).


PrivHab+
ProPHET non rispetta la privacy: A chiede a B qual è P(B,C), per calcolare P(A,C).
-> cifratura omomorfica.
ellisse, ma squadrato (per computational reasons di cifratura).
habitat di un nodo destinazione.


