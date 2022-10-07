---
tags: [java, vavr]
---
# VAVR

*Last update: 7 Oct 2022*


### LINK

* SITE:     [https://www.vavr.io/](https://www.vavr.io/)
* JAVADOC:  [https://www.javadoc.io/doc/io.vavr/vavr/0.10.3/index.html](https://www.javadoc.io/doc/io.vavr/vavr/0.10.3/index.html)
* MANUAL:   [https://www.vavr.io/vavr-docs/](https://www.vavr.io/vavr-docs/)


### EITHER

**Either.sequence**

Reduces many Eithers into a single Either by transforming an Iterable<Either<L, R>> into a Either<Seq<L>, Seq<R>>.

If any of the given Eithers is a Either.Left then sequence returns a Either.Left containing a non-empty Seq of all left values.

If none of the given Eithers is a Either.Left then sequence returns a Either.Right containing a (possibly empty) Seq of all right values. 

```java
Either<Integer, String> e1 = Either.right("ok1");
Either<Integer, String> e2 = Either.right("ok2");
Either<Integer, String> e3 = Either.left(3);
Either<Integer, String> e4 = Either.left(4);

Either<Seq<Integer>,Seq<String> e10 = Either.sequence(List.of(e1, e2)); // e10 = rigth(Seq("ok1", "ok2")

Either<Seq<Integer>,Seq<String> e11 = Either.sequence(List.of(e1, e2, e3, e4)); // e10 = left(Seq(3, 4)
```

**Either.sequenceRigth**

Works like `Either.sequence` if there are no left either.

If there is at least one left, it returns the first one, not all of them as `Either.sequence` does.
