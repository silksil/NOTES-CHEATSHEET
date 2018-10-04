The tests below test two cases:
- `.find` = Whether we can find someone or multiple records based on certain properties. Returns an array
- `.findOne` = Whether we can find a particular record, e.g. find someone based on a _id. 

```js
const assert = require('assert');
const User = require('../src/user');

describe('Reading users out of the database', () => {

  // make it global, so we can access it everywhere
  let joe;

  // First we create a user, so we can test whether we can find it
  beforeEach((done) => {
    joe = new User({ name: 'Joe' });
    joe.save()
      .then(() => done());
  });

  it('finds all users with a name of joe', (done) => {
    /*
      besides .find you can use findOne
      .findOne returns only the first user
      .find returns an array with all users

      First you define the user model / class
      You then define the criterea you search for
    */
    User.find({ name: 'Joe' })
      // the callback passes all the users found
      .then((users) => {

        /*
          Monogoose already assigns an id, even though it has not been saved
          Thus we take the created user (joe) and can check it's _id
          The _id isn't a raw string, but an object that wraps that string
          Thus we have to do .toString
        */
        assert(users[0]._id.toString() === joe.id.toString())
        done();
      });
  });
  it('find a user with a particular id', (done) => {
    User.findOne({ _id: joe._id })
      // the difference between .find is that it returns a single user
      .then((user) => {
        assert(user.name === 'Joe');
        done();
      });
  });
});
```
