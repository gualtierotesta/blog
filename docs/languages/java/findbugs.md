---
tags: [java, findbugs, spotbugs, quality, checks]
---
# FindBugs & SpotBugs

**TO BE UPDATED**


* FindBugs 2.x  Java 5+ (?)  Last version: 2.0.3
* FindBugs 3.x  Java 7+      Last version: 3.0.1

Suppress annotation

```java
import edu.umd.cs.findbugs.annotations.SuppressFBWarnings;

@SuppressFBWarnings(value = { "DMI_EMPTY_DB_PASSWORD", "HARD_CODE_PASSWORD" }, justification = "Test")
@SuppressFBWarnings(value = { "NP_UNWRITTEN_FIELD", "UWF_UNWRITTEN_FIELD" }, justification = "UI components defined by Vaadin")
```

Dependency:

```xml
<dependency>
    <groupId>com.google.code.findbugs</groupId>
    <artifactId>annotations</artifactId>
    <version>3.0.0</version>
</dependency>
```