# Simulatore DLX: CPU, bus e periferiche

Simulatore visivo e interattivo di un sistema basato sul processore **DLX** (sequenziale), pensato per ripassare Calcolatori Elettronici. Mostra, una fase alla volta (IF, ID, EX, MEM, WB), cosa succede nel datapath della CPU e, nello stesso istante, sui bus della scheda.

## Cosa si vede

- **CPU**: PC, IR, register file (R0, R20, R21, R22), registri A e B, immediato esteso, ALU, LMD e mux indirizzi (PC nel fetch, ALU nella fase MEM).
- **Bus**: BA[31..0], BE[3..0], MEMRD, MEMWR e BD[31..0] diviso in 4 corsie da 8 bit. I bus non pilotati sono in **alta impedenza** (grigio tratteggiato).
- **Decoder di 1° livello** con i chip select CS_EPROM, CS_RAM, CS_INPUT.
- **EPROM** (00000000h) con il programma, **RAM** (FFFF0000h) mostrata byte per byte, **porte di input** (20000000h) collegate al bus tramite buffer tri-state abilitati da CS · MEMRD · BE.

## Funzioni

- Avanza / Indietro / Reset, frecce ← → da tastiera, modalità automatica a tre velocità.
- **Modalità quiz**: prima dei passi importanti chiede di prevedere BE, indirizzi e valori finali, con punteggio.
- Due programmi demo:
  1. `LHI` + `LH` + `LHI` + `SW`: halfword signed dalle porte e word in RAM.
  2. `LHI` + `LB` + `LHI` + `SB`: corsie di byte, un solo tri-state, byte spostato sulla corsia alta.
- **SWAP** dei tri-state e valori delle porte modificabili, per vedere l'effetto sull'estensione del segno.
- Riga **RTL** di ogni fase, timeline cliccabile, spiegazione di ogni blocco con un tocco.

## Come usarlo

È un unico file HTML: aprilo nel browser. Per pubblicarlo online: *Settings → Pages → Deploy from a branch → `main` / root*.

Note: opcode del DLX di Hennessy-Patterson; decodifica degli indirizzi semplificata.
