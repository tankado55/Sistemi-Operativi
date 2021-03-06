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

## 2.2 Interfaccia con l'utente del sistema operativo

Vi sono 2 modi fondamentali per gli utenti di comunicare con il sistema operativo:
* Interfaccia a riga di comando, che può essere implementata in 2 modi: nel primo, lo stesso interprete dei comandi contiene il codice per l'esecuzione del comado, quindi il numero dei comandi che si possono impartire determina le dimensioni dell'interprete. Nel secondo metodo (UNIX), implementa la maggior parte dei comandi per mezzo di programmi di sistema, in questo modo i programmatori possono aggiungere nuovi comandi al sistema.
* Interfaccia grafica.

## 2.3 Chiamate di sistema

Le **chiamate di sistema** costituiscono un interfaccia per i servizi resi disponibili dal sistema operativo.

La maggior parte dei programmatori usano in genere un **API** che fornisce delle funzioni che invocano le chiamate di sistema per conto del programmatore,
le più diffuse sono: le API di widows, la API posix (UNIX, Linux e Mac OS X) e la API java per le
applicazioni eseguite dalla JVM.

## 2.4 Categorie di chiamate di sistema

Le chiamate di sistema sono classificabili in 6 categorie principali:

1. **Controllo dei processi**: un programma in esecuzione deve potersi fermare, sia in modo anomalo che in modo normale (`abort()`,`end()`), un processo che
esegue un programma può richiedere di caricare (`load()`) ed eseguire (`exec()`) un altro programma, inoltre è necessario poter mantenerne il controllo impostando degli attributi, potrebbe essere necessario terminare un programma (`terminate()`), molto spesso due processi possono condividere dati, spesso i sistemi operativi offrono chiamate che consentono di bloccare (`acquire_lock()`) i dati condivisi ecc.

2. **Gestione dei file**: `create(), delete(), open(),close()`

3. **Gestione dei dispositivi**: `request(), release()` ecc.

4. **Gestione delle informazioni**: Molte delle chiamate di sistema hanno semplicemente lo scopo di trasferire le informazioni tra il programma utente e il sistema operativo, ad esempio: `time()` per ottenere la data.

5. **Comunicazione**: chiamate per consentire la comunicazione dei processi.

6. **Protezione**: Es: `set_permission(), get_permissio()`

## 2.7 Struttura del sistema operativo

### 2.7.1 Struttura semplice

Molti sistemi operativi non hanno una struttura ben definita, come ad esmpio MS-DOS dove non vi è una netta separazione fra le interfacce e i livelli di funzionalità,
Questa struttura monolitica rendeva difficile l'implementazione e la manutenzione, ma offriva comunque un vantaggio in termini di prestazioni.

### 2.7.2 Metodo stratificato

In presenza di hardware appropriato, i sistemi operativi possono essere suddivisi in moduli più piccoli e gestibili, ciò permette al sistema operativo di mantenere un contollo molto più stretto sul calcolatore e sulle applicazioni che lo utilizzano. Vi sono molti modi per rendere modulare un sistema operativo. Uno di essi
è il **metodo stratificato**, secondo il quale il sistema è suddiviso in un certo numero di livelli o strati: il più basso corrisponde all'hardware e il più alto all'interfaccia con l'utente. Un tipico strato è composto da strutture dati e da un insieme di routine richiamabili dagli strati di livello più alto.

Il vantaggio principale offerto da questo metodo è dato dalla semplicità di progettazione e di debugging.
Ogni stato si realizza impiegando unicamente le operazioni messe a disposizione dagli strati inferiori, di conseguenza, ogni strato nasconde a quelli superiori l'esistenza di determinate struttre dati, operazioni e hardware.

La principale difficoltà risiede nella definizione appropriata dei diversi strati, inoltre tende ad essere meno efficiente delle altre.

### 2.7.3 Microkernel

Seguendo questo orientamento si progetta il sistema operativo rimuovendo dal kernel tutti i componenti non essenziali, realizzandoli come programmi di livello utente o
di sistema.In generale un microkernel offre servizi minimi di gestione dei processi, della memoria e di comunicazione.
Lo scopo principale del microkernel è fornire funzioni di comunicazione tra i processi client e i vari servizi, anch'essi in esecuzione nello spazio utente, la comunicazione viene realizzata mediante scambio di messaggi con il microkernel.

Uo dei vantaggi del microkernel è la facilità di estensione del sistema operativo, inoltre poichè è ridotto all'essenziale, se il kernel deve essere modificato, i
cambiamenti da apportare sono ridotti, e il sistema operativo risultante è più semplice da portare su diverse architetture. Inoltre offre maggiori garanzie di sicurezza e affidabilità, poichè i servizi si eseguono in gran parte come processi utente, e non come processi del kernel.

Purtroppo i microkernel possono incorrere in cali di prestazioni dovuti al sovraccarico indotto dall'esecuzione dei processi di sistema in modalità utente.

### 2.7.4 Moduli

Il miglior approccio attualmente disponibile per la progettazione dei sistemi operativi si basa sull'utilizzo di **moduli del kernel caricabili dinamicamente**.
L'idea di base è che il kernel deve fornire direttamente i servizi principali, mentre gli altri servizi sono implementati in modo dinamico, quando il kernel è in esecuzione. è più flessibile di un sistema a strati, perchè ogni modulo può chiamare qualsiasi altro modulo. (usato in Linux, Mac os X e Windows)





# Capitolo 3: Processi

Un processo è un entità attiva, con un contatore di programma e un insieme di risorse associate.

### 3.1.2 Stato del processo

Un processo può trovarsi un uno dei seguenti stati:
* **Nuovo**
* **Esecuzione**
* **Attesa**: Il processo attende I/O o un segnale.
* **Pronto**: Il processo attende di essere associato ad un unità di elaborazione.
* **Terminato**

### 3.1.3 Blocco di controllo dei processi (PCB)

Ogni processo è rappresentato nel sistema da un **blocco di controllo**.

Un **PCB** contiene molte informazioni connesse a un processo specifico:
* **Stato del processo**
* **Contatore di programma**: contiene l'indirizzo della successiva istruzione da eseguire per tale processo.
* **Registri della CPU**
Quando si verifica un'interruzione della CPU si devono salvare tutte queste informazioni, in modo da permettere la correttaesecuzione del processo in un momento successivo.
* **Informazioni sullo scheduling**: priorità ecc.
* **Informazioni sulla gestione della memoria**
* **Informazioni di accounting**: Quota d'uso, tempo di utilizzo della CPU ecc.
* **Informazioni sullo stato dell'I/O**: lista dei dispositivi assegnati a un processo, elenco dei file aperti ecc.

### 3.2.1 Code di scheduling

Ogni processo è inserito in una **coda di processi**.
Un nuovo processo si colloca inizialmente nella ready queue, dove attende finchè non è selezionato per essere eseguito. Una volta che il processo è in fase di esecuzione, si può verificare uno dei seguenti eventi:
* Il processo può emettere una richiesta di I/O e quindi essere inserito in una coda di I/O;
* Il processo può creare un nuovo processo figlio e attendere la terminazione;
* Il processo può essere rimosso forzatamente dalla CPU ed essere reinserito nella coda dei processi pronti.

### 3.2.2 Scheduler

Il sistema operativo, incaricato di selezionare i processi dalle suddette code, compie la selezione per mezzo di uno **scheduler**:
* Lo **scheduler a lungo termine** sceglie i processi da caricare in memoria affinchè vengano eseguiti.
* Lo **scheduler a breve termine** fa la selezione tra i processi pronti per l'esecuzione e assegna la CPU a uno di loro.

Lo scheduler a breve termine seleziona frequentemete un nuovo processo per la CPU mentre quello a lungo termine si esegue con una frequenza molto inferiore.

é fondamentale che lo scheduler a lungo termine selezioni una buona combinazione di processi I/O bound e CPU bound.

In alcuni sistemi operativi come quelli in time-sharig, si può introdurre uno **scheduler a medio termine**, in alcuni casi può essere vantaggioso rimuovere processi dalla memoria e reintrodurli in seguito, questo sistema si chiama avvicendamento dei processi in memoria (**swapping**). Può servie a migliorare la combinazione dei processi o per liberare memoria.

### 3.2.3 cambio di contesto

Il passaggio della CPU a un nuovo processo implica il salvataggio dello stato del processo attuale e il ripristino dello stato del nuovo processo. Questa procedura si chiama **context switch**: Il sistema salva il contesto del processo uscente nel suo **PCB** e carica il contesto del processo  che subentra.

### 3.3.1 Creazione di un processo

Durante la propria esecuzione, un processo può creare numerosi altri processi (processo padre e processo figlio) formando un **albero** di processi.
La maggior parte dei sistemi operativi identifica un processo per mezzo di un numero univoco, detto **pid**.

In Linux il processo "init" svolge il ruolo di processo padre di tutti i processi utente.

Un processo figlio può essere in grado di ottenere le proprie risorse direttamente dal sistema operativo o può essere vincolato a un sottoinsieme delle risorse del processo padre.

Per quanto riguarda l'esecuzione ci sono 2 possibilità:
1. Il processo genitore continua l'esecuzione in modo concorrente con i propri figli.
2. Il processo genitore attende che alcuni o tutti i suoi figli terminino.

Per quanto riguarda lo spazio d'indirizzi del nuovo processo altre 2 possibilità:
1. Il processo figlio è un duplicato del processo genitore (stessi programmi e dati).
2. Nel processo figlio si carica un nuovo programma.

In UNIX un nuovo processo si crea per mezzo della chiamata di sistema `fork()`, ed è composto di una copia dello spazio degli indirizzi del processo genitore. Questo meccanismo permette di comunicare senza difficoltà con il proprio processo figlio. La chiamata `fork()` riporta il valore 0 nel nuovo processo e riporta il pid del processo figlio nel processo genitore. In questo modo i processi possono procedere sapendo chi è il padre e chi è il figlio.

Generalmente, dopo , uno dei due processi esegue una chiamata di sistema `exec()` pper sostituire lo spazio di memoria del processo con un nuovo programma, quindi avvia la sua esecuzione. Il processo genitore se non ha altro da fare invoca `wait()` per rimuovere se stesso dalla coda ready fino alla terminazione del figlio.

### 3.3.2 Terminazione di un processo

Un processo termina quando finisce l'esecuzione della sua ultima istruzione e invoca la chiamata di sistema `exit()`. Un processo genitore può terminare uno dei suoi proessi figli per diversi motivi:
* Il processo figlio ha ecceduto nell'uso di alcune tra le risorse che gli sono state assegnate.
* Il compito assegnato non è più richiesto.
* Il processo genitore termina e il sistema operativo non consente a un processo figlio di continuare l'esecuzione in tale circostanza(terminazione a cascata).

Un processo genitore può attendere la terminazione di un processo figlio utilizzando la chiamata `wait()`, a cui viene passato un parametro che permette al genitore di ottenere lo stato di uscita del figlio. Quando un processo termina, le sue risorse vengono deallocate ma la sua voce nella tabella dei processi deve rimanere fino a quando il padre chiama wait(). Un processo terminato, ma il cui geitore non ha ancora chiamato `wait()` viene detto **zombie**. Il processo "init" invoca periodicamente `wait()` per raccogliere lo stato di uscita di qualsiasi processo orfano il cui geitore ha terminato senza eseguire la `wait()` e rilascia il suo PID.

## 3.4 Comunicazione tra processi

Un processo è **idipendente** se non può influire su altri processi o suburne l'influsso. è **cooperante** se ifluenza o può essere influenzato da altri processi in esecuzione. Per lo scambio di dati e informazioni i processi cooperanti necessitano di un **meccanismo di comunicazione tra processi** (**IPC**). I modelli fondamentali sono due: a **memoria condivisa** e a **scambio di messaggi**.

### 3.4.1 Sistemi a memoria condivisa
Richiede che i processi comunicanti allochino una zona di memoria condivisa. L'esecuzione concorrente dei due processi richiede la presenza di un buffer che possa essere riempito dal produttore e svuotato dal consumatore. Il buffer dovrà risiedere in una zona di memoria condivisa. I due processi devono essere sincronizzati in modo tale che il consumatore non tenti di consumare un'unità non ancora pronta. Il buffer può essere **illimitato** o **limitato**.

### 3.4.2 Sistemi a scambio di messaggi

è utile in ambienti distribuiti, dove i processi possono risiedere su macchine diverse connesse da una rete. Un meccanismo a scambio di messaggi deve prevedere almeno 2 operazioni: receive(message) e send(message).

Ci sono diversi metodi per realizzare un canale di comunicazione:
* comunicazione diretta o indiretta;
* comunicazione sincrona o asincrona;
* gestione automatica o esplicita del buffer.

Con la **comunicazione diretta**, ogni processo che intenda comunicare deve nominare esplicitamente il ricevente o il trasmittente della comunicazione. Esiste esattamente un canale tra una coppia di processi. Tali cablature (hard coding) di informazioni nel codice sono meno vantaggiose della seguente soluzione:

Con la **comunicazione idiretta** i messaggi s'iviano a delle **porte** o **mailbox**, due processi possono comunicare solo se condividono la stessa mailbox. Una mailbox può appartenere al processo o al sistema, una mailbox posseduta dal sistema operativo ha una vita autonoma, un sistema premette a un processo le segueti operazioni:
* creare una nuova mailbox;
* iviare e ricevere messaggi tramite la mailbox;
* rimuovere una mailbox.

Lo scambio di messaggi può essere:
* invio sincrono: il processo che invia il messaggio si blocca nell'attesa che venga ricevuto.
* invio asincrono: Il processo invia il messaggio e riprende la propria esesecuzione.
* Ricezione sicrona: Il ricevente si blocca nell'attesa di un messaggio.
* ricezione asincrona: Il ricevente riceve un messaggio valido o un valore nullo.

Se send() e receive() sono entrambi bloccanti si parla di **rendezvous**.

I messaggi scambiati risiedono in code temporanee, possono essere:
* capacità zero
* Capacità limitata
* Capacità illimitata

## 3.6 Comunicazione nei sistemi client-server

### 3.6.1 Socket
Una **socket** è definita come l'estremità di un canale di comunicazione. Una coppia di processi che comunica attraverso una rete usa una coppia di socket, ogni socket
è identificata da un indirizzo IP concatenato a un numero di porta.
Il server attende le richieste del client, stando in ascolto sulla porta specificata, quando il server riceve una richiesta, accetta la connessioe proveniente dalla soccket del client, e si stabilisce la comunicazione.
I server che svolgono servizi specifici stanno in ascolto su porte ben note (tutte le porte al di sotto di 1024).
Quando un processo client richiede una connessione, il calcolatore che lo ospita assegna una porta specifica (numero arbitrario maggiore di 1024).
La consegna dei pacchetti al processo giusto avviene secondo il numero della porta di destinazione.
La comunicazione tramite socket è considerata di basso livello, perchè permettono unicamente la trasmissione di un flusso non strutturato di byte.

### 3.6.2 Chiamate di procedure remote
I messaggi scambiati per la comunicazione RPC sono ben strutturati. Si indirizzano a un demone RPC, in ascolto su una porta del sistema remoto, e contengono un identificatore della funzione da eseguire e i parametri da passare a tale funzione.  Nel sistema remoto si esegue questa funzione e s'invia ogni risultato al richiedente in un messaggio distinto. La semantica delle RPC permette a un client di richiamare una procedura presente in un sistema remoto nello stesso odo i cui invocherebbe una procedura locale. Il sistema delle RPC nasconde i dettagli necessari. Un problema riguarda le differenze nella rappresentazione dei dati nel client e nel server. Per risolvere tali differenze molti sistemi di RPC definiscono una rappresentazione indipendente dalla macchina (external data representation, XDR).

### 3.6.3 Pipe

Una **pipe** agisce come un canale di trasmissione tra processi.
Le **pipe convezionali** permettono a due processi di comunicare secondo una modalità standard chiamata produttore-consumatore. Il produttore scrive a una estremità, mentre il consumatore legge dall'altra, sono quindi unidirezionali. Non si può accedere a una pipe al di fuori del processo che la crea. Solitamente un processo padre crea una pipe per comunicare con il processo figlio, ne consegue che le pipe possono essere utilizzate soltato per la comunicazione tra processi in esecuzione sulla stessa macchina.

Le **named pipe** costituiscono uno strumento di comunicazione molto più potente; la comunicazione può essere bidirezionale, e la relazione padre-figlio non è ecessaria. Una volta che si è creata la pipe, diversi processi possono utilizzarla per comunicare, inoltre, continuano ad esistere anche dopo che i processi comunicanti sono terminati.


# Capitolo 4: Thread

Un thread è l'unità di base d'uso della CPU, un processo multithread è in grado di svolgere più compiti in modo concorrente.
Vantaggi:
1. **tempo di risposta**, utile nella progettazione di interfacce utente.
2. **Condivisione delle risorse**, i thread codividono per default la memoria e le risorse del processo al quale appartengono.
3. **Economia**, è molto più conveniente creare un thread e gestirne i cambi di contesto che creare un nuovo processo.
4. **Scalabilità**, I vantaggi sono ancora maggiori nelle architetture multiprocessore, dove i thread si possono eseguire in parallelo.

### 4.2.1 Le sfide della programmaizione

Per i programmatori di applicazioni la sfida consiste nel modificare programmi esistenti e progettare nuovi programmi multithread, principali sfide:
1. **Identificazione dei task**, Consiste nell'esaminare le applicazioni al fine di individuare aree separabili in task distinti che possano essere eseguiti in parallelo.
2. **Bilanciamento**
3. **Suddivisione dei dati**
4. **Dipendenze dei dati**, In casi in cui un task dipende dai dati forniti da un altro, i programmatori devono assicurare che l'esecuzione dei task sia sincronizzata.
5. **Test e debugging**, è per natura più difficile rispetto alle applicaizioni con un singolo thread.

### 4.2.2 Tipi di parallelismo

In generale, esistono 2 tipi i parallelismo: Il **parallelismo dei dati** riguarda la distribuzione di sottoinsiemi dei dati su più core di elaborazione, Il **parallelismo delle attività** prevede la distribuzione di attività (thread) su più core, ogni thread realizza un'operazione diversa e possono operare sugli stessi dati o su dati diversi.

## 4.3 Modelli di supporto al multithreading

I thread possono essere distiti in thread a livello utente e thread a livello kernel, deve esistere una relazione tra i due livelli:
* **Modello da molti a uno**: fa corrispondere molti thread a livello utente a un singolo thread livello kernel, è impossibile eseguire thread multipli in parallelo nei sistemi con più core.
* **Modello uno a uno**: permette esecuzioe parallela ma la creazione di un thread kernel per ogni thread utente genera overhead (usato da linux e windows).
* ** Modello molti a molti**: non ha i precedenti difetti.


# Capitolo 5: Sincronizzazione dei processi

Le procedure del produttore e del consumatore possono non funzionare correttamente sei si eseguono in modo concorrente, le istruzioni contatore++ e contatore-- corrispondono a una serie di istruzioni in linguaggio macchina che possono essere eseguite in una qualunque sequenza che però conservi l'ordine interno di una singola istruzione di alto livello. Si arriva in uno stato non corretto perchè si è permesso a entrambi i processi di manipolare concorrentemete la variabile "contatore", per evitare problemi di questo tipo (**race condition**) occorre assicurare che un solo processo alla volta possa modificare la variabile "contatore".

## 5.2 Problema della sezione critica

Il problema della sezione critica consiste nel progettare un protocollo che i processi possono usare per cooperare. Ogni processo deve chiedere il permesso per entrare nella propria sezione critica. La sezione di codice che realizza questa richiesta è la **sezione d'ingresso**. La sezione critica può essere seguita da una **sezione d'uscita**.

Una soluzione del problema della sezione critica deve soddisfare i tre requisiti:
1. **Mutua esclusionne**: al più un processo esegue una sezione critica.
2. **Progresso**: Solo i processi che stanno per entrare in sezione critica possono decidere chi entra.
3. **Attesa limitata**: Deve esistere un massimo numero di volte per cui un processo può entrare (di seguito)

In un dato momento numerosi processi in modalità kernel possono essere attivi. Se ciò si verifica, il **codice del kernel** è soggetto a **race codition**. Strutture dati dal kernel soggette a questo problema possono essere quelle per l'alloczione della memoria, per la gestione delle interruzioni o le liste dei processi. Le due strategie principali per la gestione delle sezioni critiche nei sistemi operativi sono: **kernel con diritto di prelazione** e ** kernel senza diritto di prelazione. i primi sono immuni a race condition sulle strutture dati del kernel ma i kernel con diritto di prelazione possono vantare di una maggiore prontezza nelle risposte, inoltre sono più adatti alla programmazione real-time.

## 5.3 Soluzione di Peterson

é una soluzione software. A causa del modo in cui i moderni elaboratori eseguono le istruzioni elementari del linguaggio macchina, non è affatto certo che la soluzione di Peterson funzioni correttamente (scopo didattico).

La soluzione è limitata a due processi e richiede che i processi condividano i seguenti dati `int turn;` e `boolean flag[2];`, la variabile turn segnala, di chi sia il turno per l'accesso alla sezione critica mentre l'array flag, indica se un processo sia pronto a entrare nella sezione crtica.
 **tutti e 3 i requisiti sono soddisfati**.

## 5.4 Hardware per la sincronizzazione

Le tecniche hardware si basano sul concetto dei **lock**, molte delle moderne architetture offrono particolari istruzioni che permettono di controllare e modificare il contenuto di una parola di memoria in modo atomico (`test_and_set()` e `compare_ad_swap()`).

definizione di test_and_set():
```c++
boolean test_and_set(boolean *obiettivo){
	boolean valore = *obiettivo;
	*obiettivo = true;
	return valore;
}

```
Si può realizzare la mutua esclusione dichiarando una variabile booleana globale `lock` inizializzata a `false`.

Realizzazione di mutua esclusione con test_and_set() (rispetta solo mutua esclusione):
```c++
do {
   while (test_and_set(&lock)); //cicla quando restituisce vero (significa che un altro processo concorrente è in sezione critica)
   *sezione critica*
   lock = false;
   *sezione non critica*
} while (true);

```


## 5.5 Lock mutex

Le soluzioni hardware al problema della sezione critica sono complicate e generalmente inaccessibili ai programmatori di applicazioni. In alternativa si implementano strumenti software come il **lock mutex**, in pratica il processo deve acquisire il lock prima di entrare in una sezione critica e rilasciarlo quando esce(acquire() e release()).
Un lock mutex ha una variabile booleana`available` il cui valore indica se il lock è disponibile o meno, se il lock è disponibile la chiamata `acquire()` ha successo.
```c++
acquire() {
    while (!available)
       ; //attesa attiva
    available = false;
}

release() {
   available = true;
} 
```
Le due chiamate devono essere eseguite atomicamente, quindi i lock mutex spesso sono realizzati utilizzando i meccanismi hardware descritti in precedenza.
Il principale svantaggio è che richiede **attesa attiva** che spreca cicli di CPU, Gli **spinlock** sono spesso impiegati in sistemi multiprocessore in cui un thread può "girare" su un processore mentre un altro esegue la sua sezione critica.

## 5.6 Semafori

Un **semaforo** è una variabile intera cui si può accedere solo tramite due operazioni atomiche predefinite: `wait()` e `signal()`.
Si usa distinguere fra **semafori binari** (simili a lock mutex) e **semafori contatore** che trovano applicazione nel controllo dell'accesso a una data risorsa presente in un  numero finito di esemplari. Il semaforo è inizialmente impostato al numero di risorse disponibili, quando il semaforo è a 0 tutte le risorse sono occupate.

Per superare la necessità dell'attesa attiva nella definizione di `wait()` quando un processo trova il valore del semaforo non positivo invece di restare in attesa attiva può **bloccare** se stesso.
L'operazione di bloccaggio pone il processo in una coda d'attesa associata al semaforo e pone lo stato del processo a "waiting". Quindi si trasferisce il controllo allo scheduler della CPU che sceglie un altro processo pronto per l'esecuzione.
Un processo bloccato verrà riavviato in seguito all'esecuzione di un operazione `signal()` da parte di qualche altro processo. Il proceso si riavvia tramite un operazione `wakeup()`, che modifica lo stato del processo da *waiting* a *ready*.


Per realizzare i semafori in questo modo si può definire il semaforo come segue:

```c++
typedef struct {
    int value;
    struct process *list;
} semaphore;

wait(semaphore *S) {
    S->value--;
    if (S->value < 0) {
        aggiungi questo processo a S->list;
	block();
    }
}

signal(semaphore *S) {
    S->value++;
    if (S->value <= 0) {  //se c'è qualcuno in attesa
        togli un processo P da S->list;
	wakeup(P);
    }
}

```
A ogni semaforo sono associati un valore intero (`value`) e una lista di processi (`list`), contenente i processi i attesa a un semaforo, l'operazione `signal()` preleva un processo da tale lista e lo attiva.

Le operazioni sui semafori devono essere eseguite in modo atomico, in un contesto monoprocessore lo si può risolvere semplicemente inibendo le interruzioni durante l'esecuzione di signal() e wait(), mentre nei sistemi multiprocessore i sistei SMP devono mettere a disposizione altre tecniche come `compare_and_swap` e gli spinlock.

La realizzazione di un semaforo con coda di attesa può condurre a una situazione di **stallo**.
Un insieme di processi è in stallo se ciascun processo dell'insieme attende un evento che può essere causato solo da un altro processo dell'insieme. Inoltre ppossiamo avere **starvation** se i processi si aggiungono e si rimuovono dalla lista di attesa con criterio LIFO.

Può accadere che un processo con priorità più bassa (L) ifluenzi il tempo che il processo H attenderà in attesa della risorsa R.
Questo problema si risolve implementando un **protocollo di ereditarietà delle priorità**, secondo il quale tutti i processi che stanno accedendo a risorse di cui hanno
bisogno processi con priorità maggiore ereditano la priorità  più alta finchè on finiscono di utilizzare le risorse (per non avere prelazione).

## 5.7 Problemi tipici di sincronizzazione

### 5.7.1 Produttore/consumatore con memoria limitata

Strutture dati condivise:
```c++
int n;
semaphore mutex = 1;  // garantisce la mutua esclusione degli accessi al buffer.
semaphore empty = n;
semaphore full = 0;
```

Processo produttore:
```c++
do {		
   /* Produce un elemento in next_produced */
   wait(empty);
   wait(mutex);

   /* inserisci nextproduced in buffer */

   signal(mutex);
   signal(full);
} while (true);
```

Processo consumatore:
```c++
do {
   wait(full);
   wait(mutex));

   /* rimuovi un elemento da buffer e mettilo in next_consumed */

   signal(mutex);
   signal(empty);

/* cosuma l'elemento contenuto in ext_consumed*/

} while (true);
```

### 5.7.2 Problema dei lettori-scrittori

Il *primo* problema dei lettori-scrittori richiede che nessun lettore attenda, a meno che uno scrittore abbia già ottenuto il permesso di usare l'insieme di dati
condiviso.

La soluzione prevede le seguenti strutture dati condivise:
```c++
semaphore rw_mutex = 1;
semaphore mutex = 1;
int read_count = 0;
```
Il semaforo rw_mutex funziona come semaforo di mutua esclusione per gli scrittori e serve anche al primo e all'ultimo lettore che entra o esce dalla sezione critica.

Processo scrittore:
```c++
do {
   wait(rw_mutex);

   /* esegui l'operazione di scrittura */

   signal(rw_mutex);
} while (true);
```

Processo lettore:
```c++
do {
   wait(mutex)
   read_count++;
   if (read_count == 1)
      wait(rw_mutex);
   signal(mutex);

   /* esegue l'operazione di lettura */

   wait(mutex);
   readcount--;
   if (read_count == 0)
      signal(rw_mutex);
   signal(mutex);
} while (true);
```

Se uno scrittore si trova nella sezione critica e n lettori attendono di entrarvi, si accoda un lettoe a rw_mutex e n-1 lettori a mutex.
Inoltre, se uno scrittore esegue signal(rw_mutex) si può riprendere l'esecuzionw dei lettori in attesa, oppure di un singolo scrittore in attesa.
La scelta è fatta dallo scheduler.

### 5.7.3 Problema dei cinque filosofi

Una semplica soluzione consiste ne rappresentare ogni bacchetta con un semaforo. Questa soluzione garantisce che due vicini non mangino contemporaneamente, ma è insufficiente poichè non esclude la possibilità che si abbia una situazione di stallo.

Possibili soluzioni:
* un filosofo può prendere le sue bacchette solo se sono entrambe disponibili (in sezione critica)
* si adotta una soluzione simmetrica

##5.8 Monitor

è un costrutto fondamentale di sincronizzazione di alto livello.
Il monitor è un ADT che comprende un insieme di operazioni definite dal programmatore che, sono contraddistinte dalla mutua esclusione.






to be continued...

# Capitolo 6: Scheduling della CPU

Lo scheduler può essere **con prelazione** e **senza prelazione** (quando si assegna la CPU a un processo, questo rimane in possesso della CPU fino al momento del suo rilascio dovuto al termine dell'esecuzione o al passaggio allo stato di attesa. Lo scheduling con prelazione può portare a race condition.

Un elemento coinvolto nella funzione di scheduling della CPU è il **dispatcher**, si ctratta del modulo che passa effettivamente il controllo della CPU al processo
scelto dallo scheduler a breve termine.

## 6.2 Criteri di scheuling

Diversi algoritmi di scheduling della CPU hanno proprietà differenti e possono favorire una particolare classe di processi.

Alcuni criteri per il confronto di algoritmi:
* **utilizzo della CPU**: La CPU deve essere più attiva possibile.
* **Produttività**: è data dal numero di processi completati nell'unità di tempo.
* **Tempo di completamento**: è il tempo necessario per eseguire il processo stesso, ed è la somma dai tempi passati nell'attesa dell'ingresso in memoria, nella ready queque, durante l'esecuzione della CPU e nelle operazioni di I/O.
* **Tempo di attesa**: L'algoritmo di scheduling influisce solo sul tempo di attesa nella ready queque. è la somma degli intervalli d'attesa passati in questa coda.
* **tempo di risposta**: è dato dal tempo che intercorre tra la effettuazione di una richiesta e la prima risposta prodotta, ma non dal suo tempo necessario per completare l'output.
Nella maggior parte dei casi si ottimizzano i valoi medi ma in alcune circostanze è più opportuno ottimizzare i valorri minimi e massimi.

### 6.3.1 Scheduling in ordine d'arrivo

é il più semplice algoritmo di scheduling, si basa su una coda FIFO, quando un processo arriva entra nella ready queque, si collega il suo PCB all'ultimo elemento della coda.
Il tempo medio di attesa può variare grandemente all'aumentare della variabilità dei CPU burst dei vari processi. Si ha **effetto convoglio**. Non ha prelazione.

### 6.3.2 Scheduling shortest-job-first

to be continued...

# Capitolo 7: Stallo dei processi

Un insieme di processi è in stallo se ciascun processo dell'insieme attende un evento che può essere causato solo da un altro processo dell'insieme.

## 7.2 Caratteristiche della situazione di stallo
Si può avere una situazione di stallo solo se in un sistema si verificano contemporaneamente le seguenti quattro condizioni:
1. **Mutua esclusione**
2. **Possesso e attesa**
3. **Assenza di prelazione**
4. **Attesa circolare**

### 7.2.2 Grafo di assegnazione delle risorse

Le situazioni di stallo si possono descrivere avvalendosi di un **grafo di assegnazione delle risorse**. Se il grafo non contiene cicli, nessun processo del sistema subisce uno stallo, se il grafo contiene un ciclo, potrebbe esserci uno stallo.
Se ciascun tipo di risorsa ha esattamente un'istanza, allora l'esistenza di un ciclo implica la presenza di uno stallo.

## 7.3 Metodi per la gestione delle situazioni di stallo

Il problema delle situazioni di stallo si può affrontare in tre modi:
* si può usare un protocollo per prevenire o evitare le situazioni di stallo, assicurando che il sistema non entri mai in stallo.
* si può permettere al sitema di entrare i stallo, individuarlo, e quindi eseguire il riprstino.
* si può ignorare del tutto il problema, fingendo che le situazioni di stallo non possano mai verificarsi.

Quest'ultima è la soluzione utilizzata dalla maggior parte dei sistemi operativi. Quindi diveta un problema dei programmatori degli applicativi.

## 7.4 Prevenire le situazioni di stallo (Elimino una condizione necessaria)

Si può prevenire il verificarsi di uno stallo assicurando che almento una delle 4 condizioni non si verifichi.

### 7.4.2 Possesso e attesa

Si può usare un protocollo che ponga la condizione che ogni processo, prima di iniziare la propria esecuzione, richieda tutte le risorse che gli servono.
Un protocollo alternativo è quello che permette a un processo di richiedere risorse solo se non ne possiede: un processo può richiedere risorse e adoperarle, ma prima di richiedere ulteriori risorse deve rilasciare tutte quelle che possiede. Entrambi i procololli presentano degli svantaggi.

### 7.4.3 Assenza di prelazione

Il protocollo da applicare per assicurarsi che questa condizione non sussista prevede che se un processo possiede una o più risorse e ne richiede un altra che non gli si può assegnare immediatamente, allora si esercita la prelazione su tutte le risorse attualmente in suo possesso.

### 7.4.4 Attesa circolare
Un modo per assicurare che tale condizione d'attesa non si verifichi consiste nell'imporre un ordinamento totale all'insieme dei tipi di risorse e imporre che ciascun processo richieda le risorse in ordine crescente.

## 7.5 Evitare le situazioni di stallo ( (SO)attribuire risorse in modo controllato)

Un metodo per evitare situazioni di stallo consiste nel richiedere ulteriori informazioni sulle modalità di richiesta delle risorse.

Uno stato si dice **sicuro** se il sistema è in grado di assegnare risorse a ciascun processo in un certo ordine e impedire il verificarsi di uno stallo.

### 7.5.3 Algoritmo del banchiere

Quando si presenta al sistema, un nuovo processo deve dichiarare il numero massimo delle istanze di ciascun tipo di risorsa di cui potrà aver bisogno.
Quando un utente richiede un gruppo di risorse, si deve stabilire se l'assegnazione di queste risorse lasci il sistema in uno stato sicuro. Se si rispetta tale condizione, si assegnano le risorse.
Si simula l'assegnazione al processo delle risorse, se lo stato risultante è sicuro, la transazione è completata, in caso contrario si ripristina il vecchio stato.

## 7.6 Rilevamento delle situazioni di stallo

Se un sistema non si avvale di un algoritmo per prevenire o evitare lo stallo, il sistema può offrire i seguenti algoritmi:
* Un algoritmo che esamini lo stato del sistema per stabilire se si è verificato uno stallo;
* Un algoritmo che ripristini il sistema dalla condizione di stallo.
Lo schema di rilevamento e di ripristino richiede un overhead che include non solo i costi necessari per l'esecuzione dell'algoritmo di rilevamento, ma anche i potenziali costi dovuti alle perdite di
informazioni connesse al ripristino da una situazione di stallo.

### 7.6.1 Istanza singola di ciascun tipo di risorsa

Se tutte le risorse hanno una singola istanza si fa uso del **grafo d'attesa**, ottenuto dal grafo di assegnazione delle risorse togliendo i nodi dei tipi di risorse e componendo gli archi tra i processi.

### 7.6.2 Più istanze di ciascun tipo di risorsa

Si serve di strutture dati variabili nel tempo

### 7.6.3 Uso dell'algoritmo di rilevamento

Se le situazioni di stallo sono frequenti, è necessario ricorrere spesso all'algoriitmo per il loro rilevamento. Inoltre, il numero dei processi coinvolti nel ciclo di stallo può aumentare. Si potrebbe invocare l'algoritmo di rilevamento a intervalli definiti, oppure ogni volta che la CPU scende sotto il 40%.

## 7.7 Ripristino da situazioni di stallo

Una volta rilevato, uno stallo si può eliminare in 2 modi:

### 7.7.1 Terminazione ddei processi

Si possono adoperare 2 metodi:
* **Terminazione di tutti i processi in stallo**
* **Terminazione di un processo alla volta fino all'eliminazione del ciclo di stallo**. Questo metodo comporta un notevole overhead, poichè,, dopo aver terminato ogni processo,, si deve invocare un algoritmo di rilevamento per stabilire se esistono ancora processi in stallo.

### 7.7.2 Prelazione delle risorse

Le risorse si sottraggono in successione ad alcuni processi e si assegnano ad altri finchè si ottiene l'interruzione del ciclo di stallo.

Si devono considerare i seguenti problemi:
1. **Selezione di una vittima**
2. **Ristablimento di un precedente stato sicuro**. Un processo a cui è stata sottratta una risorsa bisogna ricondurlo a un precedente stato sicuro, ma è più semplice terminare il processo.
3. **Attesa indefinita**. Occorre garantire che le riisorse non siano sottratte sempre allo stesso processo.









# Capitolo 8: Memoria centrale

### 8.1.1 Hardware di base

La memoria centrale e i registri incorporati nel processore sono le sole aree di memorizzazione a cui la CPU può accedere direttamente.
I registri incorporati nella CPU sono accessibili, in genere, nell'arco di un ciclo del clock della CPU.
Ciò non vale per la memoria centrale, cui si accede tramite una transazione sul bus della memoria che può richiedere molti cicli di clock.
In tal caso il processore entra necessariamente in **stallo**, poichè manca dei dati richiesti per completare l'istruzione che sta eseguendo. Questa situazione è intollerabile, perchè gli accessi alla memoria sono frequenti. Il rimedio consiste nell'interposizione di una emoria veloce tra CPU e memoria centrale: la **cache**.

Occorre anche assicurare una corretta esecuzione delle operazioni, a tal fine bisogna proteggere il sistema operativo dall'accesso dei processi utenti.
Tale protezione deve essere messa in atto a livello hardware per questioni di prestazioni.
Bisogna assicurarsi che ciascun processo abbia uno spazio di memoria separato, in modo da proteggere i processi l'uno dall'altro. Si può implementare il meccanismo di protezione tramite due registri, detti **registro base** e **registro limite**.
Il registro base contiene il più piccolo indirizzo legale della memoria fisica; il registro limite determina la dimensione dell'intervallo ammesso.
Per mettere in atto tale meccanismo la CPU confronta ciascun indirizzo generato in modalità utente con i valori contenuti dai due registri.
Qualsiasi tentativo di accedere alle aree di memoria riservate al sistema operativo comporta l'invio di un eccezione che restituisce il controllo al sistema operativo.

### 8.1.2 Associazione degli indirizzi

* **Compilazione**: Se nella fase di compilazione si sa dove il processo risiederà, si può generare **codice assoluto**.
* **Caricamento***: Se nella fase di compilazione non è possibile sapere dove risiederà il processo, il compilatore deve generare **codice rilocabile**. In questo caso si ritarda l'associazione finale degli indirizzi alla fase di caricamento.
* **Esecuzione**: se durante l'esecuzione il processo può esssere spostato da un segmento di memoria a un altro, si deve ritardare l'associazione degli indirizzi fino alla fase d'esecuzione.

### 8.1.3 Spazi di indirizzi logici e fisici

Un indirizzo generato dalla CPU è chiamato **indirizzo logico**, mentre un indirizzo visto dall'unità di memoria è detto **indirizzo fisico**.
L'associazione nella fase d'esecuzione dagli indirizzi logici a fisici è svolta da un dispositivo detto **unità di gestione della memoria** (**MMU**): quando un processo genera un indirizzo, prima dell'invio all'unità di memoria, si somma a tale indirizzo il valore contenuto nel registro di rilocazione.

### 8.1.4 Caricamento dinamico

Per migliorare l'utilizzo della memoria si può ricorrere al **caricamento dinamico**, mediante il quale si carica in memoria centrale
una procedura solo quando viene richiamata, è utile quando servono grandi quantità di codice per gestire casi non frequenti.

### 8.1.5 Linking dinamico e librerie condivise

Le librerie collegate dinamicamente sono librerie di sistema che vengono collegate ai programmi utente quando questi vengono eseguiti.
Con il linking dinamico, per ogni riferimento a una procedura di libreria s'inserisce all'interno dell'eseguibile una piccola porzione di codice di riferimento (stub),
che indica come localizzare la giusta procedura di libreria residente in memoria. Durante l'esecuzione lo stub controlla se la procedura richiesta è già in memoria,
altrimenti provvede a caricarla, in entrambi i casi sostituisce se stesso con l'indirizzo della procedura,
inoltre in caso di aggiornamenti della libreria, tutti i programmi che fanno riferimento a quella libreria usano automaticamente la nuova versione.

## 8.2 Avvicendamento dei processi (swapping)

Un processo può essere temporaneamente tolto dalla memoria centrale e sospato in **memoria ausiliaria** e in seguito riportato in memoria per continuare l'esecuzione.
Grazie allo swapping, lo spazio totale degli indirizzi fisici di tutti i processi può eccedere la reale dimensione della memoria fisica del sistema, aumentando cosi il grado di multiprogrammazione possibile.

### 8.2.1 Avvicendamento standard

Quando lo cheduler della CPU decide di eseguire un processo, richiama il dispatcher, che controlla se il primo processo della coda si trova in memoria centrale. Se non si trova in memoria, e in questa non c'è spazio libero, il dispatcher scarica un processo dalla memoria e vi carica il processo richiesto dallo scheduler CPU.
Il tempo di cambio di contesto è piuttosto elevato. Per scaricare un processo dalla memoria è necessario essere ceti che sia completamente inattivo.

### 8.3.2 Allocazione della memoria

Uno dei metodi più semplici per l'allocazione della memoria consiste nel suddividere la stessa in **partizioni** di dimensione fissa.
Nello schema a partizione variabile il sistema operativo conserva una tabella in cui sono indicate le partizioni di memoria disponibili e quelle occupate.
Inizialment tutta la memoria è a disposizione dei processi. Nel lungo periodo, la memoria conterrà una serie di buchi di diverse dimensioni.
I criteri più usati per sceglieere un buco libero tra quelli disponibili sono i seguenti:
* **First-fit**: Si assegna il primo buco abbastanza grande.
* **Best-fit**: Si assegna il più piccolo buco in grado di contenere il processo.
* **Worst-fit**: Si assegna il buco più grande.

### 8.3.3 Frammentazione

Questi criteri dii allocazione soffrono di **frammentazione esterna**, che si ha quando lo spazio di memoria totale è sufficiente per soddisfare una richiesta, ma non è contiguo.
La frammetazione è interna quando l'hoverhead necessario per tenere traccia di un buco è nettamente più grande del buco stesso (soluzione: suddividere la memoria fisica in blocchi di dimensione fissa).
Una soluzione al problema della frammentazione esterna è la **compattazione**, tuttavia non è sempre possibile: non si può utilizzare se la rilocazione è statica ed è effettuata nella fase di assemblaggio o di caricamento.
Un'altra possibile soluzione è data dal consentire la non contiguità dello spazio degli indirizzi logici di un processo.

## 8.4 Segmentazione

La **segmentazione** è uno schema di gestione della memoria che supporta una rappresentazione della memoria dal punto di vista del programmatore.
Uno spazio d'indirizzi logici è una raccolta di segmenti, ogni riferimento a un segmento si compie per mezzo di: <numero di segmento, offset>.
Normalmente quando un programma viene compilato il compilatore costruisce automaticamete i segmenti in rapporto al programma sorgente,
possono essere creati segmenti distiti per i seguenti elementi di un programma:
il codice, le variabili globali, lo heap, gli stack usati da ciascun thread, la libreria standard del C.

### 8.4.2 Hardware di segmentazione

Occorre tradurre gli idirizzi bidimensionali definiti dall'utente negli indirizzi fisici unidimensionali.
Questa operazione si compie tramite una **tabella dei segmenti**, ogni suo elemento è una coppia ordinata: la base del segmento e il limite del segmento.
Il numero di segmento si usa come indice per la tabella dei segmenti; l'offset d dell'indirizzo logico deve essere compreso tra 0 e il limite del segmento. Se la condizione dell'offset è rispettata, questo viene sommato alla base del segmento per produrre l'indirizzo della memoria fisica dove si trova il byte desiderato.

## 8.5 Paginazione

La segmentazione permette che lo spazio degli indirizzi fisici di un processo non sia contiguo. La **paginazione** è un altro schema di gestione della memoria che offre lo stesso vantaggio. A differenza della segmentazione evita la frammentazione esterna e la necessità di compattazione.
L'implementazione della paginazione è frutto della collaborazione tra il sistema operativo e l'hardware del computer.

### 8.5.1 Metodo di base

L'implementazione consiste nel suddividere la memoria fisica in blocchi di dimensione fissa, detti **frame**, e nel suddividere la memoria logica in blocchi di pari dimensione, detti **pagine**.
Ogni indirizzo generato dalla CPU è diviso in due parti: un **numero di pagina**, e un **offset di pagina**. Il numero di pagina serve come indice per la
**tabella delle pagine**, contenente l'indirizzo di base in memoria fisica di ogni pagina.
Questo indirizzo di base si combina con l'offset di pagina per definire l'indirizzo della memoria fisica.
La dimensione di una pagina è, in genere, una potenza di 2 compresa tra 512 byte e 1 GB.
Con la paginazione si evita la frammentazione esterna: infatti qualsiasi frame libero si può assegnare a un processo che ne abbia bisogno.
Potrebbe esserci frammentazione interna in quanto lo spazio richiesto da un processo in  generale non è multiplo della dimensione della pagine.
Quindi, in teoria, conviene usare pagine piccole ma a ogni elemento della tabella delle pagine è associato un hoverhead che si riduce all'aumentare delle dimensioni delle pagine. La dimensione tipica è compresa tra i 4KB e 8KB.
Un aspetto importante della paginazione è la netta distinzione tra la memoria vista dal programmatore e l'effettiva memoria fisica, in quanto, il programma utente potrebbe essere sparso in memoria fisica. La differenza è colmata dall'hardware di traduzione degli indirizzi.

### 8.5.2 Supporto hardware alla paginazione

L'implementazione hardware della tabella delle pagine si può realizzare in modi diversi. Nel caso più semplice, si usa un insieme di registri.
Ma in genere i calcolatori usano tabelle troppo grandi. quindi vengono mantenute in memoria principalle e un **registro di base della tabella delle pagine** punta alla tabella stessa.
Con questo metodo però per accedere a un byte occorrono due accessi in memoria (uno per l'elemento della tabella e uno per il byte stesso).
La soluzione a questo problema consiste nell'impiego di una speciale, piccola cache hardware, detta **TLB**(translation look-aside buffer).
La TLB è una memoria associativa ad alta velocità in cui ogni elemento consiste in due parti: una chiave e un valore.
La ricerca è molto rapida e in un hardware moderno è parte della pipeline.
Si usa insieme con la tabella delle pagine nel modo seguente: la TLB contiene una piccola parte degli elementi della tabella delle pagine; quando la CPU genera un indirizzo logico, si presenta il suo numero di pagina alla TLB, se è presente, tale frame è immediatamente disponibile. Se non è presente (**insuccesso della TLB** TLB miss), si deve consultare la tabella delle pagine in memoria. Inoltre i numeri della pagina e del frame vengono inseriti nella TLB, e al riferimento successivo la ricerca sarà molto più rapida.
Per calcolare il **tempo effettivo d'accesso alla memoria** occorre tener conto della probabilità dei due casi.

### 8.5.3 Protezione

La protezione della memoria è assicuata dai bit di protezione associati a ogni frame, tali bit si trovano nella tabella delle pagine e possono determinare se una pagina si può leggere e scrivere o leggere soltanto ecc.
Di solito si associa a ciascun elementeo della tabella delle pagine un ulteriore bit (**bit di validità**), se impostato a valido indica che la pagina corrispondente è nello spazio d'indirizzi logici del processo.

### 8.5.4 Pagine condivise

Un ulteriore vantaggio della paginazione consiste nella possibilità di condividere codice comune.
Per essere condivisibile il codice deve essere **codice rientrante**, cioè che non cambia durante l'esecuzione.

## 8.6 Struttura della tabella delle pagine

### 8.6.1 Paginazione gerarchica

Nella maggior parte dei moderni calcolatori la tabella delle pagine diventa eccessivamente grande, quindi si adotta un algoritmo di paginazione a due livelli, in cui la tabella stessa è paginata.

# Capitolo 9: Memoria virtuale

In molti casi non è necessario avere in memoria l'intero programma.
La **memoria virtuale** è una tecnica che permette di eseguire processi che possono anche non essere completamente contenuti in meoria. 
Il vantaggio principale è quello di permettere che i programmi siano più grandi della memoria fisica.
Permette inoltre ai processi di condividere facilmente file e di realizzare memorie condivise.

## 9.2 Paginazione su richiesta

è necessario che l'hardware fornisca un meccanismo che consenta di distinguere le pagine presenti in meoria da quelle nei dischi.
A tal fine è utilizzabile lo schema basato sul bit di validità. Il bit impostato come valido significa che la pagina è valida e presente in memoria.

L'accesso a una pagina contrassegnata come non valida causa un evento di **page fault**.

Procedure di gestione dell'eccezione:
1. Si controlla una tabella interna per questo processo  per stabilire se fosse un accesso valido.
2. Se valido ma la pagina non è ancora in memoria, se ne effettua il caricamento.
3. Si individua un frame libero.
4. Si carica la pagina.
5. Si modifica la tabella interna del processo e la tabella delle pagine per indicare che la pagina si trova in memoria.
6. Si riavvia l'istruzione interrotta dall'eccezione.

La difficoltà maggiore si presenta quando un istruzione può modificare parecchie locazioni diverse, in quanto si può verificare un page fault quando lo spostamento è stato effettuato solo in parte.
Si può risolvere esegueno un microcodice che tenti di accedere prima alle estremità delle sequenze di byte per verificare che siano in memoria. Oppure si possono utilizzare alcuni registri temporanei per coservare i valori delle locazioni sovrascritte e nel caso di un page fault, si riscrivono i vecchi valori.

## 9.3 Copiatura su scrittura

La tecnica del **copy-on write** si fonda sulla condivisione iniziale da parte dei processi genitori e dei processi figli.
Le pagine condivise si contrassegnano come pagine da copiatura su scrittura, quindi, se un processo scrive su una pagina condivisa, il sistema deve creare una copia di tale pagina.

## 9.4 Sostituzione delle pagine

Si ha **sovrallocazione** quando, verificandosi un page fault, la lista dei frame liberi e vuota, quindi tutta la memoria è in uso.

### 9.4.1 Sostituzione di pagina

Quando nessun frame è libero, occorre liberarne uno attualmente utilizzato. é possibile liberarlo scrivendo il suo contenuto nell'area di swap e modificando la tabella delle pagine per indicare che la pagina non si trova più in memoria.
In questo caso sono necessari 2 trasferimenti di pagine, uno dentro e uno fuori la memoria.
Questo overhead si può ridurre usando un **bit di modifica** che viene posto a 1 ogni qual volta che la pagina viene modificata. Se è a 1 la pgina deve essere scritta su disco. Se è a 0 non c'è bisogno, c'è già.

Per realizzare la paginazione su richiesta è necessario risolvere due problemi principali: occorre sviluppare un **algoritmo di allocazione dei frame** e un **algoritmo di sostituzione delle pagine**. Ossia: occorre decidere quanti frame vadano assegnati a ciascun processo. Inoltre, quando è richiesta la sostituzione di pagina, occorre selezionare i frame da sostituire.
La progettazione di algoritmi idonei è un compito importante, poichè l'I/O dei dischi è molto oneroso.

### 9.4.2 Sostituzione delle pagine secondo FIFO

L'algoritmo più semplice è un algoritmo FIFO.
Le prestazioni non sono sempre buone, inoltre potrebbe verificarsi l'**anomalia di Belady** secondo la quale aumentando il numero di frame disponibili in alcuni casi il tasso di page fault potrebbe aumentare.


### 9.4.3 Sostituzione ottimale delle pagine

Consiste nel sostituire la pagina che non verrà usata per il periodo di tempo più lungo, rihiede la conoscenza futura, quini è utilizzato solamente per studi comparativi.

### 9.4.4 Sostituzione delle pagine usate meno recentemente (LRU)

Si sostituisce la pagina che non è stata usata per il periodo più lungo (least recent used).
Necessita di associare a ogni pagina l'istante in cui è stata usata per l'ultima volta.
Il criterio LRU si usa spesso come algoritm di sostituzione. Il problema principale riguarda la sua implementazione.
Si può realizzare con:
* **contatori**: implica una ricerca all'interno della tabella delle pagine per individuare la pagina usata meno recentemente, e una scrittura in memoria per ogni accesso alla memoria.
* **Stack**: Ogni volta che si fa un riferimento a una pagina, la si estrae dallo stack e la si colloca in cima a quest'ultimo. Ogni aggiornamento è un po più costoso ma non si deve compiere alcuna ricerca.

Entrambe le implementazioni sono possibili solo tramite un supporto hardware. 

### 9.4.5 Sostituzion delle pagine per approssimazione a LRU

Sono pochi i sistemi di calcolo che dispongono del supporto hardware per una vera sostituzione LRU.
Molti sistemi tuttavia possono fornire un aiuto: un **bit di riferimento**.

#### Agoritmo con bit supplementari di riferimento

Ulteriori informazioni sull'ordinamento si possono ottenere registrando i bit di riferimento a intervalli regolari, ad esempio conservando lo stato in una tabella da un byte per pagina a intervalli di 100 ms, salvando nel bit più significativo del byte  e shiftando gli altri a destra.

#### Algoritmo con seconda chance (orologio)

Si basa su un algoritmo FIFO, tuttavia, dopo aver selezionato una pagina, si controlla il bit di riferimento, se è 0, viene sostituita, se è 1 si dà una seconda chance e si passa alla successiva.
L'implementazione si basa sull'uso di una coda circolare. Nel caso peggiore, quando tutti i bit sono impostati a 1, il puntatore percorre un ciclo su tutta la coda.

#### Algoritmo a seconda chance migliorato

l'algoritmo si può migliorare, considerando oltre al bit di riferimento, anche quello di modifica, la differenza è che si da a preferenza a restae in memoria alle pagine modificate, al fine di ridurre il numero di I/O richiesti.

## 9.5 Allocazione dei frame

le strategie di allocazione dei fame sono soggette a parecchi vincoli. è necessario assegnare almeno un numero miimo di frame, perchè quando si verifica un page fault prima che sia stata completata l'esecuzione di un'istruzione, quest'ultima deve essere riavviata. Di conseguenza, i frame disponibili devono essere in numero sufficiente per contenere tutte le pagine cui ogni singola istruzione può far riferimento. Il numero minimo di frame per ciascun processo è definito dall'architettura.

### 9.5.2 Algoritmi di allocazione

Il modo più semplice è quello per cui a ciascun processo si da una parte uguale di frame (**allocazione uniforme**).
Un alternativa consiste nell'**allocazione proporzionale** secondo cui la memoria disponibile si assegna a ciascun processo secondo la propria dimensione.

### 9.5.3 Allocazione globale e allocazione locale

Gli algoritmi di sostituzione delle pagine si possono classificare in due categorie generali:
* **sostituzione globale**: permette he per un processo si scelga un frame per la sostituzione dall'insieme di tutti i frame, anche se qual frame è al momento allocato a un altro processo.
* **sostituzione locale**: richeide che per un processo si scelga un frame solo dal proprio insieme di frame.

Quello globale permette a un processo ad alta priorità di aumentare il proprio livello di allocazione dei frame a discapito dei processi a bassa priorità. Questo algoritmo risente di un problema: un processo non può controllare il proprio tasso di page fault, lo stesso processo può comportarsi in modi del tutto diversi, a causa di circostanze del tutto esterne.

## 9.6 Thrashing

Un processo in thrashing spende più tempo per la paginazione  che per l'esecuzione dei processi.

### 9.6.1 Cause del thrashing

Il thrashing causa notevoli problemi di prestazioni.
Aumentando il grado di multiprogrammazione aumenta anche l'utilizzo della CPU, fino a raggiungere un massimo. Se a questo punto si aumenta ulteriormente il grado di multiprogrammazione, l'attività di paginazione degenera e fa crollare l'utilizzo della CPU.

Per bloccare il thrashing occorre ridurre il grado di multiprogrammazione.
Questa situazione si può limitare usando un algoitmo di sostituzione locale, o algoritmo di sostituzione con priorità. In questo modo un processo non può provocarne la degenerazione di un altro ma il problema non è completamente risolto in quanto i processi che rimangono i thrashing occupano la coda del dispositivo di paginazione, di conseguenza il tempo di accesso in memoria aumenta anche per gli altri. Per evitare queste situazioni occorre fornire al processo tutti i frame di cui necessita. Per far ciò occorre osservare quanti sono i frame che un processo sta effettivamente usando.
Questo approccio definisce il **modello di località**.
Per esempio quando si invoca una procedura, essa definisce una nuova località.
Quindi supponendo di  allocare a un processo un numero di frame sufficiente per sistemare le sue località attuali, finchè le località non vengano modificate, non hanno luogo altri page fault.
 In caso contrario la paginazione del processo degenera. 

### 9.6.2 Modello del working set

Il **modello del working set** è basato sull'ipotesi di località. Usa un parametro (delta), per definire la **finestra del working set**. Se una pagina è in uso attivo si trova nel working set.
La caratteristica più importante è la sua dimensione. Il sistema operativo controlla il working set di ogni processo e gli assegna un numero di frame sufficiente. Se la somma totale dei working set supera il numero totale di frame disponnibili, il sistema operativo individua un processo da sospenere e assegna i suoi frame ad altri processi.
Questa strategia impedisce il trashing, mantenendo il grado di multiprogrammazioen più alto possibile.

## 9.6.3 Frequenza dei page fault

La strategia basata sulla **frequenza dei page fault** è più diretta.
Consiste nel fissare un limite inferiore e un limmite superiore per la frequenza desiderata dei page fault. Se la frequenza oltrepassa i limiti, il sistema agirà di conseguenza.

# Capitolo 11: Interfaccia del file system

## 11.3 Struttura della directory e del disco

### 11.3.3 Directory a un livello
é la struttura più semplice, tutti i file sono contenuti nella stessa directory, presenta limiti notevoli.

### 11.3.4 Directory a due livelli

Nella struttura a ue livelli, ogni utente dispone della propria **directory utente**.
Quando un utente fa un riferimento a un file particolare, il sistema operativo esegue la ricerca solo nella directory di quell'utente. In queesto modo utenti diversi possono avere file con lo stesso nome.
Questa struttura isola un utente dagli altri, potrebbe essere uno svantaggio se gli utenti volgiono collaborare e accedere ai rispettivi file.
Se l'accesso.
Una directory a due livelli può essere vista come un albero di altezza 2. La radice è la directory principale, i suoi diretti discendenti sono le directory utente, da cui discendoono i file. Quindi ogni file ha un nome di percorso.

###  11.3.5 Directory con struttura ad albero

Un albero è il più comune tipo di struttura delle directory. L'albero ha una directory radice, e ogni file del sistema ha un unico nome di percorso.
Quando si fa un riferimento a un file, si esegue ua ricerca nella directory corrente. Se il file non si tova in taale directory, l'utente deve specificare un nome di percorso.
I nomi di percorso possono essere: **relativi** (definisce un percorso che parte dalla directory corrente) o **assoluti** (comincia dalla radice dell'albero.

### 11.3.6 directory con struttura a grafo aciclico

Un **grafo aciclico** permette alle directory di avere sottodirectory e file  condivisi. In questo modo lo stesso file può essere in due directory diverse.
In genere viene realizzato con un **collegamento** (un puntatore). Questa struttura è più flessibile ma anche più complessa.

### 11.4 Montaggio di un file system

Per essere reso accessibile ai processi di un sistema, un file system deve essere montato.
Si fornisce al sistema operativo il nome del dispositivo e la sua locazione nella struttura di file e directory alla quale agganciare il file system. (detta **punto di montaggio**) Di solito, un punto di montaggio è una directory vuota.
Il passo successivo consiste nella verifica da parte del sistema operativo della validità del file system. Infine, il sistema operativo annota nella sua struttura della directory che un certo file system è montato al punto di montaggio specificato.

## 11.5 Condivisione file

### 11.5.2 File system remoti

Possibile tramite ftp, DFS (file system distribuito) (permettono il montaggio di uno o più file system di uno o più calcolatori remoti in un calcolatore locale), WWW, cloud computing, può essere anonimo o autenticato.

## 11.6 Protezione
### 11.6.2 Controllo degli accessi

L'approccio più comune al problema della protezione è rendere l'accesso dipendente dall'identità dell'utente. Lo schema più generale consiste nell'associare una **lista di controllo degli accessi** (**ACL**) a ogni file e directory; in tale lista sono specificati i nomi degli utenti e i relativi tipi d'accesso consentiti. Quando un utente richiede un accesso a un particolare file il sistema operativo esamina la lista di controllo degli accessi associata a quel file.
Il problema della liste è la loro lunghezza, che causa 2 inconvenienti:
* La costruione di una lista può essere un compito noioso.
* L'elemento della directory deve essere di dimensione variabile, quindi avremo una gestione dello spazio più complicata.

Questi problemi si risolvono introducendo una versione condensata, raggruppando gli utenti in 3 classi distinte:
* **Proprietario**
* **Gruppo**, insieme di utenti che condividono il file e hanno bisogno di accessi simili.
* **
Universo**

# Capitolo 12: Realizzazione del file system

## 12.1 Struttura del file system
Un file system presenta due problemi di progettazione molto diversi.
Il primo riguarda la definizione dell'aspetto del file system agli occhi dell'utente (file, operazioni su file, struttura delle directory).
Il secondo riguarda la creazione di algoritmi e strutture dati che permettano di far corrispondere i file system logico ai dispositivi fisici di memoria secondaria.

Lo stesso file system è generalmente composto da molti livelli distinti.
Il livello più basso, il **controllo dell'I/O** si occupa del traferimento tra memoria centrale e seconndaria. il ** file system di base** dev soltanto inviare dei generici comandi all'appropriato driver di dispositivo per leggere e scrivere blocchi fisici nel disco. Il **modulo di organizzazione dei file** può tradurre gli indirizzi dei blocchi logici negli indirizzi dei blocchi fisici.
Infine, il **file system logico** gestisce i metadati, cioè tutte le strutture del file system eccetto gli effettivi dati: gestisce la struttura delle directory, mantiene le strutture dei file tramite i **FCB** ed è responsabile anche della protezione e della sicurezza.

### 12.2.3 File system virtuali

è un componente software che permette al kernel del sistema operativo un accesso al file system attraverso delle funzioni standard e indipendenti dal reale tipo di fyle sistem utilizzato.

## 12.4 Metodi di allocazione

### 12.4.1 Allocazione contigua
Il numero di posizionamenti (seek) richiesti per acedere a file il cui spazio è allocato in modo contiguo è minimo.
Presenta alcuni problemi: bisogna individuare lo spazio per un nuovo file, il problema è, soddisfare una richiesta di dimensione n data una lista do buchi liberi (best-fit, first-fit, worst-fit), inoltre questi algoritmi soffrono di **frammentazione esterna** (soluzione: deframmentazione), altro problema: la determinazione a priori della quantità di spazio necessario per un file.

### 12.4.2 Allocazione concatenata

L'**allocazione concatenata** risolve tutti i problemi dell'allocazione contigua, ogni file è composto da una lista concatenata di blocchi del disco i quali possono essere sparsi in qualsiasi punto del disco stesso. La directory contiene un puntatore al primo e all'ultimo blocco del file. Ogni blocco contiene un puntatore al blocco successivo.
Per leggere un file occorre leggere i blocchi seguendo i puntatori da un blocco all'altro. Non esiste frammentazione esterna.

Può essere usata in modo efficiente solo per i file ad accesso sequenziale. Per trovare l'i-esimo blocco di un file occorre partire dall'inizio del file e ogni accesso a un puntatore implica una lettura del disco, e talvolta un posizionamento della testina.
Un altro svantaggio riguarda lo spazio richiesto dai puntatori.
La soluzione più comune a questo problema consiste nel riunire un certo numero di blocchi contigui in **cluster**, cosi abbiamo meno puntatori e meno posizionamenti della testina, però abbiamo frammentazione interna.

Un altro problema riguarda l'affidabilità. Poichè i file sono tenuti insieme da puntatori sparsi nel disco.

Una variante importante del metodo di allocazione concatenata consiste nell'uso della **tabella di allocazione dei file** (**FAT**).
Per contenere la tabella si riserva una sezione del disco all'inizio di ciascun volume. la FAT ha un elemento per ogni blocco del disco ed è indicizzat dal numero di blocco.

### 12.4.3 Allocazione indicizzata

In mancanza di una FAT,l'allocazione concatenata non è in grado di sostenere un efficiente accesso diretto. L'**allocazione indicizzata** risolve questo problema, raggruppando tutti i puntatori in una sola posizione: il **blocco indice**.
Ogni file ha il proprio blocco indice: si tratta di un array d'indirizzi di blocchi del disco.

L'allocazione indicizzata soffre tuttavia di un overhead maggiore: lo spazio richiesto dai puntatori del blocco indice. Occorre allocare un intero blocco indice. per gestire il problema delle dimensioni del blocco indice vi sono i seguenti meccanismi:
* **Schema concatenato**
* **Indice a più livelli**: consiste nell'impiego di un blocco indice di pprimo livello che punta a un insieme di blocchi indice di secondo livelloche, a loro volta, puntano ai blocchi dei file.
* **Schema combinato**: consiste nel tenere i primi 15 puntatori del blocco indice nell'inode del file. I primi 12 puntano a **blocchi diretti**. Gli altri 3 puntano a **blocchi indiretti**, il primo a un **blocco indiretto singolo**, il secondo a un **blocco indiretto doppio** e il terzo a un **blocco indiretto triplo**

## 2.5 Gestione dello spazio libero

Per tenere traccia dello spazio libero in un disco, il sistema conserva una **lista dello spazio libero**.

Spesso la lista dello spazio libero si realizza come una **mappa di bit**. Ogni blocco è rappresentato  da un bit: se è libero è 1, se assegnato 0.
Un altro metodo consiste nel collegare tutti i blocchi liberi e tenere un puntatore al primo di questi.
Un altro approccio sfrutta il fatto che, generalemnte, più blocchi contigui si possono allocare contemporaneamente, quindi, è sufficiente tenere l'indirizzo del primo blocco libero e il numero n di blocchi liberi contigui.