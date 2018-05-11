PROMISES

### Promise.all
``` javascript
Let calculation = 7;
let someCalculation1 = function() {
  return new Promise(
    function(resolve, reject) {
      if (calculation>0) {
        setTimeout(() => {
          let calculation1 = calculation;
          calculation1 *= 10;
          resolve(calculation1);
        }, 1000);
      } else {
        let reason  = 'The provided value is not greater then 0';
        reject(reason);
      }
    });
}
let someCalculation2 = function() {
  return new Promise(
    function(resolve, reject) {
      if (calculation>0) {
        setTimeout(() => {
          let calculation2 = calculation
          calculation2 *= 20;
          resolve(calculation2);
        }, 500);
      } else {
        let reason  = 'The provided value is not greater then 0';
        reject(reason);
      }
    });
}
Promise.all([ someCalculation1(), someCalculation2() ])
.then(([calculation1, calculation2]) => { console.log(calculation1 +' and '+calculation2) })
.catch(reason => { console.log(reason); })

```
