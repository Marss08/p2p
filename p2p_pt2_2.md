TCP

self-clocking: si regola in base agli ACK.
`cwnd`, `MSS`, `RTT`.
slow start, (cwnd >=) ssthresh: congestion avoidance.
evento di perdita: timeout, 3 ACK duplicati.
3 ACK duplicati: Fast Retransmit,
    e Fast Recovery (da Reno in poi).
Equità tra flussi.

Varianti: Tahoe, Reno, New Reno, Vegas, Westwood(+), SACK, Veno.

`RTO`: Retransmission Time-Out
SampleRTT, media mobile =: EstimatedRTT
Karn/Partridge: exponential backout: Time-Out = 2* EstimatedRTT.
Jackbson/Karels: deviazione
    diff = SampleRTT - EstimatedRTT
    Deviation = Deviation + beta * ( |diff| - Deviation )
    Time-Out = EstimatedRTT + 4* Deviation

Invece, se timeout accade,
    ssthresh := flight_size / 2,
    cwnd := 1.


Tahoe
Fast Retransmit: cwnd := 1.
no Fast Recovery.
semplice e conservativo.


Reno
algoritmo di Nagle (big packet optimization)
3 ACK duplicati -> Fast Recovery:
ssthresh dimezzata (`AIMD`), cwnd := ssthresh + #dup_acks
-> inflation di cwnd.

new ACK ( = non duplicato) -> uscita da Fast Recovery, into Congestion Avoidance.
ma se timer scade in Fast Recovery -> evento tragico = slow start.

non fa bene se perdite multiple e dense.


New Reno
rispetto a Reno:
in Fast Recovery:
Partial ACK, for outstanding packets, deflating cwnd.
timer: impatient, or slow & steady.


Vegas
baseRTT vs (current) RTT
thruput atteso (ottimo) vs attuale
diff < alfa: code vuote -> Additive Increase
alfa < diff < beta: ottimo -> no Increase.
diff > beta: Additive Decrease.
-> AIAD.
1 dup ACK, e timeout, triggera Fast Retransmit.
Rimane in Fast Retransmit 'ricorsVM'.

problema del route change. soluzione...

problema del probing falsato di SampleRTT.
soluzione: RED

difetto: too friendly.


SACK
BWE: BandWidth Estimation
or
RE: Rate Estimation

b_k = d_k / (t_k - t_k-1)
^ questo è un campione/sample di banda.

ora, BE sarebbe la media aritmetica tra i vari b_k.
tuttavia, lo è in maniera esponenziale.
Nota bene: guarda la legge di aggiornaM = è effettVM quella di una media aritmetica, ponderata. (è solo che in forma di legge di aggiornaM sembra qualcosa di diverso)

BE_k = alpha_k BE_k-1 + (1-alpha_k)* (b_k - b_k-1)/2

a_k dipende anch'esso da [...], non è costante appunto.

loss event:
𝑠𝑠𝑡ℎ𝑟𝑒𝑠ℎ = (𝐵𝐸 ∗ 𝑅𝑇𝑇min)/𝑠𝑒𝑔𝑠𝑖𝑧𝑒
^ questa legge dovrebbe essere proprio quella che distingue i 2 casi di loss event.

fairness, e Reno-friendli-ness.



TCP Westwood+
SempliceM aggiorna la BWE a ogni RTT,
e ciò migliorerebbe la stima.

^ actually adottato da Linux 2.6 in poi (non so se ancora oggi).



TCP Veno
Reno, con qualche aggiustaM Vegas-like.
N sia la stima del #pacchetti presenti nelle code di rete:
𝑁 = 𝑐𝑤𝑛𝑑 ∙ (1 − 𝑅𝑇𝑇𝑚𝑖𝑛/𝑅𝑇𝑇 )

/Beta è una costante speriML.
se si verifica una perdita va così interpretata:
se N < /Beta: perdita casuale (non congestV),
se N >= /Beta: perdita congestiva

in ogni caso,
se N < /Beta, la banda è sottoutilizzata, e in regione di congestion avoidance, si comporta come Reno = crescita lineare della cwnd.

se N >= /Beta, la banda è ben utilizzata, e la finestra cresce alla metà del tempo.

Nel caso di perdita:
se N >= /Beta, uguale a Reno: dimezza cwnd e ssthresh, ma
se N < /Beta, è sempre una legge AIMD, ma
    ssthresh := 4/5 cwnd
(e cwnd := ssthresh anche in questo caso)
