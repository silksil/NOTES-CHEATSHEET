### Create instance through constructor
```javascript
const Car = function (wheels, seats, engines) {
    this.wheels = wheels;
    this.seats = seats;
    this.engines = engines;
};

const myCar= new Car(3, 1, 2);
console.log(myCar);
// prints  Car { wheels: 3, seats: 1, engines: 2 }
```

### Test whether a instance is a part of a certain class
```
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
var auto = new Car('Honda', 'Accord', 1998);

console.log(auto instanceof Car);
// expected output: true

console.log(auto instanceof Object);
// expected output: true
```
