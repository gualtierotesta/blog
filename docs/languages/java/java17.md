---
tags: [java, 17, jdk]
---
# JAVA 17

*Last update: 7 Oct 2022*

### Record

* immutable data class
* no setter
* the getters have the xxx form (not getXxx())
* equals, hashCode and toString are automatically defined
* no instance fields
* static fields are allowed
* can be defined inside other records, classes and methods (like named tuples)

```java
record MyFirstRecord(int c, String S) {}
```    

A record can have a simplified constructor useful to add assertions and validations:

```java
record MyFirstRecord(int c, String S) {
    MyFirstRecord() {
        // c and S assigned automatically
        if (c < 42) throw new IllegalArgumentException();
    }
}
```    
    
### Sealed class

A sealed class or interface restricts which classes or interfaces can extend or implement it. 


### Pattern matching instanceOf

No need to cast the value before using it.

Before Java 17

```java
if (shape instanceof Rectangle) {
    Rectangle r = (Rectangle) shape; // CAST
    return 2 * r.length() + 2 * r.width();
}
```

Java 17

```java
if (shape instanceof Rectangle r) {
    return 2 * r.length() + 2 * r.width();
}
```


### Text blocks (15+)

Strings defined on multiple lines without escaping.

```java
String html = """
········    <html>
········        <body>
········            <p>Hello World.</p>
········        </body>
········    </html>
········""";
```

The 3 `"` open and close the text block and they also define the indention (shown with the dots).
    
    

### Switch expression

The switch can be also used as expression, returning a value.

Unlike switch statements, the cases of switch expressions must be exhaustive, which means that for all possible values.

The `yield` statement does not require the `break` statement.

Syntax 1 (recommended)

```java
val result = switch(label) {
    case label1 -> value1;
    case label2 -> {
        ....
        yield value2;
        }
    case label3 -> throw new Exception()
    default -> ...
}
```

Syntax 2

```java
val result = switch(label) {
    case label1 : 
        yield value1;
    case label2:
        ....
        yield value2;
    case label3: throw new Exception()
    default: ...
}
```
