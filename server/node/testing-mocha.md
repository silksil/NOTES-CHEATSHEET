## Intro
Check this video for more info about test-driven development and unit testing: https://www.youtube.com/watch?v=Nf5pIGU4Snc

Find info about the difference between unit testing and functional testing: 
- https://www.linkedin.com/pulse/difference-between-api-testing-unit-mj-alavi/
- https://www.quora.com/What-are-the-differences-between-an-API-unit-test-and-an-API-functional-test
-


## Configuration
`npm i mocha --save-dev` you don't need to run it on a service; you only need to it for testing personally on the machine.

`npm i supertest --sav-dev`

`npm i sinon --save-dev`


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
- **before() and after()** can be used to prepping data and clean it up afterwards. e.g. add a user to the db, and delete it from the db. So will call before(), then it() and then after(). 


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

## Unit testing
Besides testing the the response of an API, you can test what is happening inside the API. In other words, you can open the black box, so you not only know whether the output is correct, but also test the individual units. 

### Side effects
We can split functions into two categories:

    Functions without side effects
    And functions with side effects

Testing code with Ajax, networking, timeouts, databases, or other dependencies have side effects. To mitigate this we can create ***test-doubles***:

- **Spies**, which offer information about function calls, without affecting their behavior. When that test function is being called you can make certain assertions about it to make sure it is being called with certain arguments. 
- **Stubs**, which are like spies, but completely replace the function. This makes it possible to make a stubbed function do whatever you like — throw an exception, return a specific value, etc
- **Mocks**, which make replacing whole objects easier by combining both spies and stubs

Here are some examples of other useful assertions provided by Sinon:

    sinon.assert.calledWith can be used to verify the a function was called with specific parameters (this is probably the one I use most often)
    sinon.assert.callOrder can verify functions were called in a specific order



##### Sources: 
- https://github.com/chaijs/chai-http
- http://mherman.org/blog/2015/09/10/testing-node-js-with-mocha-and-chai/
- https://www.youtube.com/watch?v=7QtlKGjR50o
- https://devhints.io/chai
