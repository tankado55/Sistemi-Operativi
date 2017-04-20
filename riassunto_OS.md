# Capitolo 1

Un **sistema operativo** è un insieme di programmi che gestisce gli elementi fisici (hardware) di un calcolatore; fornisce una piattaforma ai programmi applicativi e agisce da intermediario fra l'utente e l'hardware del calcolatore.
é possibile considerare un sistem operativo come un **assegnatore di risorse** (tempo di cpu, spazio di memoria, dispositivi di I/O ecc.).

## 1.4 Struttura del sistema operativo

Fra le più importanti caratteristiche dei sistemi operativi vi è la **multiprogrammazione**: consente di aumentare la percentuale d' utilizzo della CPU, organizzando le attività computazionali (job) in modo tale da mantenerla in continua attività.
I diversi job vegono inizialmente collocati sul disco in un area chiamata **job pool**, contenente tutti i processi in attesa di essere allocati nella memoria centrale.
Il sistema operativo ne sceglie uno e inizia l'esecuzione.
A un certo punto il job potrebbe trovarsi nell'attesa di qualche evento, come il completamento di un operazione di I/O. Il sistema passa semplicemente a un altro job e lo esegue, finchè c'è almeno un job da eseguire, la CPU non è mai inattiva.

Un **sistema di calcolo interattivo** permette la comunicazione diretta tra utente e sistema.

Un sistema operativo time-sharing permette a più utenti di condividere contemporaneamente il calcolatore, per assicurare a ogni utente una piccola frazione del tempo di calcolo il sistema si avvale dello **scheduling** e della **multiprogrammazione** 

## Duplice modalità di funzionamento
Per garantire il corretto funzionamento è necessario distinguere tra l'esecuzione di codice del sistema operativo e di codice utente.
Sono necessarie almeno due diverse modalità: **modalità utente** e **modalità di sistema**. Per indicare quale sia la modalità attiva, l'architettura della CPU deve essere dotata di un bit, chiamato **bit di modalità**:kernel(0) o user(1).
La duplice modalità di funzionamento consente la protezione del sistema operativo e degli altri utenti dagli errori di un utente.

## Sistemi embedded real-time

I sistemi embedded funzionano quasi sempre con **sistemi operativi real-time**. Essi si utilizzano quando sono stati imposti rigidi vincoli di tempo alle funzioni del processore o al flusso dei dati. L'elaborazione deve avvenire entro i limiti prestabiliti.

# Capitolo 2 Strutture dei sistemi operativi

## 2.1 Servizi di un sistema operativo

Un sistema operativo offre un ambiente in cui eseguire i programmi e fornire servizi.

Un primo gruppo di servizi offre funzionalità all'utente:
* **interfaccia con l'utente**
* **esecuzione di un programma**: Il sistema deve poter caricare un programma in memoria ed eseguirlo.
* **Operazioni di I/O**
* **Gestione del file system**
* **Comunicazioni**: in molti casi un processo ha bisogno di scambiare informazioni con un alto processo, realizzabile attraverso memoria condivisa o scambio di messaggi.
* **Rilevamento di errori

