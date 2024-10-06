---
tags: [coding, java, maven, plugins, versions, pom.xml, super pom, maven-enforcer-plugin]
---

This article is obsolete. You can find an updated and expanded version on [Medium](https://medium.com/@gualtierotesta/the-maven-enforcer-plugin-e45d68c0fa80).


*Last update: 16 Dec 2018*

The [Maven Enforcer Plugin](https://maven.apache.org/enforcer/maven-enforcer-plugin ) lets us define and, if required, enforce some environmental conditions like the:

* the Java version
* the Maven version
* the operating system type (windows, linux…)

If an environmental condition is *enforced*, the project build will **fail** if that condition is not respected.

In this post, we will focus on enforcing the Maven tool version but let’s begin with why we should enforce the Maven version.

Every Maven release has linked, in its [super pom](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#Super_POM), a specific set of plugin versions; changing the Maven version will also change the plugin versions unless the plugin is specified in the build→ plugins or in the build → pluginManagement sections in the pom.xml files.

In the Maven build process, several key plugins are involved: clean, compile, assembly, jar, war, ear, surefire… A different plugin version can potentially bring to different results.

If we are working in a team, with several developers with their own PC and local development environment, and/or if we are using a CI/CD pipeline, we can have:

1. Developer A use Maven version x to build the project
2. Developer B or the CI tool like Jenkins uses Maven version y to build the project

The two build outputs could be different because the Maven versions x and y could use a different version of the plugins involved in the build (if not explicitly set in the pom.xml file).

If we want to avoid this “hidden” dependency between the Maven version and the build results we can:

1. Specify the version of the most common/important plugins in the pom.xml file
2. Enforce the Maven tool version to be used to build the project; in our example, we could say: that everybody should use version y (or x).

How to enforce the Maven release? By using the Maven Enforcer Plugin add the following lines in the build → plugins section of the pom.xml:

``` xml title="pom.xml fragment" linenums="1"
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-enforcer-plugin</artifactId>
            <version>3.1.0</version>
            <executions>
                <execution>
                    <id>enforce-maven</id>
                    <goals>
                        <goal>enforce</goal>
                    </goals>
                    <configuration>
                        <rules>
                            <requireMavenVersion>
                                <version>[3.6.1,)</version>
                            </requireMavenVersion>
                        </rules>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

In this example, the `requireMavenVersion` **rule** has been specified. Within this rule, the Maven release can be specified as:

`<version>3.8.6</version>` : only version 3.8.6 can be used

`<version>[3.6.1,)</version>` : minimum accepted version is 3.6.1; 3.8.4 and 3.8.0 are ok, 3.5.1 is not valid

`<version>3.5</version>` : accepted versions are any 3.5.x

Another interesting rule is the `requirePluginVersions` which can help us to check a common Maven best practice: Maven plugins version should be explicitly defined in the pom.xml, to prevent build results variation when a different Maven tool release is used.

An example of the `requirePluginVersions` rule is the following:

``` xml title="pom.xml fragment" linenums="1"
<requirePluginVersions>
    <message>Best Practice is to always define plugin versions!</message>
    <banLatest>true</banLatest>
    <banSnapshots>true</banSnapshots>
    <phases>[ERROR] clean,deploy,site</phases>
</requirePluginVersions>
```

which forces the best practice to always define the version of the Maven plugins used in the project. The build will fail if the plugin version is not explicitly defined or it is a snapshot (`banSnapshots` tag) or latest (`banLatest` tag) version.

See [here](https://maven.apache.org/enforcer/enforcer-rules/requirePluginVersions.html) for all details about this rule configuration.

More usages can be found on the [plugin site](https://maven.apache.org/enforcer/maven-enforcer-plugin/usage.html).
