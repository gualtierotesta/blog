---
tags: [java, exceptions, best practice, antipattern]
---
# Exceptions in Java

*Last update: 12 Nov 2022*

References and credits:

* [book: Effective Java](https://www.goodreads.com/book/show/105099.Effective_Java_)
* [post: Top 20 Java Exception Handling Best Practices](https://howtodoinjava.com/best-practices/java-exception-handling-best-practices/)

In Java, the exceptions are divided into checked, unchecked and errors.

### Checked Exceptions

The checked exceptions are exceptions that must be declared in the throws clause of a method.

They extend Exception and are intended to be an "in your face" type of exception.

A checked exception indicates an expected problem that can occur during normal system operation. 

Some examples are problems communicating with external systems and problems with user input. Note that, depending on your code's intended function, "user input" may refer to a user interface, or it may refer to the parameters that another developer passes to your API. 

Often, the correct response to a checked exception is to try again later or to prompt the user to modify his input.

### Unchecked Exceptions

The unchecked exceptions are exceptions that do not need to be declared in a throws clause. They extend RuntimeException.

An unchecked exception indicates an unexpected problem that is probably due to a bug in the code. The most common example is the **NullPointerException**.

There are many core exceptions in the JDK that are checked exceptions but really shouldn't be, such as IllegalAccessException and NoSuchMethodException.

An unchecked exception probably shouldn't be retried, and the correct response is usually to do nothing and let it bubble up out of your method and through the execution stack. This is why it doesn't need to be declared in a throws clause. 

Eventually, at a high level of execution, the exception should probably be logged (see below).

### Errors

Errors are serious problems that are almost certainly not recoverable.

Some examples are OutOfMemoryError, LinkageError, and StackOverflowError.


### UI

When the application user is a human (i.e. we have a UI), we have several options when an internal exception is thrown.

Note: validation errors due to invalid form fields are not exceptions.


**Solution 1**

Show the exception stack trace to the user who will hardly understand.
The application remains in an undefined state.

**Solution 2**

Hide the exception to the user. The application seems working properly but the exception affected its behaviour.
An example: the user press a button, the logic behind it raises an exception and does not complete its function. The user, not seeing any error, does not understand what is happening.

**Solution 3**

The application shows a human-understandable error message that explains a problem has occurred and the requested function cannot be executed.
The error message can contain a unique error identifier that can be used to track the error in the log files.
After showing the message, the application returns to a well-defined status.
This is the preferred solution.


### Effective Java

Joshua Bloch's "Effective Java, 3rd edition" book contains an entire chapter, the 10th, on exceptions.

This chapter contains several items:

* Item 69: use exceptions only for exceptional conditions
* Item 70: use checked exceptions for recoverable conditions and runtime exceptions for programming errors
* Item 71: avoid unnecessary use of checked exceptions
* Item 72: Favor the use of standard exceptions
* Item 73: Throw exceptions appropriate to the abstraction
* Item 74: Document all exceptions thrown by each method
* Item 75: Include failure-capture information in detailed messages
* Item 76: Strive for failure atomicity
* Item 77: Don't ignore exceptions

From the above items, we can derive the following *rules*:

1. Do not use exceptions to handle expected situations.
1. Do not throw Error
1. Do not use the exception message to analyze the cause of the problem. Use custom fields to deliver context information.
1. Do not throw checked exceptions if the caller(s) cannot handle them. In this case, throw an unchecked exception.
1. Use standard JDK exceptions when possible.
1. Do not rethrow layer specific exceptions. For example, do not rethrow SQLException from a data layer. Map the exception to an application-oriented exception.

