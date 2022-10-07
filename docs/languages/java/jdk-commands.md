---
tags: [java, jdk, cli, tools, hotspot, j9, openj9]
---
# JDK command line tools

**TO BE TRANSLATED INTO ENGLISH**


### HotSpot and J9 common commands

    jar         Manipolazione file .jar
    jarsigner   Firma file .jar
    java        Esegue codice Java
    javac       Compila sorgenti Java
    javadoc     Processa file .java e ne produce il JavaDoc
    javap       Mostra il risultato della compilazione in byte code
    jconsole    profiler, Java Monitoring and Management Console
    jdb         Debugger
    jdeprscan   DEPRecated Scanner, cerca nei .class e .jar usi API deprecated
    jdeps       Mostra le dipendenze
    jimage      Gestisce il package JIMAGE, ottimizzato sulla piattaforma
    jjs         Nashhorn JavaScript engine
    jlink       9+ Linker per creare JRE custom basati sul moduli
    jmod        Gestisce il package JMOD, package modularizzati
    jrunscript  Generico command line script sheel. Di default usa JavaScript
    jshell      9+ Java Shell
    keytool     Gestisce il keystore
    pack200     Comprime file
    rmic        RMI Compiler
    rmid        RMI Activation demone
    rmiregistry RMI registry
    serialver   Mostra il serial version UID
    unpack200   Decomprime file

### HotSpot only commands

    jaotc       Ahead-of-time Compiler
    jcmd        JVM runtime diagnostic
    jhsdb       Core dump analyzer
    jinfo       Mostra informazioni sul runtime della JVM
    jmap        Mostra mappa informazioni sulla memoria a runtime, sostituita da jcmd
    jps         Elenca le applicazioni in esecuzione nella JVM
    jstack      Mostra stack trace dei Java threads
    jstat       MOstra statistiche sulla JVM
    jstatd      Versione demone di jstat

### J9 only commands

    jdmpview    Analizza il contenuto di un core dump
    jextract    Estrae informazion dal core dump    
    traceformat Processa trace data

### Commands, libraries and projects once in the JDK and now indipendent

    Visual VM           jvisualvm
    Mission Control     jmc)
    JavaFX              javafxpackager, javapackager)
    CORBA               idlj, orbd, servertool, tnameserv)
    JAXB                schemagen, xjc)
    JAXWS/WSDL          wsgen, wsimport

### Commands removed after Java 8

    appletviewer    Rimosso in 11
    extcheck        Rimosso in 9

    javah           Native-Header Generation Tool, rimosso in 10
    javaws          WebStart, rimosso in 11
    jhat            Heap visualizer, rimosso in 9
    jsadebugd       Rimosso in 9
    native2ascii    Rimosso in 9. https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8074431
    policytool      Rimosso in 10


