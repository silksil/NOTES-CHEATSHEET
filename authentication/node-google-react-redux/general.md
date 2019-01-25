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
### Back-End
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
```js
exports.signin = (req, res, next)=> {

  //passport passes on the user
  res.send({ token: tokenForUser(req.user) });
}
```

### Front-end (REACT-REDUX)
#### Sign-Up
##### Create form
```jsx
import React, { Component } from 'react';
import { reduxForm, Field } from 'redux-form';
import { compose } from 'redux';
import { connect } from 'react-redux';
import * as actions from '../../actions'

class SignUp extends Component {
  onSubmit = (formProps) => {
    this.props.signUp(formProps, () => {
      this.props.history.push('/feature');
    });
  }
	render() {
    const { handleSubmit } = this.props;
		return (
			<form onSubmit={handleSubmit(this.onSubmit)}>
				<fieldset>
					<label>Email</label>
					<Field
						name="email"
						type="text"
						component="input"
            autoComplete="none"
					/>
				</fieldset>
				<fieldset>
					<label>Password</label>
					<Field
						name="password"
						type="password"
						component="input"
            autoComplete="none"
					/>
				</fieldset>
        <div> {this.props.errorMessage}</div>
        <button>Sign Up!</button>
			</form>
		);
	}
}

function mapStateToProps(state) {
  return { errorMessage: state.auth.errorMessage };
}

export default compose (
  connect(mapStateToProps, actions),
  reduxForm({ form: 'signup' })
)(SignUp);
// compose allows you to include as many higher order components with an easier to read syntax
```

##### Create action creator to fetch to see if user exist
If user exists, return error. If it not, return token.
```js
export const signUp = (formProps, callback ) => async dispatch => {
  try {
  const response = await axios.post('/api/signup', formProps);

  dispatch({ type: AUTH_USER, payload: response.data.token })
  localStorage.setItem('token', response.data.token);
  callback();
  } catch (error) {
  dispatch({ type: AUTH_ERROR, payload: 'Email in use' })
  }
};
````

##### Send data to reducer
- If a jsonWebtoken is assigned to `authenticated`, it is assumed someone is signed in.
- If jsonWebtoken is not assigned, not signed in.
- Error displays that the user failed to login
```js
import { AUTH_USER, AUTH_ERROR } from '../actions/types';

const INITIAL_STATE = {
  authenticated: '',
  errorMessage: '',
};

export default function (state = INITIAL_STATE, action) {
  switch(action.type) {
    case AUTH_USER:
      return { ...state, authenticated: action.payload }
    case AUTH_ERROR:
     return { ...state, errorMessage: action.payload }
    default:
      return state;
  }
}
```

#### Persist our login state
We store our token in localStorage to persist our login state. We can fetch it from there everytime the app starts back-up. We do this after we succesfully fetched a token trough `localStorage.setItem('token', response.data.token)` in our action creator:
```js
export const signUp = (formProps, callback ) => async dispatch => {
  try {
  const response = await axios.post('/api/signup', formProps);

  dispatch({ type: AUTH_USER, payload: response.data.token })
  localStorage.setItem('token', response.data.token);
  callback();
  } catch (error) {
  dispatch({ type: AUTH_ERROR, payload: 'Email in use' })
  }
};
```
Then, we put an initial state in our redux store. The key of the aut piece of state is `auth` and the value is `authenticated` that we want to initialize when we create the redux-store. We assign what we get from `localStorage.getItem('token')`.
```jsx
import React from 'react';
import ReactDom from 'react-dom';
import { Provider } from 'react-redux';
import {createStore, applyMiddleware} from 'redux';
import ReduxThunk from 'redux-thunk'

import App from './components/App';
import reducers from './reducers';

const store = createStore(reducers, {
  auth: { authenticated: localStorage.getItem('token') }
}, applyMiddleware(ReduxThunk));

ReactDom.render(

  <Provider store={store}><App /></Provider>,
    document.querySelector('#root')
);
```



#### Show pages only if a user is logged-in
In order to do this we first create a higher-order component that checks the state. If somebody is not authorized, the person will be redirected to a different webpage:
```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';

export default ChildComponent => {
  class ComposedComponent extends Component {
    // Our component just got rendered
    componentDidMount() {
      this.shouldNavigateAway();
    }
    // Our component just got updated
    componentDidUpdate() {
      this.shouldNavigateAway();
    }
    shouldNavigateAway() {
      if (!this.props.auth) {
        this.props.history.push('/');
      }
    }
    render() {
      return <ChildComponent {...this.props} />;
    }
  }
  function mapStateToProps(state) {
    return { auth: state.auth.authenticated};
  }
  return connect(mapStateToProps)(ComposedComponent);
};
```
This can be imported into the pages that you only want to show to peole that are authorized to it.
```
import requireAuth from '../requireAuth';
```
```jsx
export default requireAuth(Test);
```


#### Logout
Is a two phase process. 1. Move the token out of the localstorage. 2.) Flip the `authenticated` piece of state to false/empty string.

First, create a component that triggers an `signOut` action creator.
```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
import * as actions from '../../actions';

class Signout extends Component {
  componentDidMount() {
    this.props.signOut();
  }

  render() {
    return <div>Sorry to see you go</div>;
  }
}

export default connect(null, actions)(Signout);
```
Then in the signOut action creator we remove the token out of localStorage and return an empty string to the `authenticated` piece of state.

```jsx
export const signOut = () => {
  localStorage.removeItem('token');

  return {
    type: AUTH_USER,
    payload: '' // sign out be returning a empty user
  }
};
```

















