# Learning Goals
After reading this article you should be able to answer the following questions:
- What are primitive data and reference types?
- How do the primitive reference types behave differently?
- What is the advantage and disadvantage of object by reference?
- What is types coercion and what's JS's basic rule? 
- What are expressions and statements?
- 
- What are literals?
- What are and when to use anonymous functions?
- What are methods?
- How use if-else and the switch statement different?

## Data Types
#### What are primitive data and reference types?
These six types are considered to be primitives. A primitive is not an object and has no methods of its own
- Boolean
- Null
- Undefined
- Number
- String
- Symbol (new in ECMAScript 6)

Everything else is an Reference type, which is an Object type. Below are some of the standard objects. Notice some of these were on the primitive list too, but don’t confuse them. These act as constructors to create those types. i.e Boolean('a') // true

There are two that are the main ones you’ll use for your own structures:
- Object
- Array

There are many other objects too just to list a few:
- Function
- Boolean
- Symbol
- Error
- Number
- String
- RegExp
- Math
- Set

#### How do the primitive reference types behave differently?
The way javascript works on the other hood: Primitive types and reference types differ in the way data is being stored and handled. When you use reference types, the data will not directly be created in the 'box' of a declared variable, but it will refer to the value. In contrast, for primitive types it will be directly sitting in the 'box' of a declared variable.
When it's in the box, it's in the stack. If it refers, it's located in the heap. As a result, primitive and reference types behave differently when you change the variables.

When you assign a variable to an existing reference type, you don't copy, but you refer to the same type. This is called *pass by reference*. If you would change one variable, you would change both. In contrast, when a parameter is *passed by value*, the caller and callee have two independent variables with the same value. If the callee modifies the parameter variable, the effect is not visible to the caller.

Example how it works when you change a reference type:
```js
let a = { name: "Kelly" };
let b = a;
b.name = "Dusty";

console.log(a.name) // prints Dusty because it's a reference type
```

Example how it works when you change a primitive type type:
```js
let a = 6;
let b = a;
b = 5;

console.log(a) // prints 6 because it is a primitive type
```
#### Advantage and disadvantage object by reference 
- Advantage. If you pass objects by reference (like JavaScript does), then you can think of objects are "real world things", that exists "outside of the place" you're currently computing. Objects get a kind of "permanence", and you can use them from different places in your code. This can be very useful, and it's the basis of a programming style known as Object Oriented Programming (OOP).
- Disadvantage. The disadvantage of passing objects by reference is that it can become a bit unpredictable what your data looks like. Other pieces of code might be changing it in ways you might not expect (or they might simply be at fault), and as a result your code might not work correctly. This way, the previously mentioned advantage can turn into a disadvantage.

## What is types coercion and what's JS's basic rule? 
Type coercion happens when a value doesn't have the right type in order to be a parameter for some operator in an expression. The JavaScript rules of the game are that basically everything is always coerced, whenever this is necessary for the operator. This means that when you write normal things like 4 + 5, everything goes as usual. But when you write "hello" + 5, then 5 is first coerced to "5".

## Truthiness / falseyness
One artifact of the "everything is always coerced when necessary" rule, is that every non-Boolean expression will implicitly be able to coerce into a Boolean value. Put differently: every expression is "truthy" or "falsey".

For example, the literal "joy" is just a string. But, if you use the string in a Boolean expression such as "joy" || 5 < 4, it will be coerced to a Boolean value, because the operator || only works on Boolean values. JavaScript will coerce "joy" to true, and subsequently "joy" || 5 < 4 will evaluate to true. So, we say that the value "joy" is truthy.

## What are expressions and statements?
### Expressions
Any unit of code that can be evaluated to a value is an expression. Expressions can be as long as you want them to be, but they would always result in a single value. When you combine elements like numbers, variables and strings, you will use expressions. The simplest example is the addition of two numbers:
```js
4 + 5
```
This is an "expression". In this case, it consists of:
- the addition operator (+)
- two literals, namely 4 and 5.

When expressions use the = operator to assign a value to a variable, it is called an *assignment expression*. 
```js
average = 55;
```

##### Arithmetic Expressions
Arithmetic expressions evaluate to a numeric value. Examples include the following:
```js
10;     // Here 10 is an expression that is evaluated to the numeric value 10 by the JS interpreter
10+13; // This is another expression that is evaluated to produce the numeric value 23
```

##### String Expressions
String expressions are expressions that evaluate to a string. Examples include the following:
```js
'hello';
'hello' + 'world'; // evaluates to the string 'hello world'
```
##### Primary Expressions:
Primary expressions refer to stand alone expressions such as literal values, certain keywords and variable values. Examples include the following
```js
'hello world'; // A string literal
23;            // A numeric literal
true;          // Boolean value true
sum;           // Value of variable sum
this;          // A keyword that evaluates to the current object
```

##### Left-hand-side Expressions:
Also known as lvalues, left-hand-side expressions are those that can appear on the left side of an assignment expression
```js
// variables such as i and total
i = 10;
total = 0;
// properties of objects
var obj = {}; // an empty object with no properties
obj.x = 10; // an assignment expression
// elements of arrays
array[0] = 20;
array[1] = 'hello';
// Invalid left-hand-side errors
++(a+1); // SyntaxError. Attempting to increment or decrement an expression that is not an lvalue will lead to errors.
```

##### Logical Expressions:
Expressions that evaluate to the boolean value true or false are considered to be logical expression.
```js
10 > 9;   // evaluates to boolean value true
10 < 20;  // evaluates to boolean value false
true;     //evaluates to boolean value true
a===20 && b===30; // evaluates to true or false based on the values of a and b
```

### Statements
A statement is an instruction to perform a specific action. Such actions include creating a variable or a function, looping through an array of elements, evaluating code based on a specific condition etc. JavaScript programs are actually a sequence of statements.

##### Declaration Statements:
Such type of statements create variables and functions by using the var and function statements respectively. Examples include
```js
var sum;
var average;
// In the following example, var total is the statement and total = 0 is an assignment expression
var total = 0;
// A function declaration statement 
function greet(message) {
  console.log(message);
}
```

##### Expression Statements:
Wherever JavaScript expects a statement, you can also write an expression. Such statements are referred to as expression statements.
```js
// In the following example, sum is an expression as it evaluates to the value held by sum but it can also pass off as a valid statement.
sum;
// An expression statement that evaluates an expression with side effects
b = 4+38;
```
##### Conditional Statements
Conditional statements execute statements based on the value of an expression. Examples of conditional statements includes the if..else and switch statements.

##### Loops and Jumps 
Looping statements includes the following statements: while, do/while, for and for/in. Jump statements are used to make the JavaScript interpreter jump to a specific location within the program. Examples of jump statements includes break, continue, return and throw.

## What are literals?
Literals, or literal expressions, are values that are writting down directly, without the use of variables or any other language feature (such as functions or more operators). Think of it this way: 9 is literally 9, but 4 + 5 not (you first have to compute it).

## What are operators?
Types of oeprators:
- Numerical operators
- Increment and decrement
- (In)equality
- Comparisons
- Boolean operators

## Functions
#### What are and when to use anonymous functions?
Functions are behavior. In order to call/invoke that behavior, we need to refer to that behavior. Usually we use names for that. So, what is an anonymous function? A function without a name. But then how do you refer to it?
```js
const someFunction = function(name) {
    console.log('Hello ' + name)
}
```

That's how. We declared a function the same way we did before, but we left out the name. This code illustrates two things: functions don't need names, and functions are values that can be assigned to variables.

Anonymous functions are useful when we want to pass them around like values. Passing them around can mean, assigning them to variables, passing them as arguments in function calls, and returning them as values inside function bodies.

#### What are methods?
Methods are functions that are properties on objects. An object has properties, as we know, and the property values can be any JavaScript value

## If-else vs. Switch
In an if-statement we have multiple (possibly unrelated expressions) that are all coerced to booleans:
```js
if (apples) {
    // ...
} else if (oranges) {
    // ...
} else if (apples === oranges) {
    // ...
}
```
In a switch statement there is only one expression, and its value is compared to the cases using strict (===) comparison. The data type of the expression and the cases is usually string or number, but can be anything.
```js
switch (10) { // the expression of type number
    case 1: 
        // no match
        break
    case "I will never equal the number 10":
        // no match
        break
    case true:
        // no match
    case 10:
        console.log("it was 10!")
        break;
}
```
### Technical terms
- **Evaluation** is actually just the technical term for "to compute". For instance, 4 + 5 evaluates to 9. Programmers like to say "evaluates" instead of "computes". For more talk on evaluation, check out the Advanced Topics section of this module.
