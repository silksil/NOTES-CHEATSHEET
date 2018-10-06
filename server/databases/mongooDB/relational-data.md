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
## Sub/embedded documents
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
    `this` refers to the instance of the model we are working on
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
### Good & Bad Cases Embedded/Nested documents
- **Good:**  Let's say we want to show blog posts of various user, approx. 10 posts a page. If we then want to fetch 10 blog posts of all the users, it becomes challenging: we don't want to first fetch all user and then select 10 posts (not efficient), and if we would fetch 10 users and then select their blog posts we might end up with users that don't have any posts.
- **Bad:** To get nested records of a specific model, e.g. blog posts of a specific users. 

## Seperate Collections
As an aternative to embedded/nested documents we can create seperate collections and assign a relationship. 
The main disadvantage is that queries becomes more difficult; we have to create multiple queries. In order to create seperate collections we would then link the different collections by passing properties an array with certain id's that represent records of a different collection. For example:

<img src="images/relational-seperate-document.png?" width="500">

To do this, we would first create a seperate model for blog posts and assign a seperate collection of comments to it:
```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const BlogPostSchema = new Schema({
  title: String,
  content: String,
  /*
    Refering to collections is different than referencing subdocuments
    We pass a array to comments to state we expect many different comments
    'type' points off to a different collections by including ObjectId
    'ref' to understand what type/collection the id's relate to we assign a ref
    through 'ref' we state that we create a `Comment` model
    This is similar as: `const User = mongoose.model('user', UserSchema)``
  */
  comments: [{
    type: Schema.Types.ObjectId,
    ref: 'comment'
  }]
});

const BlogPost = mongoose.model('blogPost', BlogPostSchema);

module.exports = BlogPost;
```
We then create a model for comments and assign the comments to a certain user:
```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const CommentSchema = new Schema({
  content: String,
  /*
    We only have a singel user associated with a comment, so we assign a object
  */
  user: { type: Schema.Types.ObjectId, ref: 'user' }
});

// model has have to have the same name as in blogPost
const Comment = mongoose.model('comment', CommentSchema);

module.exports = Comment;
```
Next, assign a collection of blog posts to a user model:
```js
const mongoose = require('mongoose');
const PostSchema = require('./post');
const Schema = mongoose.Schema;

const UserSchema = new Schema({
  name: {
    type: String,
    validate: {
      validator: (name) => name.length > 2,
      message: 'Name must be longer than 2 characters.'
    },
    required: [true, 'Name is required.']
  },
  posts: [PostSchema],
  likes: Number,
  blogPosts: [{
    type: Schema.Types.ObjectId,
    ref: 'blogPost'
  }]
});

UserSchema.virtual('postCount').get(function() {
  return this.posts.length;
});

const User = mongoose.model('user', UserSchema);

module.exports = User;
```
We would then start testing the assocations. First, we have to adapt the test_helper file to assure that every collection is dropped after every test to make sure every test is being run with a clean sheet:

```js
const mongoose = require('mongoose');

/*
 Mongoose has its own promise library, which can show an error
 Here you state you use the ES6 library for promises
*/
mongoose.Promise = global.Promise;

// State that you connect to mongo one time before all tests run
before((done) => {
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
    // done() states: move one to first test, once you are connected
    .once('open', () => { done(); })
    .on('error', (error) => {
      console.warn('Warning', error);
    });
});

  /*
    We drop the db everytime we run a test
    We do this so that individual test are not impacted by each other
    Through 'drop' we take all records and delete it
  */

  beforeEach((done) => {
  const { users, comments, blogposts } = mongoose.connection.collections;
  /*
    We have to drop every collection seperaretly to make sure we have a fresh start for every test
    We include callack to make sure we don't skip to the next test untill we have finished the other test and dropped the included data
  */
  users.drop(() => {
    comments.drop(() => {
      blogposts.drop(() => {
        done();
      });
    });
  });
});
```
Next, we would create the actual tests that will test the assocations. In order to do this, we will first make sure we add a user, that posts a blog post, that has a comment

```js
const mongoose = require('mongoose');
const assert = require('assert');
// upercase if it refers to a class
const User = require('../src/user');
const Comment = require('../src/comment');
const BlogPost = require('../src/blogPost');

describe('Assocations', () => {
  // lowercase if it refers to an instance
  let joe, blogPost, comment;

  beforeEach((done) => {
    joe = new User({ name: 'Joe' });
    blogPost = new BlogPost({ title: 'JS is Great', content: 'Yep it really is' });
    comment = new Comment({ content: 'Congrats on great post' });

    /*
      Has a collection of blogPost and I want to associate a blogpost
      We push in an entire models
      Mongo sees that you push an entire model and simply assigns an objectId
    */
    joe.blogPosts.push(blogPost);
    blogPost.comments.push(comment);

    /*
      This is a hasOne relationship, thus we don't push
      Mongo sees that it is an reference and assigns a objectid
    */
    comment.user = joe;

    // make sure that all three are saved, before calling done()
    Promise.all([joe.save(), blogPost.save(), comment.save()])
      .then(() => done());
  });
});
```
Next, we create a test that checks wether a blogpost is associated with a user:

```js
  it('saves a relation between a user and a blogpost', (done) => {
    User.findOne({ name: 'Joe' })
      /*
        if we would now onsole.log Joe, we would only have a blogPost _id
        A query only gets executed if we include .then (alternatively the less modern.exec)
        But, this query is not finding all the data
        We can add a 'modifier' to customize the query => make it more specific or broad
        .populate is a modifier in which you add a relationship to the query (in this case blog posts)
        The comments are now NOT automatically included, you have to state this specifically
      */
      .populate('blogPosts')
      .then((user) => {
        assert(user.blogPosts[0].title === 'JS is Great');
        done();
      });
  });
```
The test above does not include any test in relation to comment. The main difference with comments is that is deeply nested, which requires a different query, namely: 
```js
it('saves a full relation three/graph', (done) => {
  User.findOne({ name: 'Joe' })
    .populate({
      // path says, inside the object, find these assocations
      path: 'blogPosts',
      // populates says: inside the assocation
      populate: {
        // inside association 'blogPosts' find assocation 'comments'
        path: 'comments',

        // if you load up nested assocations you have to include the model
        model: 'comment',

        // inside association 'comments' find assocation 'user'
        populate: {
          path: 'user',
          model: 'user'
        }
      }
    })
    .then((user) => {
      assert(user.name === 'Joe');
      assert(user.blogPosts[0].title === 'JS is Great');
      assert(user.blogPosts[0].comments[0].content === 'Congrats on great post');
      assert(user.blogPosts[0].comments[0].user.name === 'Joe');

      done();
    });
  });
 ```
 






