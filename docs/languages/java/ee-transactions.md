---
tags: [java, ee, transactions]
---
# JAVA EE Transactions

**TO BE TRANSLATED INTO ENGLISH**


The transactions can be 

* Container managed
* Application (o bean) managed

Resources related to the transactions

```java
@Resource
private EJBContext context;

@Resource
private UserTransaction userTransaction;
```

Links:

1. [Rollback](ttp://www.developerscrappad.com/547/java/java-ee/ejb3-x-jpa-when-to-use-rollback-and-setrollbackonly/)
2. [Teoria sulle transazoni](http://www.giuseppesicari.it/articoli/java-transaction-api/)
3. [Force rollback](http://stackoverflow.com/questions/832375/is-there-a-way-to-force-a-transactional-rollback-without-encountering-an-excepti)


### Container-managed transaction (CMT)

The CMT transactions are handled by the EJB container.

```java
@TransactionManagement(TransactionManagementType.CONTAINER)
```
I limiti di start e stop della transazione sono i metodi dell'EJB: la transazione è creata prima della chiamata al metodo e il commit subito dpo la fine del metodo.

Il **rollback** è determinato dalle eccezioni o da una chiamata diretta a _context.setRollbackOnly()_

Vietato l'uso di :

* Metodi commit, setAutoCommit e rollback di java.sql.Connection
* Metodo getUserTransaction di javax.ejb.EJBContext
* Tutti i metodi di javax.transaction.UserTransaction

L'annotation **@TransactionAttribute** determina il comportamento transazionale del singolo metodo o di tutti i metodi nella classe. I suo valori sono:

* REQUIRED : default, usa la transazione del chiamante o ne crea una nuova se il chiamante non è in transazione
* REQUIRES_NEW : una nuova transazione è sempre creata; se il chiamante è in transazione, questa viene sospesa
* MANDATORY: il chiamante deve essere in transazione pena eccezione di _TransactionRequired_
* NOT_SUPPORTED: per metodi che devono essere eseguiti **fuori** da una transazione; se il chiamante è in transazione, questa viene sospesa
* SUPPORTS: in transazione solo se anche il chiamante è in transazione. Da usare con cautela
* NEVER: mai in transazione; lancia eccezione se chiamante è in eccezione

Nota: gli MDB supportano solo gli attributi REQUIRED e NOT_SUPPORTED

Pseudo-codice:

```
try {
    ..updates
} catch(Exception ecc) {
    sessioncontext.setRollbackOnly();
}
```

### Application-bean managed transaction (BMT)

The BMT transactions are handled by our code.

```java
    @TransactionManagement(TransactionManagementType.BEAN)
```
Gestite dal codice: le transazioni sono create e chiuse in maniera esplicita.

L'API di riferimento è la JTA (Java Transaction API).

L'interface di riferimento è **javax.transaction.UserTransaction* che permette di iniziare, chiudere e abortire le transazioni in maniera programmatica.

Pseudo-codice:

```
try {
    usertransaction.begin();
    ..updates
    usertransaction.commit();
} catch(Exception ecc) {
    usertransaction.rollback;
}
```

## TEORIA

Tutti i sistemi transazionali esibiscono quattro caratteristiche note con l’acronimo **ACID**: 

* **Atomicità** significa che i task che costituiscono la transazione costituiscono un’unica unità di elaborazione pertanto devono essere eseguiti tutti affinchè la transazione possa considerarsi completata positivamente.
* **Consistenza** significa che se il sistema si trova in uno stato consistente con le regole di business prima dell’invocazione della transazione, allora successivamente all’invocazione dovrà trovarsi ancora in uno stato consistente.
* **Isolamento** è quella proprietà che assicura che le transazioni non si influenzino vicendevolmente.
* **Duravolezza** significa che se una transazione che è stata completata con successo i suoi effetti diventano permanente.

Altro concetto importante è il **two-phase commit** che indica che tutte le risorse coinvolte nella transazioni arrivano prima ad uno stadio di commit preliminare per poi passare a quello definitivo ma solo se tutte sono al preliminare. Se una fallisce allora tutte le risorse fanno rollback.

