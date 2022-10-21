---
tags: [groovy, java, maven, build, plugin]
---
# The GMavenPlus plugin

*Last update: 21 Oct 2022*

The Maven plugin GMavenPlus allows you to build Groovy and mixed Java-Groovy projects. The plugin integrates itself into the Maven lifecycle, compiling the Groovy code and creating the stub code where required.

As the first step, you should add the Groovy dependency in the project `pom.xml`:

```xml
<dependency>
    <groupId>org.apache.groovy</groupId>
    <artifactId>groovy</artifactId>
    <version>${groovy.version}</version>
</dependency>
```

**Remember** that starting from the 4.0 release, Groovy groupId is `org.apache.groovy` and not anymore `org.codehaus.groovy`.

The second step is to specify the GMavenPLus plugin in the `pom.xml` build plugins:

```xml
<plugin>
    <groupId>org.codehaus.gmavenplus</groupId>
    <artifactId>gmavenplus-plugin</artifactId>
    <version>${gmavenplus-plugin.version}</version>
    <executions>
        <execution>
            <goals>
                <goal>addSources</goal>
                <goal>addTestSources</goal>
                <goal>generateStubs</goal>
                <goal>compile</goal>
                <goal>generateTestStubs</goal>
                <goal>compileTests</goal>
                <goal>removeStubs</goal>
                <goal>removeTestStubs</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <invokeDynamic>true</invokeDynamic>
    </configuration>
</plugin>
```

The final step is to add your Groovy files in the `src/main/groovy` dir. The Java files should be placed as usual in the `src/main/java` dir.

See a complete example on my [GitHub repo](https://github.com/gualtierotesta/PlayWithGroovy/tree/master/mixed).


### Links

* [Project example](https://github.com/gualtierotesta/PlayWithGroovy/tree/master/mixed)
* [Plugin repository on GitHub](https://github.com/groovy/GMavenPlus)
* [Plugin Home Page](https://groovy.github.io/GMavenPlus)
