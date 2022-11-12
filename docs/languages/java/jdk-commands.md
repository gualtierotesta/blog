---
tags: [java, jdk, cli, tools, hotspot, j9, openj9]
---
# JDK command line tools

*Last update: 12 Nov 2022*

### HotSpot and J9 common commands

* **jar**         Creates the .jar files
* **jarsigner**   Signs the .jar files
* **java**        Starts thte JVM and execute the Java code in the application
* **javac**       Java compiler
* **javadoc**     Extracts the documentation info from the Java source files and produce the JavaDoc
* **javap**       Show the bytecode of a .class file
* **jconsole**    profiler, Java Monitoring and Management Console
* **jdb**         Debugger
* **jdeprscan**   DEPRecated Scanner, looks for deprecated code in .class and .jar files
* **jdeps**       Show the dependencies
* **jimage**      Handle the JIMAGE package
* **jjs**         Nashhorn JavaScript engine
* **jlink**       9+ Linker to create custom JRE
* **jmod**        Handles the JMOD package
* **jrunscript**  Generic command line script shell. By default, it uses JavaScript
* **jshell**      9+ Java Shell
* **keytool**     Handles the keystore
* **pack200**     Compress files
* **rmic**        RMI Compiler
* **rmid**        RMI Activation daemon
* **rmiregistry** RMI registry
* **serialver**   Shows the serial version UID
* **unpack200**   Decompress a pack2000 file

### HotSpot only commands

* **jaotc**       Ahead-of-time Compiler
* **jcmd**        JVM runtime diagnostic
* **jhsdb**       Core dump analyzer
* **jinfo**       Shows JVM runtime info
* **jmap**        Shows the runtime memory map (replaced by jcmd)
* **jps**         Lists all the applications running in a JVM
* **jstack**      Shows the Java threads stack trace
* **jstat**       Shows JVM statistics
* **jstatd**      Daemon version of jstat

### J9 only commands

* **jdmpview**    Analyse a core dump file
* **jextract**    Extract info from a core dump file
* **traceformat** Process the trace data

### Commands, libraries and projects once in the JDK and are now independent

* **Visual VM**           jvisualvm
* **Mission Control**     jmc
* **JavaFX**              javafxpackager, javapackager
* **CORBA**               idlj, orbd, servertool, tnameserv
* **JAXB**                schemagen, xjc
* **JAXWS/WSDL**          wsgen, wsimport

### Commands removed after Java 8

* **appletviewer**    Removed in 11
* **extcheck**        Removed in 9
* **javah**           Native-Header Generation Tool, Removed in 10
* **javaws**          WebStart, Removed in 11
* **jhat**            Heap visualizer, Removed in 9
* **jsadebugd**       Removed in 9
* **native2ascii**    Removed in 9. https://bugs.java.com/bugdatabase/view_bug.do?bug_id=JDK-8074431
* **policytool**      Removed in 10


