# Learning Goals
Answer the following questions:
- What is OPP?
- Which 4 ingredients does pure OPP and explain them? 

# What is OPP?
With OPP you organize code, namely by grouping. The central tenet of OOP is that data and the operations upon it are tightly coupled: An object owns its data and it owns the implementation of the operations on the data. It hides those from other objects via its interface, a collection of methods or messages it responds to. Thus, the central model for abstraction is the data itself, hidden as it is behind a small API in the form of its interface. The central activity in OOP is composing new objects and extending existing objects by adding new methods to them.

Pure object oriented is usually stated to have four ingredients:
1. *polymorphism:* the ability of different classes to effectively all act the same in some way but implementation their own version of one or more functions or operations to operate in an equivalent way
2. *encapsulation:*  It describes the idea of bundling data and methods that work on that data within one unit, e.g., a class in Java.
3. *inheritance:* classes or types can inherit or ‘sublcass’ from a ‘superclass’.  This enables both reuse of code, and is a key tool for building polymorphism when more than one class subclasses from the same parent has its own implementation of the methods of the superclass
4. *abstraction:* this is giving the class an interface designed around the logical use of the class, and not simply a reflection of the mechanics of the underlying code

# Terminology
To write OPP you should know the following terminology:
- *Object* : to group data.
- *Class*: to group functions. It allows the instances to have access to the same blueprint i.e. the same methods. ???
- *Methods* are functions that are put into classes, you can only add methods to a class.
- *Constructor* is used to access arguments that are being passed to that constructor if you create a new instance. Typically you use the constructor to pair the arguments to certain variables. The constructor itself is a method.
- To refer to an instance in an constructor you use `.this`.
