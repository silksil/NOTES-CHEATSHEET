# Learning Goals
Answer the following questions:
- What is functional programming and why would you use it?

# What is functional programming and why would you use it?
Functional programming is a programming paradigm, meaning that it is a way of thinking about software construction based on some fundamental, defining principles (listed above). Functional code tends to be:
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

## What are pure functions and why should you use them?
A mathematical function, or ‘pure function’:
- Returns something.
- Operates only on the supplied arguments.
- Has no ‘side effects’, thus also no mutations.
- Deterministic: on a given input, it will always retun the same thing.

Benefits:
- They are immediately testable. They will always produce the same result if you pass in the same arguments.
- They also makes maintaining and refactoring code much easier. You can change a pure function and not have to worry about unintended side effects messing up the entire application and ending up in debugging hell.

## What is declarative programming?
Functional programming is a declarative paradigm, meaning that the program logic is expressed without explicitly describing the flow control.

## What are Higher Order Functions?
Functional programming tends to reuse a common set of functional utilities to process data. Object oriented programming tends to colocate methods and data in objects. Those colocated methods can only operate on the type of data they were designed to operate on, and often only the data contained in that specific object instance. A higher order function is any function which takes a function as an argument, returns a function, or both.

## What means not having side effects?
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

### What does immutability mean?
Immutability is important to make sure one function does not change the original data rather than should return new copy of the data after manipulation.
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
