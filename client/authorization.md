In order to determine whether someone has authorization to a certain page,  we create an AuthReducer that can return three things:

| Situation   | AuthReducer | Returns  |   
|-------------|-------------|----------|
|  Make request to bakckend to get current user           |    null         |   'null' indicates we really don't know whats up right now (e.g. when request is still pending)       |
|  Request complete, user is logged in          |     User model        |    Object containing user ID      |
|     Request done, user *is not* logged in        |      false       |   Flase means 'yep, we are sure the user isn't logged in'       |

First, we create a action creator that fetches the authentication.
```js
import axios from 'axios';
import { FETCH_USER } from './types';

export const fetchUser = () => async dispatch => {  // whenever fetchUser is called it will instantly return a function
    const res = await axios.get('/api/current_user');

    dispatch({ type: FETCH_USER, payload: res.data });
};
```
As multiple components depend on whether a user is logged in, we want to include an user in the main app component. We want to fetch our current user the instance an application loads, thus we do this through `componentDidMount`.
```js
import React, { Component } from 'react';
import { BrowserRouter, Route } from 'react-router-dom';
import { connect } from 'react-redux';
import * as actions from '../actions';

class App extends Component {
  componentDidMount() {
    this.props.fetchUser(); // call the function after App is being rendered
  }
  
  render() {
    return (
      <div>
        <BrowserRouter>
          <div >
            <Header />
          </div>
        </BrowserRouter>
      </div>
    );
  }
}

export default connect(null, actions)(App) // connect action creators
```
As mentioned above, we either return `null`, `userModel` or `false`. Our us
```js
import { FETCH_USER } from '../actions/types';

export default function (state = null, action) { // we return null, so be default we don't know if he is logged-in
  switch (action.type) {
    case FETCH_USER:
      return (action.payload.length === 0 ? action.payload : false); // if the string is empty string  return false
    default:
      return state;
  }
}
```
Next, we hook up the authReducer to the root reducer.
```
import { combineReducers } from 'redux';
import authReducer from './authReducer';

export default combineReducers({
  auth: authReducer,
});
```

Lastly, we can adapt our components based on whether a user is logged in or not.
```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';

class Header extends Component {
  renderContent() { // rendering content based on the auth status
    switch(this.props.auth) {
      case null:
        return;
      case false:
        return <li><a href="/auth/google">Login With Google</a></li>;
      default:
        return <li><a> Logout</a></li>;
    }
  }

  render() {
    return (
      <nav>
        <div className="nav-wrapper">
          <a className="left brand-logo">
            Emaily
          </a>
          <ul className="right">
            {this.renderContent()}
          </ul>
        </div>
      </nav>
    );
  }
}

function mapStateToProps({ auth }) {
  return { auth };
}

export default connect(mapStateToProps)(Header);
```

