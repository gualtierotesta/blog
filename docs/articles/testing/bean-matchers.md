---
tags:
- code checks
- coding
- data class
- immutable
- java
- junit
- maven
- quality checks
- unit test
- unit testing
---

# POJO and JavaBean testing using Bean-matchers 

*Last update: 04 Sep 2022*

[Project repo](https://github.com/gualtierotesta/blog-projects/tree/master/pojo-testing)

The first reaction to the question “Should I unit test a data class?”  is usually a big NO because there is no meaning to checking a data class that contains boilerplate code (getter, setter, equals..) often generated using the  IDE or by libraries like Lombok and Immutables.

Real-world projects are, unfortunately, not so perfect and there are cases where unit testing a POJO or a JavaBean could make sense. Let me list some situations I have encountered.

## Security sensitive fields

If the data class contains a security-sensitive field like a password or an API token, this field should NOT be logged.

See what OWASP has to say about this problem: [Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html#data-to-exclude).

For this reason, the class should not include sensitive fields in the toString method.

## Logging unfriendly fields

The data class can contain fields that are binary data or very long strings. In this case, we should exclude these fields from the toString method to prevent them to bloat our logging files.

## Non-standard equals and hashCode methods

The standard way to implement an equals method is to compare, one by one, all fields in the class. Sometimes one field contains a “unique” identifier that makes other field comparisons useless or even dangerous.

For example, this unique field can contain the primary key column of the database record from which the POJO data has been extracted. In this case, the POJO equality could or should depend only on its primary key field and the equals (and hashCode) implementation should use just this field.

## No args constructor (JavaBeans)

The JavaBean standard requires the presence of a no-args constructor, a constructor without parameters.

The compiler usually creates the no-args constructor unless there are already other (non no-args) constructors defined in the class.

At runtime, the library (for example, the JPA libraries) which rely on the no-args constructor presence will fail due to the missing no-args constructor.

One important remark: checking all the above conditions using a unit test is a way to guarantee the correct behaviour of the data class throughout all project life. The class can be correctly implemented when created but, later, one developer, while adding a new field in the class, can wrongly regenerate the toString method, restore the log of a password field, or forget to regenerate the equals/hashCode methods properly.

Checking data class quality is also useful in legacy projects to detect improper class definitions because the unit class can be added aside from the “main” classes, without disturbing the legacy code.

## The Bean-matchers library

The [Bean-matchers library](https://github.com/orien/bean-matchers), created by Orien Madgwick, lets us test the class conditions to guarantee that all future changes in the class will not break them.

Let’s assume we have the following data class:

```java
class BasicBean {
  private int id;
  private String string;
  private char[] password;
  private Long[] longArray;
  private String veryLongString;

  public BasicBean(final int id) {
    this.id = id;
  }

   // getter, setter, equals, hashCode, toString

```

Conditions we want to assure:

* the id field should be used to check equality and to generate the class hash
* the password field should not be logged because security sensible
* the veryLongString field should not be logged to avoid logging files bloating
* the class should have a no-args constructor to be used with JPA.

Using the Bean-matchers library, the test class can be:

```java
@Test
public void testTheClassIsGoodJavaBean() {
  MatcherAssert.assertThat(BasicBean.class,
    CoreMatchers.allOf(
      // This is Java Bean so we want an empty constructor
      BeanMatchers.hasValidBeanConstructor(),
      // All fields should have getter and setter
      BeanMatchers.hasValidGettersAndSetters(),
      // Only the 'id' field in hashcode and equals
      BeanMatchers.hasValidBeanHashCodeFor("id"),
      BeanMatchers.hasValidBeanEqualsFor("id"),
      // Password and veryLongString fields should not 
      // be included in the toString method
      BeanMatchers.hasValidBeanToStringExcluding(
          "password", "veryLongString")
    ));
}
```

The Beans-matchers library creates an instance of our BasicBean class and checks if all conditions we have defined on

* the constructor
* the getters and setters,
* the equals method
* the hashCode method
* the toString method

are valid otherwise, the test fails.

A test failure will alert us whenever a change in the BasicBean class breaks the above conditions. It is a kind of safety net.

## For Lombok users

Using Lombok, we can implement our bean conditions using Lombok annotations. The following code is the equivalent version of the plain Java version reported above:

```java
@Data
@NoArgsConstructor
@EqualsAndHashCode(of = "id")
@ToString(exclude = {"password", "veryLongString"})
public class LombokBean {
  private int id;
  private String string;
  private char[] password;
  private Long[] longArray;
  private String veryLongString;

  public LombokBean(final int id) {
    this.id = id;
  }
}
```

When using Lombok, I believe there is no need to have a unit test to check our bean conditions because, in general, we should not test third-party libraries (Lombok in this case), but having it will not damage your project ;-)

## Final remarks

Another possible benefit of testing data classes, especially in legacy projects, is that their code coverage is easily 100%. This should not be the main purpose (remember, there is no meaning to test getter and setter) but a side effect which I’ve found useful: having the data classes at 100%, the low test coverage is due to the logic classes, the classes which contain the code which implement the application logic. They should be the main target of any test!