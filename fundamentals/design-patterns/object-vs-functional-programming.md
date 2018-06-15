# Introduction
- OOO makes code understandable by encapsulating moving parts.
- FP makes code understandable by minimizing moving parts.

The biggest difference between the two “schools of thought” concerns the **relationship between data**  and **operations on the data** . 

#### OOO is about grouping data with functions that govern that data
The central tenet of OOP is that data and the operations upon it are tightly coupled: An object owns its data and it owns the implementation of the operations on the data. It hides those from other objects via its interface, a collection of methods or messages it responds to. Thus, the central model for abstraction is the data itself, hidden as it is behind a small API in the form of its interface.

The central activity in OOP is composing new objects and extending existing objects by adding new methods to them.

Pure object oriented is usually stated to have four ingredients:

1. polymorphism: the ability of different classes to effectively all act the same in some way but implementation their own version of one or more functions or operations to operate in an equivalent way
2. encapsulation:  It describes the idea of bundling data and methods that work on that data within one unit, e.g., a class in Java.
3. inheritance: classes or types can inherit or ‘sublcass’ from a ‘superclass’.  This enables both reuse of code, and is a key tool for building polymorphism when more than one class subclasses from the same parent has its own implementation of the methods of the superclass
4. abstraction: this is giving the class an interface designed around the logical use of the class, and not simply a reflection of the mechanics of the underlying code


####  FP tries to minimize state by using pure functions as much as possible.
A mathematical function, or ‘pure function’ operates on the supplied arguments and returns a result and does nothing else. No ‘side effects’. Nothing changed by the function, no internal variables altered that will result a future call of the same function dealing with different values.

Sources:
https://medium.com/@darrickmckirnan/object-oriented-programming-oop-functional-programming-what-are-they-the-pros-and-cons-11f98a971e38
