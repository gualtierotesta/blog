---
tags: [java, coding, logging, rules, log levels, slf4j, jul, production]
---

# The 5 Java logging rules

*Last update: 08 Sep 2022*

Logging is a critical factor that should be always kept into account during software development.

When something bad happens in production, the log files are usually the starting point of our fault analysis. And, often, they are the only information in our hands to understand what is happening and which is the root cause of the problem.

It is so very important to have the required information logged properly.

The following five logging rules are a way to check and, possibly, improve how we handle the logging in our code.

Please note that we will not discuss how to configure a logging engine nor we will compare them to each other.

## Rule 1. Logging is for readers

The logging messages should be meaningful to who will read the log files, not only to who wrote the (logging) code.

It seems a very obvious rule but it is often violated.

For example, let's consider a log message like the following

```
ERROR: Save failure - SQLException stack trace .....
```

Saving what? This message could mean something to the developer but it is completely useless for the poor guy which is looking at the production problem.

A much better message is

```
ERROR: Save failure - Entity=Person, Data=[id=123 surname="Mario"] - SQLException stack trace....
```

which explains what you wanted to save (here a Person, a JPA entity) and the relevant contents of the Person instance. Please note the word `relevant`, instead of the word `all`: we should not clutter log files with useless info like the complete print of all entity fields. 

Entity name and its logical keys are usually enough to identify a record in a table.

## Rule 2. Match the logging levels with the execution environment

All logging facades and engines available in the Java ecosystem have the concept of logging level (ERROR, INFO...), with the possibility to filter out messages with a too low level.

For example, the Java Util Logging (JUL) engine that is included in the JDK, uses the following levels: SEVERE, WARN, INFO, FINE, FINER, FINEST (+ CONFIG and OFF).

On the contrary, the two most popular logging [facade](https://en.wikipedia.org/wiki/Facade_pattern), Apache Commons Logging and SLF4J, prefer the following levels: FATAL, ERROR, WARN, INFO, DEBUG, and TRACE.

Logging level filtering should depend on which stage of the development is your code: logging level in the production should not be the same as in test/integrations environments.

Moreover, the logging level should also depend on the code owner. In general, our application code should have more detailed logging compared to any third-party library we are using. There is usually no big meaning to see, for example, Google Guava library debug messages in our log files.

I usually configure the logging as follows:

* **Production**: INFO level for my code and WARN for third-party libraries.
* **Test/Integration**: DEBUG level for my code and WARN (or INFO if needed) for third-party libraries.
* **Development**: whatever makes sense

You should choose the logging strategy that better fit your requirements. 

Note: many application servers allow to change the logging configuration at runtime. In a production environment, you can temporarily enable DEBUG messages during a debug session and disable them when the session is finished.

I discourage the use of the TRACE/FINEST level (and I'm not alone, see for example [here](https://www.slf4j.org/faq.html#trace)). I don't see a big difference between DEBUG and TRACE and it is usually difficult for the young members of the team to decide which one, DEBUG or TRACE, to use. 

Following the [Kiss](https://en.wikipedia.org/wiki/KISS_principle) principle, I suggest using ERROR, WARN, INFO and DEBUG levels only.

## Rule 3. Remove coding help logging before the commit.

While coding, we usually add logging messages, using the logger or the System.out, in our code for a better understanding of what it is happening in our application during execution /debugging sessions.

Something like:

```java
    void aMethod(String aParam) {
        LOGGER.debug(“Enter in aMethod”);
        if (“no”.equals(aParam)) {
           LOGGER.debug(“User says no”);
          ….
```

The main purpose of these messages is to trace application behaviour by showing which method is invoked and by dumping internal variables and method parameter values. Quite popular among non [TDD](https://en.wikipedia.org/wiki/Test-driven_development) devotes.

Unfortunately, these messages do not have usually a big meaning once the code has been released (to test and then production).

So this rule simply says: once you have finished developing, remove all temporary and unnecessary logging messages just before committing the code to the SCM system (git, svn..) in use.

This rule does not require removing all DEBUG messages but only the ones that do not have meaning once the application is completed and released; in other words when we are reasonably sure that the application is working properly.

## Rule 4: Check log level before logging DEBUG messages

According to Rule 2, in the production log files, we will show ERROR, WARN and INFO messages only but in our code, we can have many DEBUG messages that should not affect production execution.

Every time you want to log a DEBUG message (all the ones which remain after rule 3), add in front a check if DEBUG logging is enabled:

```java
    if ( LOGGER.isDebugEnabled() ) {
        LOGGER.debug (…….)
    }
```

This will prevent your code to build the log messages and call the logger. It is for efficiency in the program execution at production.

## Rule 5: Know your logger

How we use the logger methods can have a significant cost:

* To build the message string
* to collect the data to be included in the message string

We should review the JavaDoc of the selected logging facade/engine and understand the most efficient way to use its logger.

For example, we could create a message like this:

```java
    LOGGER.info(“Person name is “ + person.getName());
```

which creates a few unnecessary string instances.

Using SLF4J, the correct use is :

```java
    LOGGER.info(“Person name is {}“, person.getName());
```

where the format string is constant and the final message is built only if logging is enabled. See [here](http://slf4j.org/faq.html#logging_performance) for more details.