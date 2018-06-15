### Instances
"instance" is best understood as it relates to "class" in programming. "Classes" are used to define the properties and behavior of a category of things. E.g. A "Car" class might dictate that all cars be defined by their make, model, year, and mileage.
But you cant provide specifics about a particular car (for example, that 1978 Chevy Impala with 205,000 miles on it that your uncle Mickey drives) until you create an "instance" of a Car. It's the instance that captures the detailed information about one particular Car.

Source: https://stackoverflow.com/questions/20461907/what-is-meaning-of-instance-in-programming?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa


### Difference property, key and value
The term property (also: attribute, less common or even used for different things in JS) usually refers to the key/value pair that describes a member of an object. While, especially when used with a specific identifier (key), it often refers to the whole combination, it can also denote the value of that member. It does usually not mean the identifier itself.

When people try to be accurate, they distinguish between "property" (the whole thing, part of an object), "property name" (the string used as the key) and "property value" (the stored data).

Source: https://stackoverflow.com/questions/28648090/properties-vs-keys-vs-values-in-javascript?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa

### Higher-order functions
A higher order function is a function that takes one or more functions as arguments. 

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

Source: https://medium.com/humans-create-software/a-dirt-simple-introduction-to-higher-order-functions-in-javascript-b33bf9e19056

### Pure functions



