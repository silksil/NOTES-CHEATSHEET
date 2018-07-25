## Installation
### Mongoose 
`npm install --save mongoose`

## Config
### Set-Up you App File
```
const mongoose = require('mongoose')
const keys = require('./config/keys')
mongoose.connect(keys.mongoURI);
````
### Link to PROD or Dev
```js
`use strict`;

if (process.env.NODE_ENV === 'production') {
  module.exports = require ('./prod')
  } else {
  module.exports = require('./dev');
}
```js
### Config Variables
```
`use strict`;

module.exports = {
  mongoURI: 'mongodb://YOUR_USERNAME:YOURPASSWORD.....',
};
```js
### Set-Up Your Model
```js
const mongoose = require('mongoose');

const { Schema } = mongoose; // mongoose wants us to define the schema beforehand.

const userSchema = new Schema({
  googleId: String,
  credit: { type: Number, default: 0 } //
});

mongoose.model('users', userSchema); // state it should create a collection - if it not already exists
```
### Require Your Model in App file
`require('./models/user');`
