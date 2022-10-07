---
tags: [java, slf4j, logging]
---
# SLF4J

*Last update: 2 Oct 2022*

### Examples

How to define a logger

```java
private final static Logger logger = LoggerFactory.getLogger(<nome_classe>.class);
private final static XLogger logger = XLoggerFactory.getXLogger(<nome_classe>.class);
```

How to log to info and debug levels:

```java
logger.info("Message without parameters");

// no need to create an Object array
logger.info("Messaggio with many parameters {} {} {}", arg1, arg2, agr3,...);  
```

Note: avoid using the `trace` level. Prefer the `debug` level

How to log an exception

```java
logger.error("message", exception)
logger.error("message with an argument {}", arg1, exception)
```

Avoid:

```java
logger.error(e.getMessage(), exception);
logger.catching(exception);
```

