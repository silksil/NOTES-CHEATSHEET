To check whether Mongoose is connected with mongo you go through the following steps:
1. Mocha starts.
2. Tell Mongoose to connect to Mongo 
  |
  | wait
  |
  ˅ 
3. Mongoose connects to Mongo
4. Connection succesfull? Run tests
5. Connection failed? Show error

This leads to the following code:
```js
const mongoose = require('mongoose');

/* 
Here we specify where make the connection
This says, on my local machine => find this
If it remote, you would replace the localhost for a port
The last part refers to the instance/db you have to connect to
*/ 
mongoose.connect('mongodb://localhost/user_test');

/*
  'once' and 'on' are event handler 
  If you find an event called 'open' we are good to go
  If you find an event called 'on' we failed to connect
*/
mongoose.connection
  .once('open', () => console.log('Good to go'))
  .on('error', (error) => {
    console.warn('Warning', error);
  });
```

