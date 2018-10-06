## Installation and set-up  MongoDB locally
```
brew update
brew install mongodb
```
#### Create data directory where data can be saved
```
sudo mkdir -p /data/db
```
#### Take ownership of the directory
```
whoami // get $USER
sudo chown $USER /data/db
```
#### Start mongod (has to be done everytime you restart your laptop)
`mongod`

## Setup Project
#### Install Mongoose 
`npm install --save mongoose`

#### Include 'dev.js' and 'prod.js' config files
```js
// DEV
mongoURI: 'mongodb://YOUR_USERNAME:YOURPASSWORD.....',

// PRO
mongoURI: process.env.MONGO_URI,
```
#### Link to prod or dev through key.js file
```js
`use strict`;

if (process.env.NODE_ENV === 'production') {
  module.exports = require ('./prod')
  } else {
  module.exports = require('./dev');
}
```
#### Import key.js and set-up connection in app.js
```
const mongoose = require('mongoose')
const keys = require('./config/keys')
mongoose.connect(keys.mongoURI);
````
####  Require Your models in app.js
`require('./models/user');`
