---
tags: [java, jdk, ibm, j9, openj9]
---
# OPEN J9

*Last update: 7 Oct 2022*

### Non heap memory

* JIT code cache   (def 256M su 64bit)
* JIT data cache   (def = Xmx ?)
* Class storage
* Misc non heap
    
### Heap memory

Global

    Initial memory (Xms)
    Max memory     (Xmx)

With the `gencon` garbage collector, the heap memory is divided in:
    
    Nursery / New
      initial     Xmns    default: 25% di Xms
      max         Xmnx    default: 25% di Xmx
            
    Tenure (Old)
      initial     Xmos    default: 75% di Xms
      max         Xmox    default: = Xmx
            
            
Note that maxTenure + maxNursery > maxHeap !!
        
### Using Docker/K8S

Set the max heap to the 70% of the container memory
    
Example with a container limited to 512 MB

    heap      = 356 MB
    nursery   =  90 MB
    tenure    = 448 MB
    non heap  =  64 MB (estimated)
       
    
OPTIONS

    -cp|classpath <class search path of directories and zip/jar files> A : separated list of directories, JAR archives, and ZIP archives to search for class files.

    -D<name>=<value>                  set a system property

    -verbose:[class|gc|jni]                   enable verbose output

    -version      print product version and exit
    -version:<value>                   require the specified version to run
    -showversion  print product version and continue

    -jre-restrict-search | -no-jre-restrict-search                   include/exclude user private JREs in the version search

    -? -help      print this help message

    -X            print help on non-standard options

    -ea|enableassertions[:<packagename>...|:<classname>]    enable assertions with specified granularity

    -da|disableassertions[:<packagename>...|:<classname>]   disable assertions with specified granularity

    -esa | -enablesystemassertions                   enable system assertions
    -dsa | -disablesystemassertions                  disable system assertions

    -agentlib:<libname>[=<options>] load native agent library <libname>, e.g. -agentlib:hprof see also, -agentlib:jdwp=help and -agentlib:hprof=help

    -agentpath:<pathname>[=<options>]   load native agent library by full pathname

    -javaagent:<jarpath>[=<options>]    load Java programming language agent, see java.lang.instrument

    -splash:<imagepath>
                  show splash screen with specified image


### Non standard options

```
java -X
The following options are non-standard and subject to change without notice.

  -Xcompressedrefs
  This command-line option enables the compressed references feature. When the JVM is launched with this command line option, 
  it would use 32-bit wide memory references to address the heap. This feature can be used up to a certain heap size (around 
  29GB depending on the platform), controlled by -Xmx parameter. 

  -Xnocompressedrefs
   These command-line options explicitly disable the compressed references feature. When the JVM is launches with this command 
   line option it uses full 64-bit wide memory references to address the heap. This option can be used by the user to override 
   the default enablement of pointer compression, if needed. 
 
  -Xbootclasspath:<path>    set bootstrap classpath to <path> 
  -Xbootclasspath/p:<path>  prepend <path> to bootstrap classpath 
  -Xbootclasspath/a:<path>  append <path> to bootstrap classpath 
 
  -Xrun<library>[:options]  load native agent library
                            (deprecated in favor of -agentlib)
 
  -Xshareclasses[:options]  Enable class data sharing (use help for details)
 
  -Xint           run interpreted only (equivalent to -Xnojit -Xnoaot)
  -Xnojit         disable the JIT
  -Xnoaot         do not run precompiled code
  -Xquickstart    improve startup time by delaying optimizations
  -Xfuture        enable strictest checks, anticipating future default
 
  -verbose[:(class|gcterse|gc|dynload|sizes|stack|debug)]
 
  -Xtrace[:option,...]  control tracing use -Xtrace:help for more details
 
  -Xcheck[:option[:...]]  control checking use -Xcheck:help for more details
 
  -Xhealthcenter  enable the Health Center agent
 
  -Xdiagnosticscollector enable the Diagnotics Collector
 
  -XshowSettings                show all settings and continue
  -XshowSettings:all            show all settings and continue
  -XshowSettings:vm             show all vm related settings and continue
  -XshowSettings:properties     show all property settings and continue
  -XshowSettings:locale         show all locale related settings and continue
 
Arguments to the following options are expressed in bytes.
Values suffixed with "k" (kilo) or "m" (mega) will be factored accordingly.
 
  -Xmca<x>        set RAM class segment increment to <x>
  -Xmco<x>        set ROM class segment increment to <x>

New Space
  -Xmn<x>         set initial/maximum new space size to <x>
  -Xmns<x>        set initial new space size to <x>
  -Xmnx<x>        set maximum new space size to <x>

Old space
  -Xmo<x>         set initial/maximum old space size to <x>
  -Xmos<x>        set initial old space size to <x>
  -Xmox<x>        set maximum old space size to <x>
  -Xmoi<x>        set old space increment to <x>

Heap Size
  -Xms<x>         set initial memory size to <x>
  -Xmx<x>         set memory maximum to <x>

Stack Size
  -Xmso<x>        set OS thread stack size to <x>
  -Xiss<x>        set initial java thread stack size to <x>
  -Xssi<x>        set java thread stack increment to <x>
  -Xss<x>         set maximum java thread stack size to <x>

  -Xmr<x>         set remembered set size to <x>
  -Xmrx<x>        set maximum size of remembered set to <x>
  -Xscmx<x>       set size of new shared class cache to <x>
  -Xscminaot<x>   set minimum shared classes cache space reserved for AOT data to <x>
  -Xscmaxaot<x>   set maximum shared classes cache space allowed for AOT data to <x>
  -Xmine<x>       set minimum size for heap expansion to <x>
  -Xmaxe<x>       set maximum size for heap expansion to <x>
 
  -Xlp                          enable large page support
  -Xrunjdwp:<options>           enable debug, JDWP standard options
  -Xjni:<options>               set JNI options 
``` 
  
### Garbage collector

```
-Xgcpolicy
The IBM virtual machine for Java provides four policies for garbage collection. Each policy provides unique benefits.
Note While each policy provides unique benefits, for WebSphere Application Server Version 8.0 and later, gencon is 
the default garbage collection policy. Previous versions of the application server specify that optthruput is the 
default garbage collection policy.

    gencon is the default policy. This policy works with the generational garbage collector. The generational scheme 
    attempts to achieve high throughput along with reduced garbage collection pause times. To accomplish this goal, 
    the heap is split into new and old segments. Long lived objects are promoted to the old space while short-lived 
    objects are garbage collected quickly in the new space. The gencon policy provides significant benefits for many 
    applications. However, it is not suited for all applications, and is typically more difficult to tune.

    optthruput provides high throughput but with longer garbage collection pause times. During a garbage collection, all 
    application threads are stopped for mark, sweep, and compaction, when compaction is needed.

    optavgpause is the policy that reduces garbage collection pause time by performing the mark and sweep phases of 
    garbage collection while an application is running. This policy causes a small performance impact to overall throughput.

    subpool is a policy that increases performance on multiprocessor systems, that commonly use more than 8 processors. 
    This policy is only available on IBM System i® System p and System z® processors. The subpool policy is similar to 
    the gencon policy except that the heap is divided into subpools that provide improved scalability for object allocation.

  -Xminf<x>       minimum percentage of heap free after GC
  -Xmaxf<x>       maximum percentage of heap free after GC
  -Xgcthreads<x>                set number of GC threads
  -Xnoclassgc                   disable dynamic class unloading
  -Xclassgc                     enable dynamic class unloading
  -Xalwaysclassgc               enable dynamic class unloading on every GC
  -Xnocompactexplicitgc         disable compaction on a system GC
  -Xcompactexplicitgc           enable compaction on every system GC
  -Xcompactgc                   enable compaction
  -Xnocompactgc                 disable compaction
```
