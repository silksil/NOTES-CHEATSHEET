To work with pagination we have two query modifiers:
- `skip`:  You start with a skip value of zero, once you start to increment you skip a certain amount of records. So, in the example below we skip the first 15 results. 
- `limit`: It limits the amount of results you get back of the query. 

<img src="images/pagination.png" width="200">

To exemplify this, we create a test. 

First, let's add data to the db:
```
const assert = require('assert');
const User = require('../src/user');

describe('Reading users out of the database', () => {

  // make it global, so we can access it everywhere
  let joe, maria, alex, zach;

  // First we create users so we can test whether we can find it
  beforeEach((done) => {
    alex = new User({ name: 'Alex' });
    joe = new User({ name: 'Joe' });
    maria = new User({ name: 'Maria' });
    zach = new User({ name: 'Zach' });

    // the sequence of saving is not guaranteed
    Promise.all([joe.save(), alex.save(), maria.save(), zach.save()])
      .then(() => done());
  });
  ```
  Next, test whether we get back the correct users if we include .skip and .limit:
  ```
    it('can skip and limit the result set', (done) => {
    User.find({})
      /*
        sort it based on the property 'name'
        1 = a>z
        -1 = z>a
      */
      .sort({ name: 1 })
      .skip(1)
      .limit(2)
      .then((users) => {
        assert(users.length === 2);
        assert(users[0].name === 'Joe');
        assert(users[1].name === 'Maria');
        done();
      });
  });
  ```

