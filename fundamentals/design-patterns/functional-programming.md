# Intro
Functional programming is a programming paradigm, meaning that it is a way of thinking about software construction based on some fundamental, defining principles (listed above). Other examples of programming paradigms include object oriented programming and procedural programming.

Functional code tends to be:
- More concise
- More predictable
- And easier to test than imperative or object oriented coding.

Functional programming favors:
- Pure functions instead of shared state & side effects
- Immutability over mutable data
- Function composition over imperative flow control
- Lots of generic, reusable utilities that use higher order functions to act on many data types instead of methods that only operate on their colocated data
- Declarative rather than imperative code (what to do, rather than how to do it)
- Expressions over statements
- Containers & higher order functions over ad-hoc polymorphism

### Shared state
Shared state is any variable, object, or memory space that exists in a shared scope, or as the property of an object being passed between scopes. A shared scope can include global scope or closure scopes. Often, in object oriented programming, objects are shared between scopes by adding properties to other objects.

For example, a computer game might have a master game object, with characters and game items stored as properties owned by that object. Functional programming avoids shared state — instead relying on immutable data structures and pure calculations to derive new data from existing data. 

The problem with shared state is 1.) that in order to understand the effects of a function, you have to know the entire history of every shared variable that the function uses or affects. 2.) Another common problem associated with shared state is that changing the order in which functions are called can cause a cascade of failures because functions which act on shared state are timing dependent:

```javascript
// With shared state, the order in which function calls are made
// changes the result of the function calls.
const x = {
  val: 2
};

const x1 = () => x.val += 1;

const x2 = () => x.val *= 2;

x1();
x2();

console.log(x.val); // 6

// This example is exactly equivalent to the above, except...
const y = {
  val: 2
};

const y1 = () => y.val += 1;

const y2 = () => y.val *= 2;

// ...the order of the function calls is reversed...
y2();
y1();

// ... which changes the resulting value:
console.log(y.val); // 5
```

When you avoid shared state, the timing and order of function calls don’t change the result of calling the function. With pure functions, given the same input, you’ll always get the same output. This makes function calls completely independent of other function calls, which can radically simplify changes and refactoring
```javascript
const x = {
  val: 2
};

const x1 = x => Object.assign({}, x, { val: x.val + 1});

const x2 = x => Object.assign({}, x, { val: x.val * 2});

console.log(x1(x2(x)).val); // 5


const y = {
  val: 2
};

// Since there are no dependencies on outside variables,
// we don't need different functions to operate on different
// variables.

// this space intentionally left blank


// Because the functions don't mutate, you can call these
// functions as many times as you want, in any order, 
// without changing the result of other function calls.
x2(y);
x1(y);
```

### Side effects
```javascript
let meetup = {name:'JS',isActive:true,members:49};

const scheduleMeetup = (date, place) => {
  meetup.date = date;
  meetup.place = place;

  if (meetup.members < 50)
    meetup.isActive = false;
}

const publishMeetup = () => {
  if (meetup.isActive) {
    meetup.publish = true;
  }
}

scheduleMeetup('today','Bnagalore');
publishMeetup();
console.log(meetup);
```

The program is having side effect because the actual responsibility of function scheduleMeetup is to add the date and place of the meetup but it’s modifying the value of isActive as well on which some other function publishMeetup is depended upon and as a side effect the publishMeetup function will not have desired output because it’s input has changed in between. In the big program (real program), it’s really tough to debug the side effects. Side effects are not always bad but we should be careful on when to have it.

### Immutability
Immutability is important to make sure one function does not change the original data rather than should return new copy of the data after manipulation. 

In JavaScript, it’s important not to confuse const, with immutability. const creates a variable name binding which can’t be reassigned after creation. const does not create immutable objects. You can’t change the object that the binding refers to, but you can still change the properties of the object, which means that bindings created with const are mutable, not immutable.

Immutable objects can’t be changed at all. You can make a value truly immutable by deep freezing the object. JavaScript has a method that freezes an object one-level deep:
```javascript
const a = Object.freeze({
  foo: 'Hello',
  bar: 'world',
  baz: '!'
});

a.foo = 'Goodbye';
// Error: Cannot assign to read only property 'foo' of object Object
```

But frozen objects are only superficially immutable. For example, the following object is mutable:
```javascript
const a = Object.freeze({
  foo: { greeting: 'Hello' },
  bar: 'world',
  baz: '!'
});

a.foo.greeting = 'Goodbye';

console.log(`${ a.foo.greeting }, ${ a.bar }${a.baz}`);
```

###  Declarative vs Imperative
Functional programming is a declarative paradigm, meaning that the program logic is expressed without explicitly describing the flow control.

### Reusability Through Higher Order Functions
Functional programming tends to reuse a common set of functional utilities to process data. Object oriented programming tends to colocate methods and data in objects. Those colocated methods can only operate on the type of data they were designed to operate on, and often only the data contained in that specific object instance.

A higher order function is any function which takes a function as an argument, returns a function, or both.



Sources
1. https://codeburst.io/functional-programming-in-javascript-e57e7e28c0e5
2. https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0

