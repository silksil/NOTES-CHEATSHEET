# Learning Goals
- What are the different ways to write if else statements?

# Different ways to check condition
## Switch
In an if-statement we have multiple (possibly unrelated) expressions that are all coerced to booleans:
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

## Conditional (ternary) operator
```javascript
function getFee(isMember) {
  return (isMember ? "$2.00" : "$10.00");
}

console.log(getFee(true));
// expected output: "$2.00"

console.log(getFee(false));
// expected output: "$10.00"
```

## Inline if with comparison and logical operator
```jsx
<div>
 <h1>Hello!</h1>
 {unreadMessages.length > 0 &&
  <h2>
    You have {unreadMessages.length} unread messages.
  </h2>
 }
</div>
// see also: https://reactjs.org/docs/conditional-rendering.html
```
