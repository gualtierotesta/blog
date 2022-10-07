---
tags: [java, exceptions, best practice, antipattern]
---
# Exceptions in Java

**TO BE TRANSLATED INTO ENGLISH**


## https://community.oracle.com/docs/DOC-983543

Exception-Handling Antipatterns Blog

https://www.baeldung.com/java-lambda-exceptions


## Concetti base:

Le eccezioni in Java si dividono in checked, unchecked e error

Checked exceptions are exceptions that must be declared in the throws clause of a method. 
They extend Exception and are intended to be an "in your face" type of exceptions. 
A checked exception indicates an expected problem that can occur during normal system operation. 
Some examples are problems communicating with external systems, and problems with user input. 
Note that, depending on your code's intended function, "user input" may refer to a user interface, or it may refer to the parameters that another developer passes to your API. 
Often, the correct response to a checked exception is to try again later, or to prompt the user to modify his input.

Unchecked exceptions are exceptions that do not need to be declared in a throws clause. 
They extend RuntimeException. 
An unchecked exception indicates an unexpected problem that is probably due to a bug in the code. 
The most common example is a NullPointerException. 
There are many core exceptions in the JDK that are checked exceptions but really shouldn't be, such as IllegalAccessException and NoSuchMethodException. 
An unchecked exception probably shouldn't be retried, and the correct response is usually to do nothing, and let it bubble up out of your method and through the execution stack. 
This is why it doesn't need to be declared in a throws clause. 
Eventually, at a high level of execution, the exception should probably be logged (see below).

Errors are serious problems that are almost certainly not recoverable. 
Some examples are OutOfMemoryError,LinkageError, and StackOverflowError.

## Eccezioni e UI


1) Mostro il dettaglio del problema (lo stack trace della eccezione)
L'applicazione rimane in uno stato indefinito
Informazioni interne mostrato all'utente che non le può capire.

2) Nascondo le eccezioni
L'applicazione sembra funzionante ma si comporta in maniera inattesa (ad. es. combo vuota, pulsante che non fa niente)

3) Segnalo il problema all'utente
L'applicazione mostra un messaggio all'utente segnalando che è avvenuto un problema.
L'utente NON può fare niente: il messaggio serve solo ad informarlo che ci sono problemi ed eventualmente segnalare la cosa al supporto.
L'applicazione "dovrebbe" portarsi in uno stato definito (pagina di errore, refresh, ritorno alla home page....)



## Effective Java (3)

Item 69: Usare le eccezioni solo per condizioni eccezionali
	- Non devono essere usate per il normale flusso di controllo
	- Sono più lente della logica standard (poco ottimizzate dalla JVM)
	- In chiamate stateful, fornire metodi che riportano lo stato (Iterator.hasNext) da usarsi prima di quelle che riportano il dato (Iterator.next()).
	- In alternativa fornire un Optional.empty o valore marcatore se il dato manca.

Item 70: Usare le eccezioni checked per gli errori recuperabili e le unchecked per gli errori di programmazione

	- Nel dubbio lanciare Unchecked
	- Non lanciare mai Error nè estenderlo
	- Le unchecked devono estendere da RunTimeException (direttamente o indirettamente)
	- Non usare mai il message di una eccezione per raccogliere informazioni
	- Nelle checked, fornire informazioni aggiuntive (con campi ad hoc) per facilitare il lavoro del chiamante
	
Item 71: Evitare un uso non necessario delle checked

	- Le checked permettono una maggiore affidabilità in quanto obbligano a gestire il problema
	- Rendono però il codice meno chiaro e leggibile
	- Se il chiamante non può gestire il problema, lanciare unchecked
	- In alternativa fornire un Optional.empty o valore marcatore se il dato manca. 
	
Item 72: Quando possibile usare le eccezioni standard

	- Codice più chiaro
	
Item 73: Lanciare eccezioni appropriate al livello di astrazione corrente

	- Non (ri)lanciare eccezioni provenienti dai livelli applicativi più bassi (tipo SQLException)
	- Meglio rimappare su una eccezioni adatta al livello ("exception translation")
	- "Allegare" come cause l'eccezione sottostante a quella lanciata
	
Item 74: documentare TUTTE le eccezioni lanciate dai metodi
	
	- Documentare sia le checked sia le unchecked lanciate (non ovviamente quelle che si possono generare senza volerlo)
	- Descrivere le condizioni per le quali l'eccezione è lanciata
	
Item 75: Aggiungere informazioni di contesto nei messaggi

	- Idealmente, il messaggio della eccezione deve contenere i valori di tutti i parametri, campi e variabili che hanno contribuito all'eccezione
	- Non includere informazioni sensitive (password, chiavi,...)
	- Aggiungere metodi che forniscono le informazioni di contesto (per una gestione programmativa della eccezione da parte del chiamanet)
	
	
Item 76: Cercare di ottenere la failure atomicity

	- Dopo una eccezione, l'oggetto che la lanciata deve essere in uno stato valido (ad es. come era prima della chiamata che è andata in eccezione)
	
	
Item 77: Non ignorare le eccezioni

	- Mai un catch vuoto
	- Se è proprio necessario, aggiungere un commento e chiamare l'eccezione ignored (invece di e o ex).
	
	
## Oracle Antipatterns

Antipattern: un errore di programmazione molto diffuso
