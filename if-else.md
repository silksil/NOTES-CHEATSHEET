### .includes
```javascript
let status = ["status1", "status2", "status3"]

if (status.includes(checkStatus)) {
  //doSomething
}
```

### Conditional (ternary) Operator
```javascript
function getFee(isMember) {
  return (isMember ? "$2.00" : "$10.00");
}

console.log(getFee(true));
// expected output: "$2.00"

console.log(getFee(false));
// expected output: "$10.00"
```
