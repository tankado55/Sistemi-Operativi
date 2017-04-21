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

# Capitolo 2: Strutture dei sistemi operativi

## 2.1 Servizi di un sistema operativo

Un sistema operativo offre un ambiente in cui eseguire i programmi e fornire servizi.

Un primo gruppo di servizi offre funzionalità all'utente:
* **interfaccia con l'utente**
* **esecuzione di un programma**: Il sistema deve poter caricare un programma in memoria ed eseguirlo.
* **Operazioni di I/O**
* **Gestione del file system**
* **Comunicazioni**: in molti casi un processo ha bisogno di scambiare informazioni con un alto processo, realizzabile attraverso memoria condivisa o scambio di messaggi.
* **Rilevamento di errori**

Un secondo gruppo di funzioni del sistema operativo assicura il funzionamento efficiente del sistema stesso:
* **Allocazione delle risorse**: se sono attivi più utenti (o più processi) il sistema provvede all'assegnazione delle risorse necessarie a ciascuno di essi (Es. procedure di scheduling della CPU).
* **Accounting dell'uso delle risorse**: Vogliamo mantenere traccia di quali utenti usano il calcolatore e quante risorse impiegano, serve per addebitare il costo agli utenti o per redigere statistiche.
* **Protezione e sicurezza**: Quando più processi sono in esecuzione concorrente essi non devono influenzarsi o interferire con il sistema operativo.

to be continued...

# Capitolo 3: Processi

Un processo è un entità attiva, con un contatore di programma e un insieme di risorse associate.

### 3.1.2 Stato del processo

Un processo può trovarsi un uno dei seguenti stati:
* **Nuovo**
* **Esecuzione**
* **Attesa**: Il processo attende I/O o un segnale.
* **Pronto**: Il processo attende di essere associato ad un unità di elaborazione.
* **Terminato**

### 3.1.3 Blocco di controllo dei processi

Ogni processo è rappresentato nel sistema da un **blocco di controllo**.

Un **PCB** contiene molte informazioni connesse a un processo specifico:
* **Stato del processo**
* **Contatore di programma**: contiene l'indirizzo della successiva istruzione da eseguire per tale processo.
* **Registri della CPU**
Quando si verifica un'interruzione della CPU si devono salvare tutte queste informazioni, in modo da permettere la correttaesecuzione del processo in un momento successivo.
* **Informazioni sullo scheduling**: priorità ecc.
* **Informazioni sulla gestione della memoria**
* **Informazioni di acocunting**: Quota d'uso, tempo di utilizzo della CPU ecc.
* **Informazioni sullo stato dell'I/O**: lista dei dispositivi assegnati a un processo, elenco dei file aperti ecc.

### 3.2.1 Code di scheduling

Ogni processo è inserito in una **coda di processi**.
Un nuovo processo si colloca inizialmente nella ready queue, dove attende finchè non è selezionato per essere eseguito. Una volta che il processo è in fase di esecuzione, si può verificare uno dei seguenti eventi:
* Il processo può emettere una richiesta di I/Oe quindi essere inserito in una coda di I/O;
* Il processo può creare un nuovo processo figlio e attendere la terminazione;
* Il processo può essere rimosso forzatamente dalla CPU ed essere reinserito nella coda dei processi pronti.

### 3.2.2 Scheduler

Il sistema operativo, incaricato di selezionare i processi dalle suddette code, compie la selezione per mezzo di uno **scheduler**:
* Lo **scheduler a lungo termine** sceglie i processi da caricare in memoria affinchè vengano eseguiti.
* Lo **scheduler a breve termine** fa la selezione tra i processi pronti per l'esecuzione e assegna la CPU a uno di loro.

Lo scheduler a breve termine seleziona frequentemete un nuovo processo per la CPU mentre quello a lungo termine si esegue con una frequenza molto inferiore.

é fondamentale che lo scheduler a lungo termine selezioni una buona combinazione di processi I/O bound e CPU bound.

In alcuni sistemi operativi come quelli in time-sharig, si può introdurre uno **scheduler a medio termine**, in alcuni casi può essere vantaggioso rimuovere processi dalla memoria e reintrodurlo in seguito, questo sistema si chiama avvicendamento dei processi in memoria (**swapping**). Può servie a migliorare la combinazione dei processi o per liberare memoria.

### 3.2.3 cambio di contesto

Il passaggio della CPU a un nuovo processo implica il salvataggio dello stato del processo attuale e il ripristino dello stato del nuovo processo. Questa procedura si chiama **constext switch**: Il sistema salva il contesto del processo uscente nel suo **PCB** e carica il contesto del processo  che subentra.




# Capitolo 5: Sincronizzazione dei processi

