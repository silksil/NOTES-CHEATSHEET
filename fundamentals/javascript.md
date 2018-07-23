
## Instances, Conctructors and Classes
### Instances
"instance" is best understood as it relates to "class" in programming. "Classes" are used to define the properties and behavior of a category of things. E.g. A "Car" class might dictate that all cars be defined by their make, model, year, and mileage.
But you cant provide specifics about a particular car (for example, that 1978 Chevy Impala with 205,000 miles on it that your uncle Mickey drives) until you create an "instance" of a Car. It's the instance that captures the detailed information about one particular Car.

### Constructor Functions (pre-ES6) and the `new` keyword
In the example below, instead of using object literals,  we use a function in conjunction with the `new` keyword to create objects. Such a function is called a **constructor** function, and, by convention, we start its name with an uppercase letter.

When a function is called and preceded by the `new` keyword, something special happens. The JavaScript engine creates a new, empty object and assigns that object to the `this` variable.

> The `this` variable is always present in JavaScript. Its value is dependent on the current execution context. Most of the time, the value of `this` is `undefined`. However, when calling a method on an object, the `this` variable holds a reference to the object it is called on. We can use the `this` variable inside the method implementation to get at other properties and methods within the object.

We can now add properties to the new object through the `this` variable, as shown below.

When the constructor function finishes, it returns the newly constructed object as its return value.

```js
function Month(name, days) {
  this.name = name;
  this.days = days;
}

const months = [
  new Month('January', 31),
  new Month('February', 28),
  new Month('March', 31),
  new Month('April', 30),
  new Month('May', 31),
  new Month('June', 30),
  new Month('July', 31),
  new Month('August', 31),
  new Month('September', 30),
  new Month('October', 31),
  new Month('November', 30),
  new Month('December', 31)
];

months
  .filter(month => month.days === 31)
  .map(month => `${month.name} has ${month.days} days.`)
  .forEach(string => console.log(string));
```

### ES6 Classes
In ES6 a new way of defining objects and its methods was introduced. It uses the same prototype mechanism behind the scenes, but its syntax is closer to that of other object-oriented languages, such as Java, etc. Because it is only new syntax, hiding the intricacies of the prototype, it is often designated as 'syntactic sugaring'.

In ES6 classes we use the class keyword to define a class. The constructor method takes the place of the constructor function of the previous examples.

We define methods by creating functions inside the class body, however without the function keyword. As previously, the this keyword refers to the object that a method is called upon.

```js
class Month {
  constructor(name, days) {
    this.name = name;
    this.days = days;
  }

  hasDays(days) {
    return this.days === days;
  }

  isLongMonth() {
    return this.hasDays(31);
  }

  toString() {
    return `${this.name} has ${this.days} days.`;
  }

  toConsole() {
    console.log(this.toString());
  }
}

const months = [
  new Month('January', 31),
  new Month('February', 28),
  new Month('March', 31),
  new Month('April', 30),
  new Month('May', 31),
  new Month('June', 30),
  new Month('July', 31),
  new Month('August', 31),
  new Month('September', 30),
  new Month('October', 31),
  new Month('November', 30),
  new Month('December', 31)
];

months
  .filter(month => month.isLongMonth())
  .forEach(month => month.toConsole());
  ```
##### Sources: 
- https://github.com/HackYourFuture/fundamentals/blob/master/fundamentals/oop_classes.md
- https://stackoverflow.com/questions/20461907/what-is-meaning-of-instance-in-programming?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa

## Deconstructing an object
#### The difference between property, key and value
The term property (also: attribute, less common or even used for different things in JS) usually refers to the key/value pair that describes a member of an object. While, especially when used with a specific identifier (key), it often refers to the whole combination, it can also denote the value of that member. It does usually not mean the identifier itself.

When people try to be accurate, they distinguish between "property" (the whole thing, part of an object), "property name" (the string used as the key) and "property value" (the stored data).

##### Source:
- https://stackoverflow.com/questions/28648090/properties-vs-keys-vs-values-in-javascript?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa

## Functions
### Higher-order functions
A higher order function is a function that takes one or more functions as arguments. It allows us to put small functions in a big function. 

```javascript
var animals = [
  { name: ‘Waffles’, type: ‘dog’, age: 12 },
  { name: ‘Fluffy’, type: ‘cat’, age: 14 },
  { name: ‘Spelunky’, type: ‘dog’, age: 4 },
  { name: ‘Hank’, type: ‘dog’, age: 11 },
];
var oldDogs = animals.filter(function(animal) {
  return animal.age > 10 && animal.type === ‘dog’;
});
// oldDogs will now be an array that contain 
// only Waffles and Hank objects.
```

filter in the above example is a so-called higher-order function. This is a fancy word for a function that accepts another function as an argument. In the above example, the function passed to filter will be called once with each item in the animals array as the argument. This passed function is sometimes referred to as the callback. If the callback returns true, the items makes the cut for the new array that filter is creating, which is what ends up in the oldDogs variable.

##### Source: 
- https://medium.com/humans-create-software/a-dirt-simple-introduction-to-higher-order-functions-in-javascript-b33bf9e19056

### Pure functions
The definition of a pure function is:
- The function always returns the same result if the same arguments are passed in. It does not depend on any state, or data, change during a program’s execution. It must only depend on its input arguments.
- The function does not produce any observable side effects such as network requests, input and output devices, or data mutation.
    
One of the major benefits of using pure functions is they are immediately testable. They will always produce the same result if you pass in the same arguments.

They also makes maintaining and refactoring code much easier. You can change a pure function and not have to worry about unintended side effects messing up the entire application and ending up in debugging hell.

##### Source: 
- https://medium.com/@jamesjefferyuk/javascript-what-are-pure-functions-4d4d5392d49c









