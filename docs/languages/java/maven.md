---
tags: [java, maven, build]
---
# APACHE MAVEN

*Last update: 7 Oct 2022*

### Commands

Download the used dependencies' JavaDoc

      mvn dependency:resolve -Dclassifier=javadoc

Download the used dependencies' source code

      mvn dependency:resolve -Dclassifier=sources

Analyse the project dependencies reporting the missing and the unused

      mvn dependency:analyze

Show all dependencies

      mvn dependency:resolve
      mvn dependency:tree

Runs Jetty

      mvn package jetty:run -Dmaven.test.skip=true   -DskipTests

Create local dependency

      mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=10.2.0.2.0 -Dpackaging=jar -Dfile=ojdbc.jar -DgeneratePom=true


Run a main class

      mvn exec:java -Dexec.mainClass="com.example.Main"


Release

      mvn release:prepare -DdryRun=true -Darguments="-DskipTests"
      mvn release:perform
      mvn release:clean

      mvn release:update-versions -DautoVersionSubmodules=true
      mvn --batch-mode release:update-versions -DdevelopmentVersion=1.2.0-SNAPSHOT

Use a specific plugin version

      mvn groupID:artifactID:version:goal
      mvn org.apache.maven.plugins:maven-deploy-plugin:2.8.2:deploy

Show the maven configuration (from the `.settings.xml` file)

      mvn help:effective-settings

System dependency
```xml
      <dependency>
         <groupId>ldapjdk</groupId>
         <artifactId>ldapjdk</artifactId>
         <scope>system</scope>
         <version>1.0</version>
         <systemPath>${basedir}\src\lib\ldapjdk.jar</systemPath>
      </dependency>
```

### Passwords

Read [instructions](https://maven.apache.org/guides/mini/guide-encryption.html)

      mvn --encrypt-master-password <password>
      mvn --encrypt-password <password>
