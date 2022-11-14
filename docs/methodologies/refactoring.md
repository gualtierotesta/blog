---
tags: [software, refactoring]
---
# Refactoring

*Last update: 14 Nov 2022*

Links:

* [Refactoring by Martin Fowler (book)](https://www.goodreads.com/book/show/35135772-refactoring)


## Bad Smells or what we should avoid

* Duplicated code
* Too long methods
* Complex conditional statements
* Primitive obsession, when we use int and strings instead of value objects
* Indecent exposure, when we expose local methods and classes
* Solution sprawl is a solution distributed across different classes
* Alternative classes with different interfaces, when the same classes implement different interfaces and not just one
* Lazy classes: classes that do nothing
* Large (God) classes: classes that do too much
* Switch statements, when we use switches instead of proper polymorphism
* Combinatorial explosions, when the logic conditions are too complex
* Oddball solutions: different solutions to the same problem


## Compose methods (from Fowler's book)

* Extract Method (110) - move a code block to a separate method
* Inline Method (117) - replace a call to a method with the code of called method
* Inline Temp (119) - replace a temporary variable with its value
* Replace Temp With Query (120) - replace a temporary variable with a method call
* Introduce explaining variable (124) - use a temporary variable to better clarify code purpose
* Split temporary variable (128) - when a temporary variable is used more than one time
* Remove Assignment to Parameters (131)
* Replace Method with Method Object (135) - Replace a complex method with an object with a method
* Substitute algorithm (139) - Replace one algorithm with a better one

## Moving Features between Objects (from Fowler's book)

* Move method (142) - from one class to another one, where it is used most
* Move field (146) - from one class to another one, where it is used most
* Extract class (149) - when a class does too much (too many responsibilities)
* Inline class (154) - when a class does nothing
* Hide delegate (157) - introduce delegation methods to hide internal dependencies
* Remove Middle Man (160) - remove a class which delegates only
* Introduce foreign method (162) - when we want to add a method to an unmodifiable class
* Introduce local extension (164) - when we want to add many methods to an unmodifiable class


## Organizing Data (from Fowler's book)

* Self Encapsulate Field (171) - use the accessors method to access a field
* Replace data value with an object (175) - Replace a value with an object to add functionality
* Change value with reference (179) - change a value object to an object with identity
* Change reference to Value (183) - change an object with identity to a value object
* Replace array with Object (186) - when the array values have a different meaning
* Duplicate observed data (189) - split data from UI logic
* Change unidirectional association to Bidirectional (197) - make bidirectional the relation between two classes
* Change bidirectional association to unidirectional (200) - make unidirectional the relation between two classes
* Replace Magic number with symbolic constant (204) - create constants for relevant numbers in the code
* Encapsulate field (206) - replace a public field with a private field with accessor methods
* Encapsulate collections (208) - Replace accessor methods for collections fields with add and remove methods
* Replace record with data class (217) - Create an interface class with accessors for the record fields
* Replace type code with class (218) - Replace integer or string constants with classes or enums

