---
tags: [coding, java, maven, plugins, versions, pom.xml, super pom, maven-enforcer-plugin]
---

*Last update: 29 Nov 2024*

## Introduction

You can find an example of a project with this plugin [here](https://github.com/gualtierotesta/PlayWithJava).

The [Maven Enforcer Plugin](https://maven.apache.org/enforcer/maven-enforcer-plugin) is a valuable tool to be used in all Maven projects because, without this plugin, Maven is more permissive on how the project is defined, leading to possible issues or incoherence during the building process.

The Enforcer plugin:

* Checks that the project structure and definition, the `pom.xml` file(s), is correct.
* Guarantees that the project direct and transitive dependencies are not fighting to each other
* assures that the project builds are consistent and repeatable regardless who and what run them

The Enforcer plugin runs at each build and fails it whenever finds an error in the project.

The first step is to add the plugin in the project parent `pom.xml` file:

```xml
<build>
   <plugins>
     ... other plugins ...
     <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-enforcer-plugin</artifactId>
       <version>xxx</version>
       <executions>
         <execution>
           <id>enforce-maven</id>
           <goals>
             <goal>enforce</goal>
           </goals>
           <configuration>
             <rules>
               ... rules ...
             </rules>
           </configuration>
         </execution>
       </executions>
     </plugin>
   </plugins>
</build>
```

In the `rules` section, we add the rules we want to be checked by the Enforcer. The set of the built-in rules is documented [here](https://maven.apache.org/enforcer/enforcer-rules/index.html).

> We can define custom rules or use custom rules not created by the Enforcer plugin team. [Here is](https://maven.apache.org/enforcer/enforcer-api/writing-a-custom-rule.html) the link on how to create a custom rule.

If at least one of the rules fails, the Maven build fails with a message like the following:

```text
[ERROR] Rule 3: org.apache.maven.enforcer.rules.version.RequireMavenVersion failed with message:
[ERROR] Detected Maven Version: 3.6.3 is not in the allowed range [3.9,3.9].
```

The rules can be logically grouped based on their purpose. Let’s take a look at the most important ones, group by group.

## Rules to check the project structure

The purpose of the rules in this group is to check that the project is well-defined and complete. The following built-in rules belong to this group:

* reactorModuleConvergence
* banDuplicatePomDependencyVersions
* requiresFiles*

The Reactor is the Maven engine that manages the build of a multi-module project and, among other tasks, define the order in which each module should be built.

In a multi-module Maven project, the internal hierarchy can be complex, with internal dependencies between the modules: module A uses modules B and C and modules B uses module C so module C should be built before A and B and module B before A.

The [reactorModuleConvergence](https://maven.apache.org/enforcer/enforcer-rules/reactorModuleConvergence.html) rule checks that the project internal is consistent and converge to a unique and consistent build process. Moreover, the rule checks that all modules have a proper parent and with the right version.

```xml
<reactorModuleConvergence>
  <message>The reactor is not valid</message>
</reactorModuleConvergence>
```

Every multi-module project should use this rule.

On the contrary, the [banDuplicatePomDependencyVersions](https://maven.apache.org/enforcer/enforcer-rules/banDuplicatePomDependencyVersions.html) rule has a very simple job: check if there are duplicated dependencies. This check is also performed by several IDEs when editing the `pom.xml` file, but it is convenient to add it to the Enforcer checks.

```xml
<banDuplicatePomDependencyVersions/>
```

There are a set of rules which can check the presence, the absence or the size of files in the project. For example, the [requireFilesExist](https://maven.apache.org/enforcer/enforcer-rules/requireFilesExist.html) rule can be used to check if the specified files exist:

```xml
<requireFilesExist>
  <files>
    <file>README.md</file>
  </files>
</requireFilesExist>
```

In this example, the Maven build will fail if the project does not have a not-empty README.md file.

Similar rules are [requireFilesDontExist](https://maven.apache.org/enforcer/enforcer-rules/requireFilesDontExist.html) and [requireFileSize](https://maven.apache.org/enforcer/enforcer-rules/requireFilesSize.html).

**IMPORTANT**: In a multi-module project, the rules are applied to each module so, in the previous example, the file README will be searched on each module directory. If you want to check the presence of the file at the root of the project only, you have to specify an absolute path:

```xml
<requireFilesExist>
  <files>
    <file>${user.dir}/README.md</file>
  </files>
</requireFilesExist>
```

> ${user.dir} is the variable that defines the directory from which Maven was started. This variable represents the current working directory of the process running Maven.

My personal suggestion on how to use the rules in this group is the following:

```xml
<rules>
  ... other rules ...
  <reactorModuleConvergence>
    <message>The reactor is not valid</message>
  </reactorModuleConvergence>

  <banDuplicatePomDependencyVersions/>

  <requireFilesExist>
    <files>
        <file>${user.dir}/README.md</file>
        <file>${user.dir}/.editorconfig</file>
    </files>
  </requireFilesExist>
</rules>
```

Here, we enforce:

* Maven Reactor should be able to converge,
* no duplicated dependencies are allowed,
* A not-empty `README.md` file should exist at the project root directory
* A not-empty `.editorconfig` file should exist at the project root directory

## Rules to check the dependencies

The purpose of the rules in this group is to guarantee that the project has a valid set of dependencies. The following rules belong to this group:

* dependencyConvergence and requireUpperBoundDependecies
* requireReleaseDeps
* requireSameVersions
* requireExplicitDependencyScope

In Maven, the project dependencies can be direct, when we specify them in the pom.xml file, or transitive, dependencies that are direct dependencies of our dependencies. Please, be aware that this is a simplistic description of the complex dependencies resolution mechanism implemented in Maven. The reference document is here.

For example, if we have the following (direct) dependency in the project’s POM:

```xml 
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>xxx</version>
</dependency>
```

Maven will dynamically add, as project dependency, also the following (transitive) one:

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>xxx</version>
</dependency>
```

Because the `logback-classic` library requires the `slf4j-api` library.

The rule [dependencyConvergence](https://maven.apache.org/enforcer/enforcer-rules/dependencyConvergence.html) checks that all project dependencies, direct and transitives, result to a set of dependencies with a unique version. The rule will fail if a dependency is required in different versions by different dependencies; all dependencies should require the same version.

```xml
<dependencyConvergence />
```

This rule can easily fail, especially on popular libraries like SLF4J or Guava because many other libraries use them in different versions. It is sometime necessary to exclude some libraries because convergence is not difficult or impossible to achieve:

```xml
<dependencyConvergence>
  <excludes>
    <exclude>org.slf4j:slf4j-api</exclude>
  </excludes>
</dependencyConvergence>
```

The rule [requireUpperBoundDependecies](https://maven.apache.org/enforcer/enforcer-rules/requireUpperBoundDeps.html) is somehow less strict than the dependencyCovergence rule as it checks if all transitive dependencies are below or the same of the latest (upper) version required and not all to the same version.

The upped bound version of a dependency is defined in the dependencyManagement section of the project’s POM:

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>2.0.16</version>
    </dependency>
  </dependencies>
</dependencyManagement>
```

In this example, 2.0.16 is the upper bound version of the slf4j-api library. The requireUpperBoundDependencies rule fails if a direct or transitive dependency to this library refer to a version above 2.0.16.

It does make sense to use both the rules because the dependencyConverge rule says “let’s use the same version of the slf4j-api library in all project modules” while the requireUpperBoundDependecies rule says “and this version should be 2.0.16 or below”.

The [requireSameVersions](https://maven.apache.org/enforcer/enforcer-rules/requireSameVersions.html) rule is very similar to the dependencyConverge rule but with a different focus: check that “specific dependencies and/or plugins have the same version”. I don’t think it is necessary to use this rule together with dependencyConverge.

The [requireReleaseDeps](https://maven.apache.org/enforcer/enforcer-rules/requireReleaseDeps.html) rule checks the dependencies and fails if any snapshots are found. The snapshot versions are, by the definition, under development and so unstable. In order to have a repeatable and well define build process, we cannot use snapshots versions of third party libraries.

```xml
<requireReleaseDeps />
```

Another important rule is [banDynamicVersions](https://maven.apache.org/enforcer/enforcer-rules/banDynamicVersions.html) that checks if the version of any dependency version is determined at build time; all `SNAPSHOT`, `RELEASE` and `LATEST` versions are dynamically evaluated. At each build, Maven will check the repository to determine which is the exact version to be used. The effect is the build is no more repeatable; the results depend on the moment in time the build was run.

```xml
<banDynamicVersions />
```

My personal suggestion on how to use the rules in this group is the following:

```xml
<rules>
  ... other rules ...
  <dependencyConvergence />
  <requireUpperBoundDeps />
  <requireReleaseDeps />
  <banDynamicVersions />
</rules>
```

Rules to guarantee builds consistency and repeatability

The purpose of the rules in this group is to enforce Maven build consistency and repeatability. The following rules belong to this group:

* requireJavaVersion
* requireJavaVendor
* requireOS
* requireMavenVersion
* requirePluginVersions

The outcome of a Maven project build, for example the .jar file created in the target dir, depends on several factors:

1. The operating system (Linux, MacOs, Windows)
1. The Java Development Kit (OpenJDK, Temurin…)
1. The Java language version (8, 11, 21…) of the used JDK
1. The Maven tool
1. The Maven plugin.
1. The project source code

We usually assume that, if the code does not change (factor #6), the artifact is always and exactly the same regardless who in the team runs the build or how the pipeline has been set up.

**This is a wrong assumption.**

For example, changing the Maven version, several plugins change their default version (unless specified in the pom.xml) so, when we run:

```shell
mvn clean verify
```

Several Maven plugins are invoked (clean, compiler, surefire, jar, failsafe), each with its own version. Different plugin versions can bring different results.

To avoid this uncertainty in the build outcome, we can use Enforcer to control all the factors that can influence how the Maven build is executed.

The rule [requireJavaVersion](https://maven.apache.org/enforcer/enforcer-rules/requireJavaVersion.html) enforces the Java version used to build the project:

```xml
<requireJavaVersion>
    <version>[17]</version>
</requireJavaVersion>
```

In the above example, the project build requires a Java 17 compiler and will fail using, for example, a Java 21 compiler. Please note, Java version here refers to the language level supported by the JDK’s compiler, not the language level we use in our source code, which is defined by the `maven.compiler.release` property in the `pom.xml` file.

In this plugin, like in all others, numeric values can be also specified as range: see here for examples. This allows us to define a range of accepted values. In the example, only version 17 is accepted, but we could write `<version>17</version>` which means 17 or above.

With the same logic, but using strings and not numeric values, we can limit the vendor of the JDK used to build the project. The rule [requireJavaVendor](https://maven.apache.org/enforcer/enforcer-rules/requireJavaVendor.html) defines the allowed and forbidden JDK vendors.

We should also define the version of the Maven tool using the [requireMavenVersion](https://maven.apache.org/enforcer/enforcer-rules/requireMavenVersion.html) rule:

```xml
<requireMavenVersion>
  <version>[3.8.0,3.9.0)</version>
</requireMavenVersion>
```

The value here, `[3.8.0,3.9.0)`, means all versions from 3.8.0 to 3.8.x but not below or above 3.8. In other words, Maven 3.8.1 and 3.8.5 are accepted, but not the 3.6.3 or the 3.9.0 versions.

Due to the semantic versioning used in the Maven release process, all third digit releases are bug fixes only, so there should be no behavioral differences among them. In other words, it is safe to use any 3.8.x release because they will not create differences in the build outcome.

Again, with a similar approach, we can limit the build to a specific operating system version, family or architecture using the [requireOS](https://maven.apache.org/enforcer/enforcer-rules/requireOS.html) rule.

The last rule in this group is the [requirePluginVersions](https://maven.apache.org/enforcer/enforcer-rules/requirePluginVersions.html) rule that force us to specify the version of the plugins used in build process:

```xml
<requirePluginVersions>
  <banLatest>true</banLatest>
  <banSnapshots>true</banSnapshots>
  <banRelease>false</banRelease>
</requirePluginVersions>
```

These rule will fail if the basic Maven plugins do not have a version specified in the `pom.xml`file or if the version is “LATEST”, “RELEASE” or “SNAPSHOT”. A reliable and repeatable build cannot be based on plugins with a version not well-defined or not stable in time.

My personal suggestion is to limit Java and Maven versions and to specify all plugin versions:

```xml
<rules>
  ... other rules ...
  <requireMavenVersion>
    <version>[3.9.0,3.10.0)</version>
  </requireMavenVersion>
  <requireJavaVersion>
    <version>21</version>
  </requireJavaVersion>
  <requireJavaVendor>
    <includes>
      <include>Eclipse Adoptium</include>
    </includes>
  </requireJavaVendor>
  <requirePluginVersions>
    <banLatest>true</banLatest>
    <banSnapshots>true</banSnapshots>
    <banRelease>false</banRelease>
  </requirePluginVersions>
 </rules>
```

Here, we enforce:

* Maven release 3.9.x,
* Java 21 or above (note there are no []),
* Adoptium (Temurin) as JDK vendor
* basic plugins should have a version specified.

I usually don’t think it’s necessary to fix the OS version and the architecture, as I rely on the JDK vendor to be platform independent.

## Using the plugin from command line and in a CI pipeline

The approach used so far is working perfectly: we configure the plugin in the project POM, and then we run the enforcer.

An alternative approach, maybe more suited for a CI pipeline, is to provide the configuration on the command line, without the need to customize the project `pom.xml`. For example:

```shell
mvn enforcer:enforce -Denforcer.rules=reactorModuleConvergence,banDuplicatePomDependencyVersions,dependencyConvergence,requireReleaseDeps,banDuplicatePomDependencyVersions,requireUpperBoundDeps
```

Using the property `enforcer.rules` we pass to the enforcer plugin the list of the rules it should check. Note: on Maven command line, we use the -D option to specify a user property.

Unfortunately, we can provide the rule names only. If the rule requires a configuration like, for example, requireJavaVersion, we cannot use this approach.

## Conclusions

The Maven Enforcer Plugin is a valuable tool to ensure consistent and repeatable builds. It achieves this by enforcing rules on various aspects of a project’s definition and build environment.

Here’s a breakdown of the key points from the article:

Benefits:

* Ensures project structure and definition (pom.xml) are correct.
* Guarantees consistent and repeatable builds regardless of who runs them.
* Catches issues early in the build process.

How it Works:

* Added to the project’s parent pom.xml.
* Defines rules to be checked in the <rules> section.
* Runs at each build and fails if any rule is violated.
