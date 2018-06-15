## Installation
### Mongoose 
`npm install --save mongoose`

### Config
```
const mongoose = require('mongoose')
const keys = require('./config/keys')
mongoose.connect(keys.mongoURI);
````
