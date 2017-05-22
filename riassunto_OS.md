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






to be continued...

# Capitolo 6: Scheduling della CPU

Lo scheduler può essere **con prelazione** e **senza prelazione** (quando si assegna la CPU a un processo, questo rimane in possesso della CPU fino al momento del suo rilascio dovuto al termine dell'esecuzione o al passaggio allo stato di attesa. Lo scheduling con prelazione può portare a race condition.

Un elemento coinvolto nella funzione di scheduling della CPU è il **dispatcher**, si ctratta del modulo che passa effettivamente il controllo della CPU al processo scelto dallo scheduler a breve termine.

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

# Capitolo 8: Memoria centrale

### 8.1.1 Hardware di base

La memoria centrale e i registri icorporati el processore sono le sole aree di memorizzazione a cui la CPU può accedere direttamente.
I registri incorporati nella CPU sono accessibili, in genere, nell'arco di un ciclo del clock della CPU.
Ciò non vale per la memoria centrale, cui si accede tramite una transazione sul bus della memoria che può richiedere molti cicli di clock. In tal caso il processore entra necessariamente in **stallo**, pooichè manca dei dati richiesti per completare l'istruzione che sta eseguendo. Questa situazione è intollerabile, perchè gli accessi alla memoria sono frequenti. Il rimedio consiste nell'interposizione di una emoria veloce tra CPU e memoria centrale: la **cache**.

Occorre anche assicurare una corretta esecuzione delle operazioni, a tal fine bisogna proteggere il sistema operativo dall'accesso dei rocessi utenti.
Tale protezione deve essere messa in atto a livello hardware per questioni di prestazioni.
Bisogna assicurarsi che ciascun processo abbia uno spazio di memoria separato, in modo da proteggere i processi l'uno dall'altro. Si può implementare il meccanismo di protezione tramite due registri, detti **registro base** e **registro limite**.
Il registro base contiene il più piccolo indirizzo legale della memoria fisica; il registro limite determina la dimensione dell'intervallo ammesso.
Per mettere in atto tale meccanismo la CPU confronta ciascun indirizzo generato in modalità utente con i valori contenuti dai due registri.
Qualsiasi tentativo di accedere alle aree di memoria riservate al sistema operativo conporta l'invio di un eccezione che restituisce il controllo al sistema operativo.

### 8.1.3 Spazi di indirizzi logici e fisici

Un indirizzo generato dalla CPU è chiamato **indirizzo logico**, mentre un indirizzo visto dall'unità di memoria è detto **indirizzo fisico**.
L'associazione nella fase d'esecuzione dagli indirizzi logici a fisici è svolta da un dispositivo detto **unità di gestione della memoria** (**MMU**): quando un processo genera un indirizzo, prima dell'invio all'unità di memoria, si somma a tale indirizzo il valore contenuto nel registro di rilocazione.

### 8.1.4 Caricamento dinamico

Per migliorare l'utilizzo della memoria si può ricorrere al **caricamento dinamico**, mediante il quale si carica in memoria centrale
una procedura solo quando viene richiamata, è utile quando servono grandi quantità di codice per gestire casi non frequenti.

### 8.1.5 Linking dinamico e librerie condivise

Le librerie collegate dinamicamente sono librerie di sistema che vengono collegate ai programmi utente quando questi vengono eseguiti.
Con il linking dinamico, per ogni riferimento a una procedura di libreria s'inserisce all'interno dell'eseguibile una piccola porzione di codice di riferimento (stub), che indica come localizzare la giusta procedura di libreria residente in memoria. Durante l'esecuzione lo stub controlla se la procedura richiesta è già in memoria,
 altrimenti provvede a caricarla, in entrambi i casi sostituisce se stesso con l'indirizzo della procedura, inoltre in caso di aggiornamenti della libreria, tutti i programmi che fanno riferimento a quella libreria usano automaticamente la nuova versione.

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
Uno spazio d'indirizzi logici è una raccolta di segmeti, ogni riferimento a un segmento si compie per mezzo di: <numero di segmento, offset>.
Normalmente quando un programma viene compilato il compilatore costruisce automaticamete i segmenti in rapporto al programma sorgente, possono essere creati segmenti distiti per i seguenti elementi di un programma: il codice, le variabili globali, lo heap, gli stack usati da ciascun thread, la libreria stadard del C.

### 8.4.2 Hardware di segmentazione

Occorre tradurre gli idirizzi bidimensionali definiti dall'utente negli indirizzi fisici unidimensionali. Questa operazione si compie tramite una **tabella dei segmenti**, ogni suo elemento è una coppia ordinata: la base del segmento e il limite del segmento.
Il numero di segmento si usa come indice per la tabella dei segmenti; l'offset d dell'indirizzo logico deve essere compreso tra 0 e il limite del segmento. Se la condizione dell'offset è rispettaata, questo viene sommato alla base del segmento per produrre l'indirizzo della memoria fisica dove si trova il byte desiderato.

to be continued...
