# Capitolo 1

Un **sistema operativo** è un insieme di programmi che gestisce gli elementi fisici (hardware) di un calcolatore; fornisce una piattaforma ai programmi applicativi e agisce da intermediario fra l'utente e l'hardware del calcolatore.
é possibile considerare un sistem operativo come un **assegnatore di risorse** (tempo di cpu, spazio di memoria, dispositivi di I/O ecc.).

## 1.4 Struttura del sistema operativo

Fra le più importanti caratteristiche dei sistemi operativi vi è la **multiprogrammazione**: consente di aumentare la percentuale d' utilizzo della CPU, organizzando le attività computazionali (job) in modo tale da mantenerla in continua attività. I diversi job vegono inizialmente collocati sul disco in un area chiamata **job pool**, contenente tutti i processi in attesa di essere allocati nella memoria centrale. A un certo punto il job potrebbe trovarsi nell'attesa di qualche evento, come il completamento di un operazione di I/O. Il sistema passa semplicemente a un altro job e lo esegue, finchè c'è almeno un job da eseguire, la CPU non è mai inattiva.
