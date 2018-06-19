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



### It
If you don't pass any second argument, it will become blue if you run the test; it will list as a pending test. It says that you haven't written a test for it. 

If you write test first before the functionality, if it red it says: you probably are working on it, if it is green it is probably done and if it is blue it basically says that it still should be done done (it's a pending test > you haven't written a test for it). 

Example pending test - becomes blue because you don't pass a second argument. `it('define what it should do')`


