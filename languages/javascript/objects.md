# Learning Goals
- What does Object.keys() do?
- What does Object.values() do?
- What does hasOwnProperty() do?
- How can you copy a object?

# Methods
## Object.keys()
Object.keys(obj) – returns an array of keys.
```js
const states = {
  "AZ": "Phoenix",
  "NY": "Albany",
  "VA": "Richmond",
  "Wisconsin": "Madison"
}

const keys = Object.keys(states); // We pass the object as the argument.
// This statement will become an array of [ 'AZ', 'NY', 'VA',      // 'Wisconsin' ].
```

## Object.values()
Object.values(obj) – returns an array of values.
```js
const states = {
    "AZ": "Phoenix",
    "NY": "Albany",
    "VA": "Richmond",
    "Wisconsin": "Madison"
  }

const values = Object.values(states); // We pass the object as the argument.
// This statement will become an array of [ 'Phoenix', 'Albany',    // 'Richmond', 'Madison' ].

```

## Object.Entries()
Object.entries(obj) – returns an array of [key, value] pairs.
```js
const states = {
    "AZ": "Phoenix",
    "NY": "Albany",
    "VA": "Richmond",
    "Wisconsin": "Madison"
  }

const entries = Object.entries(states);// We pass the object as the argument.
// This statement will become an array of:
// [ [ 'AZ', 'Phoenix' ], [ 'NY', 'Albany' ], [ 'VA', 'Richmond'], [ 'Wisconsin', 'Madison' ] ]
```

## hasOwnProperty()
The hasOwnProperty() method returns a boolean indicating whether the object has the specified property as its own property (as opposed to inheriting it).

```js
const object1 = new Object();
object1.property1 = 42;

console.log(object1.hasOwnProperty('property1'));
// expected output: true
```



