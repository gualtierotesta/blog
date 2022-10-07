---
tags: [java, gradle, build]
---
# Gradle

*Last update: 7 Oct 2022*

### Best practice

1. usare sempre il wrapper (da creare all'inizio del progetto)
2. usare il daemon se si compila in locale


### Commands

Create the wrapper

    gradle wrapper --gradle-version=xxx

Show the versionMostra versione

    gradlew -v

Updates the wrapper

    gradlew wrapper --gradle-version x.x.x

