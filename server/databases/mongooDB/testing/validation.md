The following code tests whether the validation is working correctly. It tests:
- That it should have a certain property
- That it should include a minimal amount of characters

The model we test look like this:

```js
const mongoose = require('mongoose');

// Needs to be required to create a schema
const Schema = mongoose.Schema;

const UserSchema = new Schema({
  name: {
    type: String,
    /*
      Here we validate the passed proprety
      We check if it has more than 2 characters
      True = valid. False = invalid
    */
    validate: {
      validator: (name) => name.length > 2 ,
      message: 'Name must be longer than 2 characters.'
    },
    /*
      Here you define that this property is required
      Include a clear error message as the second list item
    */
    required: [true, 'Name is required.']
  },
  postCount: Number
});

// Here you create your user model, including the schema.
// It will create a user model if doesn't exist yet.
const User = mongoose.model('user', UserSchema)

// Now you can require the User model and make a reference to a user (CRUD)
module.exports = User;
```

The tests look like this:

```js
const assert = require('assert');
const User = require('../src/user');

describe('Validating records', () => {
  /*
    If a username is not assigned, it's not a valid record
  */
  it('requires a user name', () => {
    // include a name that is undefined, don't save it
    const user = new User({ name: undefined });
    /*
      ValidateSync returns an object validating the user model
      If a lot of properties were wrong, it would include a lot of error messages
    */
    const validationResult = user.validateSync();
    const { message } = validationResult.errors.name;

    assert(message === 'Name is required.');
  });
  it('requires a user\'s name longer than 2 characters', () => {
    const user = new User({ name: 'Al' });
    const validationResult = user.validateSync();
    const { message } = validationResult.errors.name;

    assert(message === 'Name must be longer than 2 characters.');
  });
  it('disallows invalid records from being saved', (done) => {
    const user = new User({ name: 'Al' });
    user.save()
      .catch((validationResult) => {
        const { message } = validationResult.errors.name;

        assert(message === 'Name must be longer than 2 characters.');
        done();
      });
  });
});
```
