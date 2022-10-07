---
tags: [java, jdk, data structures, list, set, map, queue]
---
# DATA STRUCTURES IN THE JDK

**TO BE TRANSLATED INTO ENGLISH**


### LIST

    Iteration/enumeration
        Enumeration è il vecchio (Java 1) modo di iterare. Sostitituita dall'iterator
        
    ArrayList
    
    CopyOnWriteArrayList: 
        utile quando faccio iterazione che cancellano alcuni elementi. 
        Da non usare quando la lista è grande perché usa più copie dell'array di cui uno
        di snapshot per mantenere l'elenco iniziale nel caso si usi un iterator.
        Utile quando ho tannte letture e poche scritture come ad esempio l'elenco degli observers/listeners

    LinkedList:
        Basata su nodi e non array.
        Da non usare. Molto inefficiente
        
    Vector:
        Sincronizzata (Thread safe). Da non usare. 
        In alternativa: Collections.synchronizedList( new ArrayList<>(...))
        
    Stack:
        Da non usare. Estende Vector.
        Usare ConcurrentLinkedDequeue
        
### SET

    TreeSet
        Richiede che gli oggetti siano Comparable; l'identità degli oggetti (per capire 
        se sono univoci) dipende dal metodo compareTo, non da equals o hashCode,
        I dati inseriti sono ordinati.
        L'albero binario è di tipo Red-Black quindi mantiene un bilanciamento tra i rami
        dell'albero, indipendentemente dai dati in ingresso (se sono ordinati o meno, ad esempio).
        Non è thread-safe.

    ConcurrentSkipListSet
        Variante thread-safe di TreeSet che usa una list di tipo SkipList che usa più puntatori per arrivare
        direttamente alla metà e ad altri punti intermedi della lista, in modo da avere un accesso
        in lettura di tipo log(n) quindi più veloce della LinkedList
        Prestazioni paragonabili alla TreeSet ma in versione concorrente
        Purtroppo è una struttura dati complessa che non è molto usata, nemmeno della JDK,
        per cui Kabuz pensa che sia piena di bug.

    CopyOnWriteArraySet
        Struttura dati molto simile alla CopyOnWriteArrayList quindi safe-thread.
        I dati inseriti non sono ordinati. L'univocità dei dati è verificata con equals().
        Consigliata per quantità piccole di dati perché le operazione hanno costo quadratico (n*n)
        Add : O(n)
        Contains : O(n)
        new from other collection: O(n * n)

    HashSet
        Set non thread-safe basato su hashCode e non compareTo (o equals). NON è ordinata, quindi.
        Lookup: O(1)
        
    ConcurrentHaspMap.getKeySet
        Modo per ottenere un set thread-safe tipo TreeSet.
        
    LinkedHashSet
        Set costruito su una LinkedHashMap (vedi dopo). Non thread-safe

    EnumSet
        Set specializzata con valori che sono enum. Ottimizzata per operazioni sugli insiemi. Non thread-safe
        

### MAP
        
    HashMap
        Non thread-safe. Usa l'hashcode della chiave per determinare il bucket dove collocare la coppia chiave, valore.
        All'interno del bucket usa poi una linked list per salvare le diverse coppie. Da Java 8, se le coppie sono più di 11
        (=tanti clash) usa un treeset per passare da lookup lineare (O(n)) a logaritimico (O(log(n)).
        Per questo, da Java 8 è meglio che l'oggetto usato come chiave, se non è una String, implementi Comparable

    ConcurrentHashMap
        Versione thread-safe di HashMap. Da Java 8, l'overhead è talmente basso da non giustificare più l'uso solo in alcuni casi.
        Per evitare il rischio che una mappa che si pensa usata solo da un thread, possa essere usata da più thread, si consiglia
        di usare ConcorrentHashMap sempre.
        Attenzione: computeIfAbsent() had two concurrency bugs - a livelock and lock contention. Risolti in Java 9.

    TreeMap
        Non thread-safe. Le chiavi sono ordinate per cui devono implementare Comparable.
        
    ConcurrentSkipListMap
        Versione concorrente di TreeMap
        Vedi <ConcurrentSkipListSet> e per gli stessi motivi Kabutz suggerisce di non usarla
 
    Hashtable
        Esiste da Java 1. Concorrente ma molto lento nelle letture. Preferire ConcorrentHashMap

    LinkedHashMap
        Simile ad HashMap ma con puntatori (link) che ricordano l'ordine di inserimento
        Non thread-safe
        Funzionalità interessante è che l'ordine, che di default, è quello di inserimento, può essere invece
        a "access_order" ovvero in ordine di ultimo accesso (get). LRU = least recently used
        
    EnumMap
        Mappa specializzata con le chiavi che sono enum. Non thread-safe. Non molto usata
        Utile al posto di usare <enum>.ordinal() come indice per un array/lista
        
    IndentityHashMap
        Mappa che controlla l'esistenza di una chiave attraverso l'identità (==) invece che con l'uguaglianza (equals)
        Non molto usata. Non thread-safe
        
    Properties
        Esiste da Java 1, estende da Hashtable ma adesso usa internamente una ConcorrentHashMap
        
    WeakHashMap
        Variante di HashMap che usa una WeakReference per le chiavi (ma una normale Stringreference sui valori)
        Da non usare perché impredicibili nel tempi (il purge è nelle diverse chiamate) e perché il WeakReference
        non sono ben gestiti dai GC.

### ITERATORI:

    fail-fast:
        L'iteratore controlla che la collezione sottostante non sia modificata durante l'iterazione, nel qual
        caso lancia una ConcurrentModificationException.
        Le collection non thread-safe normalmente hanno un iteratore fail-fast
        Non è però garantito (solo best-effort) che l'iterator rilevi la modifica.
        
    weakly consistent (o fail-safe)
        Sono gli iteratori delle collezioni thread-safe. Gestiscono il fatto che la collezione sottostante
        possa essere modificata; non lanciano mai ConcurrentModificationException.
        Non garantisco però (da qui il wekly) di riportare l'ultimo stato delle modifiche.
        Il numero di elementi iterati è quello presente al momento della creazione dell'iteratore.
        
    snapshot
        L'iteratore usa uno snapshot della collezione. usato in CopyOnWriteArrayList e CopyOnWriteArraySet
        (le collezioni COW).
        Questi iteratori sono a sola lettura (non modificano lo snalpshot).
        
    undefined
        Iterator e enumeration delle vecchie collections: Vector e Hashtable.
        Non danno garanzie di alcun genere in caso di modifiche concorrenti.
        
        
QUEUE e DEQUEUE 

    QUEUE   = FIFO, pronuncia "kiu"
    DEQUEUE = FIFO e LIFO, pronuncia "deck", (Double Ended Queue)

    Le code implementano operazioni secondo diversi nomi:
                Throws Ex   Return result
        Insert 	add(e) 	    offer(e)        aggiunge alla tail
        Remove 	remove() 	poll()          prende dalla head
        Examine element() 	peek()          esamina la head

    Nelle dequeu first = head, last = tail

    Le deque hanno anche i metodi "stack": push (=addFirst), pop (=removeFirst)

    L'ordine delle code è di solito il FIFO; 
        le priority però usano un comparatore
        le dequeue usano anche LIFO (o stack)

    Le code non permettono di solito l'inserimento di null
    
    
    ConcurrentLinkedQueue, ConcurrentLinkedDequeue
        Entrambe multi producer multi consumer. Lunghezza della coda unbounded (non limitata)
        La funzione size() ha un costo alto ( O(n) ) perché deve sempre iterare gli elementi per contarli. In una coda 
        però non ha molto senso contare gli element.
        L'iterator è weakly consistent
        ConcurrentLinkedDequeue può essere usato come stack thread-safe
        ConcurrentLinkedQueue può essere usato come LinkedList thread-safe se non si usa la get(index)
        
    ArrayDeque
        Non thread-safe. Implementazione di dequeue basata su array. 
        Veloce, la maggioranza delle operazioni sono costant time ( O(1) ). 
        Null non accettato. Iterator fail-fast.
        Non si riduce in dimensioni se togli elementi.

    Blocking queues and dequeues
        Variante che blocca la scrittura nel caso la coda è piena e la lettura nel caso la coda è vuota.
        Le blocking sono sempre thread-safe e prevedono meccanismi di timeout per non bloccare definitivamente il 
        thread che accede alla coda.
        Non accettano nulli.

    LinkedBlockingQueue e LinkedBlockingDeque 
        Hanno un lock sul head e uno sul tail perché sono ottimizzate per single producer single consumer.
        Negli altri casi il loro lock determina prestazioni più scarse rispetto ad altre code.
        E' possibile indicare una capacità (lunghezza della coda) massima.

    ArrayBlockingQueue
        Bounded queue implementata con un array di dimensioni fisse (non ridimensionabile).
        Usa un singolo lock per cui c'è contention quando c'è più di un producer e/o consumer.
        Efficiente nell'uso della memoria.

     DelayQueue
        Queue non molto usata in JDK. Gli elementi della coda sono visibili al consumer solo dopo un ritardo.
        Kabutz suggerisce di usare in alternativa un ScheduledExecutorService

    SynchronousQueue
        Coda sincrona (nel senso di non asincrona) che di fatto ha 0 o 1 elemento.
        Usata per implementare la cachedThreadPool e poco altro.
        A blocking queue in which each insert operation must wait for a corresponding remove operation 
        by another thread, and vice versa. A synchronous queue does not have any internal capacity, 
        not even a capacity of one. You cannot peek at a synchronous queue because an element is only 
        present when you try to remove it; you cannot insert an element (using any method) unless another 
        thread is trying to remove it; you cannot iterate as there is nothing to iterate. The head of the 
        queue is the element that the first queued inserting thread is trying to add to the queue; if 
        there is no such queued thread then no element is available for removal and poll() will return 
        null. For purposes of other Collection methods (for example contains), a SynchronousQueue 
        acts as an empty collection. This queue does not permit null elements. 

    LinkedTransferQueue 
        Coda introdotta nella beta di Java 7 ad uso del ForkJoinPool ma poi non più usata.
        Di per sì non è male ma non essendo utilizzata meglio non usarla.

    PriorityQueue and PriorityBlockingQueue
        Code unbounded che considerano l'ordinamento del dato per definire l'ordine nella coda. Attenzione però che l'ordinamento
        non è stabile ovvero il risultato dell'ordinamento non è sempre lo stesso, in particolare nei dati che sono
        uguali tra di loro
        PriorityQueue non è thread safe mentre lo è PriorityBlockingQueue
        La priorità può essere definita in ordine naturale (se il dato implementa Comparable) o con un Comparator.


