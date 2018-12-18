### Authentiction
The following example focuses on authentication using GraphQL and Passport. There are two approaches we could take:
1. **Decoupled**
We take of passport, without using GraphQL. We set the cookie without GraphQl, when the user does a follow up request. The request will be identified by Passport, and than it will be passed to GraphQl who then formulates the response and sends it back to the client.
<img src="images/auth-decoupled.png?" width="600">

##### Disadvantages
- Inconsistency: You split the app in a part that uses GraphQl and doesn't use it.

2. **Coupled**
Now we make use of a mutation to authenticate the user in some fashion. We have an incoming auth request, which will be a mutation, and it will pass along the request to passport.
<img src="images/auth-coupled.png?" width="600">
##### Advantages
- You use it as it the way it is intended. Useful if you have separate services to keep organized.

##### Disadvantages
- GraphQl and passport are not set-up to work well together; complex to set-up.

The remaining code will focus on the coupled approach.

### Data Overview: Types and Mutations
Let's start with designing the types and mutations.

- Signup: both creating a new user and authentication(considering him logged-in)
- Login
- Logout

The `UserType` will be added to the RootQuery. We make it possible to look add different users, a particular user or a reference to the current user.

<img src="images/auth-types-mutations.png?" width="600">

## Server
### Set up type
To set-up the UserType in GraphQl we have to look how the models looks in the db (MongoDb).
```js
const UserSchema = new Schema({
  email: String,
  password: String
});
```
It includes an email and password, nonetheless password is something we would never expose to the outside world through our GraphQL schema. If you want to limit the ability to see other people's email address, you may choose to leave the email out. In this we include it, and thus we create:

```js
const graphql = require('graphql');
const {
  GraphQLObjectType,
  GraphQLString,
  GraphQLID
} = graphql;

const UserType = new GraphQLObjectType({
  name: 'UserType',
  fields: {
    id: { type: GraphQLID },
    email: { type: GraphQLString }
  }
});

module.exports = UserType;
```
### Mutations + Helpers
In relation to the mutations, additionally functionality has to be included somewhere -- like checking passwords, reading users from db, assure that the user is not already -- in order to make it work according standards. We will not include this in the `resolve` function or mutations, but try to include this in helper function / object.

First we create the helpers that handle the authentication with passport and the db:
```js
const mongoose = require('mongoose');
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;

const User = mongoose.model('user');

// SerializeUser is used to provide some identifying token that can be saved
// in the users session.  We traditionally use the 'ID' for this.
passport.serializeUser((user, done) => {
  done(null, user.id);
});

// The counterpart of 'serializeUser'.  Given only a user's ID, we must return
// the user object.  This object is placed on 'req.user'.
passport.deserializeUser((id, done) => {
  User.findById(id, (err, user) => {
    done(err, user);
  });
});

// Instructs Passport how to authenticate a user using a locally saved email
// and password combination.  This strategy is called whenever a user attempts to
// log in.
passport.use(new LocalStrategy({ usernameField: 'email' }, (email, password, done) => {
  User.findOne({ email: email.toLowerCase() }, (err, user) => {
    if (err) { return done(err); }
    if (!user) { return done(null, false, 'Invalid Credentials'); }
    user.comparePassword(password, (err, isMatch) => {
      if (err) { return done(err); }
      if (isMatch) {
        return done(null, user);
      }
      return done(null, false, 'Invalid credentials.');
    });
  });
}));

// Creates a new user account.Notice the Promise created in the second 'then' statement.
// This is done because Passport only supports callbacks, while GraphQL only supports promises
// for async code!  Awkward!
function signup({ email, password, req }) {
  const user = new User({ email, password });
  if (!email || !password) { throw new Error('You must provide an email and password.'); }

  return User.findOne({ email })
    .then(existingUser => {
      if (existingUser) { throw new Error('Email in use'); }
      return user.save();
    })
    .then(user => {
      return new Promise((resolve, reject) => {
        req.logIn(user, (err) => {
          if (err) { reject(err); }
          resolve(user);
        });
      });
    });
}

// Logs in a user.  This will invoke the 'local-strategy' defined above in this
// file.
function login({ email, password, req }) {
  return new Promise((resolve, reject) => {
    passport.authenticate('local', (err, user) => {
      if (!user) { reject('Invalid credentials.') }

      req.login(user, () => resolve(user));
    })({ body: { email, password } });
  });
}

module.exports = { signup, login };
```

Then we create the mutations that link to these helpers:
```js
const graphql = require('graphql');
const {
  GraphQLObjectType,
  GraphQLString
} = graphql;
const UserType = require('./types/user_type');
const AuthService = require('../services/auth');

const mutation = new GraphQLObjectType({
  name: 'Mutation',
  fields: {
    signup: {
      type: UserType,
      args: {
        email: { type: GraphQLString },
        password: { type: GraphQLString }
      },
      // `req` represents the request object coming from Express. Is expected by passport.
      resolve(parentValue, { email, password }, req) {
        return AuthService.signup({ email, password, req });
      }
    },
    logout: {
      type: UserType,
      resolve(parentValue, args, req) {
        const { user } = req;
        req.logout();
        return user;
      }
    },
    login: {
      type: UserType,
      args: {
        email: { type: GraphQLString },
        password: { type: GraphQLString }
      },
      resolve(parentValue, { email, password }, req) {
        return AuthService.login({ email, password, req });
      }
    }
  }
});

module.exports = mutation;
```  
We then add the mutations to the schema:
```js
const graphql = require('graphql');
const { GraphQLSchema } = graphql;

const RootQueryType = require('./types/root_query_type');
const mutation = require('./mutations');

module.exports = new GraphQLSchema({
  query: RootQueryType,
  mutation
});
```
In order to now test it, we have to assure we have assigned a value to the property `field` in the RootQueryType. In order to handle this we, for now, will assign dummy data to it:
```js
const graphql = require('graphql');
const { GraphQLObjectType, GraphQLID } = graphql;

const RootQueryType = new GraphQLObjectType({
  name: 'RootQueryType',
  fields: {
    dummyField: { type: GraphQLID }
  }
});

module.exports = RootQueryType;
```
We can now test it in the GraphQL interface and check it in the db:
```js
mutation {
  signup(email: "test@test.com", password:"password") {
    email
  }
}
```

Next, we wanna check whether the user is authenticated, In order to do this, we add a field to the RootQueryType and we return the current authenticated user. If it contains a user (and that property is automatically puplated by passport) a user is authenticated, if it's null it's not authenticated.
```js
const graphql = require('graphql');
const { GraphQLObjectType, GraphQLID } = graphql;
const UserType = require('./user_type');

const RootQueryType = new GraphQLObjectType({
  name: 'RootQueryType',
  fields: {
    user: {
      type: UserType,
      resolve(parentValue, args, req) {
        // The request get it's properties automatically placed by passport
        // when we authenticate a user. Thus, when authenticated, it should have a `req.user`
        return req.user;
      }
    }
  }
});

module.exports = RootQueryType;
```
We can now test it through a login mutation and then write a query in which we ask for the current user with an email, it should return an email.

## Client
On the client-side, the mutation is handling the authentication. If the client-side conduct a request (be it either a query or a mutation) it doesn't attach by default any cookies, and therefore it doesn't add the current user. 
