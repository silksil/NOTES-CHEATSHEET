### Subdocuments
Subdocuments are documents embedded in other documents. In Mongoose, this means you can nest schemas in other schemas. 
Mongoose has two distinct notions of subdocuments: 
1. Arrays of subdocuments
2. Single nested subdocuments.</br>
<img src="images/mongoDB-subdocument.png?" width="300">

Subdocuments are similar to normal documents. Nested schemas can have middleware, custom validation logic, virtuals, and any other feature top-level schemas can use. The major difference is that subdocuments are not saved individually, they are saved whenever their top-level parent document is saved.

#### Example
###### Setting-up Sub-Document
```js
const mongoose = require('mongoose');
const { Schema } = mongoose;

const recipientSchema = new Schema({
  email: String,
  responded: { type: Boolean, default: false }
});

module.exports = recipientSchema;
```
###### Import in Top-Level Document
```js
const mongoose = require('mongoose');
const { Schema } = mongoose;

const RecipientSchema = require('./Recipient');

const surveySchema = new Schema({
  title: String,
  body: String,
  subject: String,
  recipients: [RecipientSchema],
  yes: { type: Number, default: 0 },
  no: { type: Number, default: 0 }
});

mongoose.model('survey', surveySchema);
```

#####
When you now pass in a array of object, it will automatically create subdocuments for you. 

#### Relationship setting
In the example below  we indicate that a document(a survey) is owned by another document(a user). 
- __ :  underscore is not required, but it is convention to indicate it is a reference/relationship field
- Type:  we indicate that is has an type that is the id of the user that owns a record
- Ref: the reference we are making belongs to the user collection
```js
const mongoose = require('mongoose');
const { Schema } = mongoose;

const RecipientSchema = require('./Recipient');

const surveySchema = new Schema({
  title: String,
  body: String,
  __user: { type: Schema.Types.ObjectId, ref: 'user'}
});

mongoose.model('survey', surveySchema);
```

## COURSE

### Sub-documents
Subdocuments are documents embedded in other documents. In Mongoose, this means you can nest schemas in other schemas. Subdocuments are similar to normal documents. Nested schemas can have middleware, custom validation logic, virtuals, and any other feature top-level schemas can use. The major difference is that subdocuments cannot be saved individually, they are saved whenever their top-level parent document is saved.

****EXAMPLE**** <br/>
Let's say we have a user model that has a user schema, including first name,  post count and a array of posts. In this case, the array of posts is the resource/sub-document we want to embed.  We don't create a seperate model for the posts to represent the nested resource. Mongoo's models exist to represent distinct collections. Which is not the case with subdocument as they are not saved individually, they are saved whenever their top-level parent document is saved. Thus, if a resource can't be represented by a stand-alone collection, we only make a post schema, not a model. 

So, first we create the post schema:
```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const PostSchema = new Schema({
  title: String
});

module.exports = PostSchema;
```
Then, we import the schema in the model and assign it to a property.
```js

const mongoose = require('mongoose');
//import schema
const PostSchema = require('./post_schema');
const Schema = mongoose.Schema;

const UserSchema = new Schema({
  name: {
    type: String,
    required: [true, 'Name is required.']
  },
  postCount: Number,
  // include the schema and assign it to the property `posts`
  posts: [PostSchema]
});

const User = mongoose.model('user', UserSchema);

module.exports = User;
```
Next, we create a test to see whether it is succesfully added:
```js
const assert = require('assert');
const User = require('../src/user');

describe('Subdocuments', () => {
  it('can create a subdocument', (done) => {
    const joe = new User({
      name: 'Joe',
      /*
        We create a subdocument by nesting properties
        Mongoose will then check the properties (e.g. is it a string?)
      */
      posts: [{ title: 'PostTitle' }]
    });

    joe.save()
      .then(() => User.findOne({ name: 'Joe' }))
      .then((user) => {
        // We check whether the subdocument succesfully added
        assert(user.posts[0].title === 'PostTitle');
        done();
      });
  });
});
```
The advantage of a subdocument is that you can add sub-document to an existing user. So, let's create a test to check this:
```js
it('Can add subdocuments to an existing record', (done) => {
  // create Joe
  const joe = new User({
    name: 'Joe',
    posts: []
  });

  // save Joe
  joe.save()
    // fetch Joe
    .then(() => User.findOne({ name: 'Joe' }))
    .then((user) => {
      /* 
        Add a post to Joe
        Posts is a array, we can push an element to the array
      */ 
      user.posts.push({ title: 'New Post' });
      // save Joe
      return user.save();
    })
    // fetch Joe
    .then(() => User.findOne({ name: 'Joe' }))
    .then((user) => {
      // make assertion
      assert(user.posts[0].title === 'New Post');
      done();
    });
  });
  ```
  Besides adding, we would also like to test whether we can delete a sub-document
  ```js
  it('can remove an existing subdocument', (done) => {
    const joe = new User({
      name: 'Joe',
      posts: [{ title: 'New Title' }]
    });

    joe.save()
      .then(() => User.findOne({ name: 'Joe' }))
      .then((user) => {
        /*
          This is not just plain javascript but functionality by mongoose
          We get the post we want to remove
          Then .remove() can be called
        */
        const post = user.posts[0];
        post.remove();
        /*
          When we remove a document we can just call .remove
          If we remove subdocuments we have to call the parent record with .save
        */
        return user.save();
      })
      .then(() => User.findOne({ name: 'Joe' }))
      .then((user) => {
        assert(user.posts.length === 0);
        done();
      });
  });
 ```
 
### Virtual types
Virtual type/field/property refers to any type of property that is not actually saved to the db. Whenever you think of a property that is the product of two or more properties, you should think of virtual types. For example, you have an array of posts, and you want to have the number of posts as an property. Instead of creating a property `postCount` in MongoDB, you only create it on the server:
```js
const mongoose = require('mongoose');
//import schema
const PostSchema = require('./post_schema');
const Schema = mongoose.Schema;

const UserSchema = new Schema({
  name: {
    type: String,
    required: [true, 'Name is required.']
  },
  posts: [PostSchema]
});

/*
  Create a virtual property, has to be done outside the Schema
  Virtual properties work through `get` and `set` features of ES6
  Through .get we can run a funciton assigned to property and return a value
  What is returned is the value assigned to the property

*/
UserSchema.virtual('postCount').get(function() {
  /* 
    this refers to the instance of the model we are working on
    If we would use a fat arrow function, the `this` would refer to the whole file
    So, use a normal function
  */
  return this.posts.length;
});

const User = mongoose.model('user', UserSchema);

module.exports = User;
```

To test it, we create the following test:
```js
const assert = require('assert');
const User = require('../src/user');

describe('Virtual types', () => {
  it('postCount returns number of posts', (done) => {
    const joe = new User({
      name: 'Joe',
      posts: [{ title: 'PostTitle' }]
    });

    joe.save()
      .then(() => User.findOne({ name: 'Joe' }))
      .then((user) => {
        assert(joe.postCount === 1);
        done();
      });
  });
});
```
