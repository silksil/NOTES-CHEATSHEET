Below we create a basic model including a schema. 

```js
const mongoose = require('mongoose');

// Heed to be required to create a schema
const Schema = mongoose.Schema;

// Here you define your schema
const UserSchema = new Schema({
  name: String
});

// Here you create your user model, including the schema.
// It will create a user model if doesn't exist yet. 
const User = mongoose.model('user', UserSchema)

// Now you can require the User model and make a reference to a user (CRUD)
module.exports = User;
```js

