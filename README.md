# Simulatore DLX: sequenziale e pipelined

Simulatore visivo e interattivo del processore **DLX**, pensato per ripassare Calcolatori Elettronici T. Segue le slide del corso: clock per clock si vede cosa fa il datapath e, nel sequenziale, anche cosa succede sui bus della scheda.

Ogni pezzo dello schema si può **toccare**: compare una scheda con cos'è, a cosa serve e cosa sta facendo in quel clock.

## DLX sequenziale

- **CPU** (slide 07): bus interni S1, S2, Dest con tri-state; PC, IAR, TEMP, A, B, C, MDR, MAR, IR con i campi; register file; ALU; unità di controllo con i LED INT e IEN; interfaccia bus con mux e demux di byte.
- **Scheda**: BA31..2, BE3..0, MEMRD, MEMWR, READY, BD in 4 corsie da 8 bit; decoder di 1° livello (CS_EPROM, CS_RAM, CS_INPUT); EPROM e RAM disegnate come 4 banchi da 8 bit; porte di input con buffer tri-state. I bus non pilotati sono in alta impedenza (grigio tratteggiato).
- Ogni passo è uno **stato del controller** (FETCH 1, FETCH 2, DECODE, EXECUTE, MEMORY, WRITE BACK, BRANCH, INTERRUPT, RFE) con la sua riga RTL.
- Programmi: porte → RAM (LH, SW); corsie di byte (LB, SB); ALU e branch (ADDI, ADD, BEQZ); **interrupt e RFE** (IAR ← PC, PC ← 0, IEN ← 0, handler a 0, ritorno con PC ← IAR, IEN ← 1).
- Opzioni: memoria lenta (READY = 0, stati di wait), SWAP dei tri-state, valori delle porte.
- **Quiz** a due livelli: *punti chiave* (chip select, BE, indirizzo in MAR, estensione del segno, salto preso, interrupt, RFE) oppure *ogni clock* (anche la micro-operazione successiva).

## DLX pipelined

- **Datapath** (slide 08): IF, ID, EX, MEM, WB; registri di pipeline IF/ID, ID/EX, EX/MEM, MEM/WB con i valori scritti a fine clock; IM e DM separate; forwarding unit; rilevazione alee e STALL; micro-operazione di ogni stadio.
- Interruttori **forwarding unit** e **split-cycle**; per ogni programma mostra gli stalli nei **tre casi d'esame** (senza FU né CS, con FU, con solo CS), clock totali e CPI = 1 + cicli persi / istruzioni.
- Programmi: caso ideale, esercizi d'esame (ADD-LW-SUB, esercizio a tre casi), load-use (slide 34), store dopo ALU, branch con predict not taken e flush (slide 38), **delayed load** prima e dopo il riordino (slide 35), **delayed branch** con 3 delay slot (slide 40).
- **Il tuo programma**: editor dove scrivere qualsiasi esercizio (etichette, `0x1F` o `1Fh`, `offset(Rn)`, branch a etichetta). Il programma resta salvato nel browser.
- **Compila tu il diagramma**: griglia del diagramma temporale da riempire (IF, ID, EX, MEM, WB, S) con correzione cella per cella e soluzione.
- **Quiz**: cicli persi, da dove arriva il dato col forwarding, stalli, bypass e split-cycle, flush, delay slot.
- **BTB a 1 bit contro 2 bit** su due cicli annidati, con previsioni ed errori giro per giro.

## Confronto

Tabella sequenziale contro pipelined e clock necessari per gli stessi programmi (stati del simulatore e tabella delle slide).

## Come usarlo

È un unico file HTML: scaricalo e aprilo nel browser, funziona anche offline (senza connessione cambia solo il font). Per metterlo online: *Settings → Pages → Deploy from a branch → `main` / root*.

Note: opcode del DLX di Hennessy-Patterson; decodifica degli indirizzi semplificata. Nel pipelined i registri partono da Ri = i·10h e la DM restituisce D000xxxxh.
