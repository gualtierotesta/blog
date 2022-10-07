---
tags: [java, spring, boot, actuactor]
---
# Spring Boot

*Last update: 7 Oct 2022*

### Actuactor

Dependency: `spring-boot-starter-actuactor`

Endpoints:

* autoconfig
* info
* configprops
* dump
* metrics
* health
* heapdump
* mappings
* beans
* trace
* env


### Configuration and Spring profiles

The files `application.properties` and `application.yml` contains the parameters to configure Spring Boot and the Spring components.

The JVM variable (option `-D`) `spring.profiles.active` mark which is the active Spring profile. The profile defines which properties file should be read. The main file (application.*) is always read. Then, the file `application-<profile>.*` is read.

We can create a data class with a @Component annotation with the @ConfigurationProperties which collect a set of configuration parameters.

		Environment variable: SPRING_PROFILES_ACTIVE
		JVM variable:         spring.profiles.active


### Logging

The spring boot started dependencies like starter-web depends on the starter-logging that uses SLF4J and LogBack and the redirectors from CommonsLogging, JUL and Log4J.




