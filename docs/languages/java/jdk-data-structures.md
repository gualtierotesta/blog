---
tags: [java, jdk, data structures, list, set, map, queue, iterator]
---
# DATA STRUCTURES IN THE JDK

*Last update: 9 Nov 2022*


### LIST

Iteration/enumeration

    The enumeration is the old way (Java 1) to iterate a collection of items. It has been replaced by the iterator.
        
ArrayList

    The standard list. It is based on an array.
    
CopyOnWriteArrayList

    Useful when removing elements during iteration.
    It should not be used when the list is very big because it creates several copies of the underlying array. 
    One of the copies is kept to be used for the iterator.
    Useful when there are many readers and few writers like in observers/listener lists.
    observers/listeners

LinkedList (Do not use)

    It is based on nodes, not arrays.
    Do not use, very inefficient.
        
Vector (Do not use)

    Synchronised array.
    Alternative: Collections.synchronizedList( new ArrayList<>(...))

Stack  (Do not use)

    Extend Vector.
    Alternative: ConcurrentLinkedDequeue


### SET

TreeSet

    The items in the collection should be Comparable: the identity of the items is based on the method compareTo, not on the methods equals or hashCode.
    The items are ordered.
    The binary tree is a Red-Black type so tree branches are balanced in length.
    Not thread-safe.

ConcurrentSkipListSet

    A thread-safe variant of TreeSet that uses a SkipList list that uses multiple pointers to get directly to the middle and other intermediate points of the list, to have a log(n) read access therefore faster than the LinkedList.

    The performances are similar to the TreeSet but concurrently.
    It is not frequently used, even in the JDK itself.

CopyOnWriteArraySet

    It is a thread-safe structure similar to CopyOnWriteArrayList.
    Items are not ordered. The method equals is used for univocity.
    Suggested for small collections because all operations have quadratic (n*n) cost.

HashSet

    A not thread-safe set which uses the method hashCode for univocity.
    Items are not ordered.
        
ConcurrentHashMap.getKeySet

    A way to get a thread-safe set similar to TreeSet.
        
LinkedHashSet

    A not thread-safe set based on LinkedHashMap (see later)

EnumSet
    Non thread-safe. It is a set where items are enums.
    It is optimised for set operations.


### MAP
        
HashMap

    Not thread-safe. 
    The key hashCode is used to determine the bucket in the map where to store the key-value couple.
    Inside the bucket, the couple is saved in a linked list.
    From Java 8, if the bucket contains more than 11 couples, the linked list is replaced by a tree set for performance reasons. it is so better to use a key which implements the Comparable interface.

ConcurrentHashMap

    The thread-safe variant of the HashMap.
    From Jaba 8, its overhead is low and there are no more strong reasons to prefer the HashMap.
    Be aware that two concurrency bugs in the computeIfAbsent method have been fixed in Java 9.

TreeMap
    
    Not thread-safe. The keys are ordered and they should implement the Comparable interface.
        
ConcurrentSkipListMap

    It is the concurrent version of the TreeMap.
    Not so used.
    
Hashtable

    It is a Java 1 concurrent map with very slow reads.
    Better to use the ConcorrentHashMap

LinkedHashMap

    Not thread-safe. It is similar to the HashMap but with links that record the insertion order.
    The order can be changed to a least recently used (LRU) order.
    
    
EnumMap

    Not thread-safe. It is a specialised map where the keys are enums.
    Not so used but it is convenient as a list index instead of the Enum.ordinal() method.

IndentityHashMap

    Not thread-safe. The key's presence is checked with the identity (==) instead of the equality ( equals() ).
    Not so used.
    
Properties

    It exists since Java 1. Originally it used a HashTable but now it is based on a ConcurrentHashMap.
        
WeakHashMap

    Similar to the HashMap where the keys are WeakReferences. The idea is the map can be emptied by the garbage collector.
    Operation time is unpredictable because the purge is implemented in the calls. Moreover, the WeakReferences are not handled very well by the garbage collector.

### ITERATORS

fail-fast

    The iterator checks if the underlying collection has been modified. If so, it throws a ConcurrentModificationException.
    The not thread-safe collections usually have a fail-fast iterator.
    It is not guaranteed (best-effort) that the iterator detects the change in the collection.
       
weakly consistent (or fail-safe)

    The thread-safe collections use fail-safe iterators. These iterators can handle the changes in the underlying collection; they never throw a ConcurrentModificationException.
    They cannot guarantee (weakly consistency) that they report the latest state of the collection.
    The number of the collection's items iterated is the one when the iterator has been created.

snapshot

    The snapshot iterator uses a read-only snapshot of the content of the collection. It is used in the COW collections (CopyOnWriteArrayList and CopyOnWriteArraySet).

undefined

    Iterator added to the old Vector and HashTable collections.
    Their behaviour is undefined when there are concurrent writes.
        
        
### QUEUE e DEQUEUE 

QUEUE   = FIFO, pronounced "kiu"
DEQUEUE = FIFO e LIFO, pronounced "deck", (Double Ended Queue)

    Queue operations have different names and implementing methods:

        - INSERT: add or offer
        - REMOVE: remove or poll
        - CHECK: element or peek
        - STACK (dequeue only): push (=addFirst) and pop (=removeFirst)

    The order is usually FIFO but the priority queues use a comparator and the dequeues can also use a LIFO order.

    All queues and dequeues do not allow null values.

    
ConcurrentLinkedQueue, ConcurrentLinkedDequeue

    They're both multi-producer and multi-consumer. The queue length is unbounded (=without limits).
    The size() method has a high cost because all items should be iterated. Moreover, there is no meaning to count the items in a queue.
    The iterator is weakly consistent.
    The ConcurrentLinkedDequeue can be used as a thread-safe stack.
    The ConcurrentLinkedQueue can be used as a thread-safe LinkedList but the method get(index) should not be used.


ArrayDeque

    It is not thread-safe; the underlying structure is an array.
    Fast, most of the operation costs have constant time ( O(1)).
    Nulls not accepted. Fail-fast operator.

Blocking queues and dequeues

    They block the write operations if the queue is full and the read operations if the queue is empty.
    They are thread-safe with timeouts that can prevent forever blocking.
    Nulls not accepted.

LinkedBlockingQueue e LinkedBlockingDeque 

    They are optimised for the single-producer single-consumer use case. In other cases, they can be slower than the other queues.
    It is possible to define a limit in the queue length.

ArrayBlockingQueue

    Bounded queue based on a fixed length array. Bounded
    Memory efficient, it uses a single lock so there is contention when there are more producers and/or consumers.

DelayQueue

    Not so used in the JDK.
    The elements in the queue are visible to the consumer only after a delay.

SynchronousQueue

    A synchronous queue with max one element. It is used to implement the cachedThreadPool.
    
    A blocking queue in which each inserts operation must wait for a corresponding remove operation by another thread, and vice versa.
    
    A synchronous queue does not have any internal capacity, not even a capacity of one. You cannot peek at a synchronous queue because an element is only present when you try to remove it; you cannot insert an element (using any method) unless another thread is trying to remove it; you cannot iterate as there is nothing to iterate. 
    
    The head of the queue is the element that the first queued inserting thread is trying to add to the queue; if there is no such queued thread then no element is available for removal and poll() will return null. 
    
    For purposes of other Collection methods (for example, contains), a SynchronousQueue acts as an empty collection. This queue does not permit null elements. 

LinkedTransferQueue 

    Introduced in a JAVA 7 beta version to be used in the ForkJoinPool but then no more used.
    
PriorityQueue and PriorityBlockingQueue

    These are unbounded queues that sort the elements in the queue.
    The PriorityQueue is not thread-safe while the PriorityBlockingQueue is.
    The priority is based on the natural order (if items implement Comparable) or via a comparator.

