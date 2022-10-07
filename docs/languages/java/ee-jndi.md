---
tags: [java, ee, jndi]
---
# EE JNDI

*Last update: 7 Oct 2022*


### Resource

```java
@Resource(name="xxx")  resource;
@Resource(lookup="xxx")  resource;
@Resource(name="xxx",lookup="yyy")  resource;
```

The `@Resource` annotation:

* binds the field `resource` to the name `xxx` defined in the Environment Naming Context (ENC, `java:comp/env/xxx`) of the component; the name parameter refers to the JNDI name (`env-entry-name` in the `ejp-jar.xml` file)
* lookup the JNDI resource `yyy`

