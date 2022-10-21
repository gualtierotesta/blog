---
tags: [java, ee, transactions]
---
# JAVA EE Transactions

*Last update: 21 Oct 2022*

The Java EE transactions can be 

* Container managed
* Application (o bean) managed

The following resources are related to the transactions:

```java
@Resource
private EJBContext context;

@Resource
private UserTransaction userTransaction;
```

Links:

1. [Force rollback](https://stackoverflow.com/questions/832375/is-there-a-way-to-force-a-transactional-rollback-without-encountering-an-excepti)


### Container-Managed Transaction (CMT)

The container-managed transactions are handled by the EJB container and the EJB class has the following annotation:

```java
@TransactionManagement(TransactionManagementType.CONTAINER)
```

The EJB methods are the points where the transaction starts and ends. In general, the transaction is created by the container before invoking the method and it is committed after the method returns.

The **rollback** of a failing transaction is caused by an exception raised by the invoked method. The roolback can also be forced by invoking the `context.setRollbackOnly()` method.

In a container-managed transaction, it is **forbidden** to:

* Invoke the methods `commit`, `setAutoCommit`, and `rollback` of the class `java.sql.Connection`
* Invoke the method `getUserTransaction` of the class `javax.ejb.EJBContext`
* Invoke any method of the class `javax.transaction.UserTransaction`

The annotation `@TransactionAttribute` determines how the container creates the transactions before invoking the EJB method. Its possible values are:

* REQUIRED: the default value. The container creates a new transaction or uses the one created by the invoking method (if any)
* REQUIRES_NEW: the container always creates a new transaction, even if there is already one transaction active (that will be suspended).
* MANDATORY: there should be already an active transaction created by the invoking method otherwise a `TransactionRequiredException` is raised.
* NOT_SUPPORTED: the method without any transaction. If an active transaction exists, it is suspended.
* SUPPORTS: if there is an active transaction, it will be kept active while invoking the method. Otherwise, no transaction is created. The use of this value is discouraged because of the uncertainty of the transaction's presence.
* NEVER: the method should never be executed inside a transaction, An exception is raised if one active transaction exists.

Please note that the MDB just support the REQUIRED and NOT_SUPPORTED attributes.

Pseudo-code:

```java
try {
    //..updates
} catch(Exception ecc) {
    // An exception has been detected. Rollback the transaction
    sessioncontext.setRollbackOnly();
}
```

### Application-bean managed transaction (BMT)

The BMT transactions are handled by our code. The following annotation should be specified at EJB class level

```java
    @TransactionManagement(TransactionManagementType.BEAN)
```

Our code should create the transactions and close them normally (commit) or abnormally (rollback).

The Java Transaction API (JTA) is the API we should refer to handle the bean-managed transactions.

Within this API, the interface `javax.transaction.UserTransaction` is the one we should use to perform actions on the transactions. An instance of this interface can be injected as a resource (see above).

Pseudo-codice:

```java
try {
    usertransaction.begin();
    // ..updates
    usertransaction.commit();
} catch(Exception ecc) {
    usertransaction.rollback;
}
```
