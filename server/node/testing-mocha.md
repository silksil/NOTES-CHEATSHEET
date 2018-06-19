## Configuration
`npm i mocha --save-dev` you don't need to run it on a service; you only need to it for testing personally on the machine.


###  Run tests (package.json)
```
{
  "scripts": {
    "test": "mocha **/*.test.js",
    "test-watch": "nodemon --exec 'npm test'"
  },
```
### Shortcuts
npm run test -g "should add numbers"

### It
If you write test first before the functionality, if it red it says: you probably are working on it, if it is green it is probably done and if it is blue it basically says that it still should be done done (it's a pending test > you haven't written a test for it). 

Example pending test - becomes blue if you don't pass the second argument:  `it('define what it should do')`

### Done()
The done argument specified makes Mocha understand you run a ascynchronous function - it does not finish processing the test before done is being called. 

Timing
Inside node nothing should take more than 1 second. Mocha's standard waiting time is max. 2000ms. 

