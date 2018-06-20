### Intro
Check this video for more info about test-driven development and unit testing: https://www.youtube.com/watch?v=Nf5pIGU4Snc

Find here an overview between unit testing and api-testing: https://www.linkedin.com/pulse/difference-between-api-testing-unit-mj-alavi/


## Configuration
`npm i mocha --save-dev` you don't need to run it on a service; you only need to it for testing personally on the machine.

`npm i supertest --sav-dev`


###  Run tests (package.json)
```
{
  "scripts": {
    "test": "mocha **/*.test.js",
    "test-watch": "nodemon --exec 'npm test'"
  },
```

### General
- **describe()**: describe can be used to organize and cluster your tests. If you use describe to cluster a specific method, it is convention to add #: 
```javascript
describe ("Test Util", () =>{
  describe('#addFunction', () => {
  });
});
```
- **it()**: it() is used for an individual test case. it() should be written as if you were saying it out loud: “It should equal zero”, “It should log the user in”
- **Running test and getting feedback in the terminal** If you run tests, the terminal will provide feedback on whether tests have passed. If you write test first before the functionality, if it is red it says: you probably are working on it, if it is green it is probably done and if it is blue it basically says that it still should be done done (it's a pending test > you haven't written a test for it).  Example pending test - becomes blue if you don't pass the second argument:  `it('define what it should do')`
- **done()** The done argument specified makes Mocha understand you run a ascynchronous function - it does not finish processing the test before done is being called. 
- **Timing** Inside node nothing should take more than 1 second. Mocha's standard waiting time is max. 2000ms. 


### HTTP requests
To make the request and assert on its response, the end method can be used:
```javascript
chai.request(app)
  .put('/user/me')
  .send({ password: '123', confirmPassword: '123' })
  .end(function (err, res) {
     expect(err).to.be.null;
     expect(res).to.have.status(200);
  });
```
Because the end function is passed a callback, assertions are run asynchronously. Therefore, a mechanism must be used to notify the testing framework that the callback has completed. Otherwise, the test will pass before the assertions are checked.

For example, in the Mocha test framework, this is accomplished using the done callback, which signal that the callback has completed, and the assertions can be verified:

To make the request and assert on its response, the end method can be used:
```javascript
it('fails, as expected', function(done) { // <= Pass in done callback
  chai.request('http://localhost:8080')
  .get('/')
  .end(function(err, res) {
    expect(res).to.have.status(123);
    done();                               // <= Call done to signal callback end
  });
});

it('succeeds silently!', function() {   // <= No done callback
  chai.request('http://localhost:8080')
  .get('/')
  .end(function(err, res) {
    expect(res).to.have.status(123);    // <= Test completes before this runs
  });
});
```
When done is passed in, Mocha will wait until the call to done(), or until the timeout expires. done also accepts an error parameter when signaling completion.

##### Sources: 
- https://github.com/chaijs/chai-http
- http://mherman.org/blog/2015/09/10/testing-node-js-with-mocha-and-chai/

