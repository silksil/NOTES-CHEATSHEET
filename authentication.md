# Cookies vs Token

#### Cookie
- It brings state in a stateles protocoll (namely the http protocol)
- Unique to each domain. Cannot be send to different domains. 
- Are included by default in all requests, in the Header `cookie: {}`


#### Token
- Have to manually wire up > e.g. manually include it in the header
- Can be sent to any domain. So if you go to a different website, you are logged in. So if you have many different servers on many different domains, it is usefull to use tokens.
- Token is more trending, mainly because it is more scalable. 
- If you have a mobile app and a web app, it is  likely to be more usefull to use a single API server. This is therefore on a seperate server than the client side of a the web app, thus a token is likely to be more interesting.

# Token
https://jwt.io/

## Generate Token
#### Install
```npm install--save jwt-simple```

#### Function to generate token
```js
const jwt = require('jwt-simple');
const config = require('../config/keys')

function tokenForUser(user) {
  const timestamp = new Date().getTime();
  return jwt.encode({ sub: user.id, iat: timestamp}, config.tokenKey);
  // sub refers to the subject it belongs to, iat refers to issued at time
}
```

#### Send Token
```js
res.json({ token: tokenForUser(user) }
```

## Authentication
#### Install & Config
```
npm install --save passport passport-jwt
```

```
const passport = require('passport');
const User = require('../models/user');
const config = require('../config');
const JwtStrategy = require('passport-jwt').Strategy;
const ExtractJwt = require('passport-jwt').ExtractJwt;
```

#### Create JWT strategy
```js
const jwtLogin = new JwtStrategy(jwtOptions, (payload, done)=> {
  // payload is encoded jwt.token that we set-up in authentication controller
  // See if the user ID in the payload exists in our database
  // If it does, call 'done' with that other
  // otherwise, call done without a user object
  User.findById(payload.sub, (err, user)=> {
    if (err) { return done(err, false); }

    if (user) {
      done(null, user);
    } else {
      done(null, false);
    }
  });
});
```
#### Setup options for JWT Strategy > define where to find the token on the request
```js
const jwtOptions = {
  jwtFromRequest: ExtractJwt.fromHeader('authorization'),
  secretOrKey: config.tokenKey // the secret to decode the token
};
```

#### Tell passport to use this strategy
```js
passport.use(jwtLogin);
```

#### Hook it up in certain routes and test it with a token
```js
const passportService = require('../services/passport');
const passport = require('passport')

const requireAuth = passport.authenticate('jwt', { session: false}) // have to specifically state that we don't use sessions

module.exports = (app) => {

  app.get('/', requireAuth, (req,res)=> {
    res.send({hi: "there"})
  })

}
```

## Login
First check whether the user provided a correct username and password, then send to the controller that sends along a token.
```js
const requireAuth = passport.authenticate('jwt', { session: false})
const requireSignin = passport.authenticate('local', { session: false })

module.exports = (app) => {
  app.post('/signup', authentication.signup)

  app.post('/signin', requireSignin, authentication.signin) 
}
```
Passport is used to verify an emaill and password
```js
const localOptions = { usernameField: 'email' };
const localLogin = new LocalStrategy(localOptions, (email, password, done)=> {
  // Verify this email and password, call done with the user
  // if it is the correct email and password
  // otherwise, call done with false
  User.findOne({ email: email }, (err, user)=> {
    if (err) { return done(err); }
    if (!user) { return done(null, false); }

    // compare passwords - is `password` equal to user.password?
    user.comparePassword(password, (err, isMatch)=> {
      if (err) { return done(err); }
      if (!isMatch) { return done(null, false); }
      else { return done(null, user); }
    });
  });
});
```
Becrypt is added in the user model file
```js
userSchema.methods.comparePassword = function(candidatePassword, callback) {
  bcrypt.compare(candidatePassword, this.password, (err, isMatch)=> {
    if (err) { return callback(err); }

    callback(null, isMatch);
  });
}
```
Then it is passed on to the controller to creates the token
```
exports.signin = (req, res, next)=> {

  //passport passes on the user
  res.send({ token: tokenForUser(req.user) });
}
```












