### Create constructor and instance
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
