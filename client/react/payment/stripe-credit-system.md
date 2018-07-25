This component is created while working on https://github.com/silksil/survey-app
- It is a component that allows you to buy credit through Stripe, store in de DB and display it in the front-end.
- It is functionality build using React and Node.js
- We store the credit state in user model (which e.g. also display id and if he or she is logged in)

### Make an Account
Go to Stripe and make an account.
`https://stripe.com/gb`

### Include Keys On The Server-Side
On the back-end we need both the publishable and secret key. Both keys can be found on Stripe under the tab `Developer`. Copy both keys and insert them in your configuration variables (for both DEV and PROD) on the server-side. Make sure to include the DEV confige file in .gitignore and for PROD you only need to define the environment path, e.g. `stripePublishableKey: process.env.STRIPE_PUBLISHABLE_KEY`. Then, make sure to include the environment variables in you actual production environment.

### Include Keys On the Client-SIde
Create an `.env.development` and `.env.production` file in your Client folder. 

You must create custom environment variables beginning with REACT_APP_. Any other variables except NODE_ENV will be ignored to avoid accidentally exposing a private key on the machine that could have the same name. Further, for the front-end we only need to include the publishable key. Thus, add:
`REACT_APP_STRIPE_KEY=your_actual_publishable_key`

For more information about environment variables in React see: https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-custom-environment-variables

### Install React Stripe Checkout Library
For this example, we will create a credit-card form in which people can fill in their info and pay 5 dollar. In order to do this we will include the `react-sripe-checkout` library. It is not the checkout-form, but is a button you can click on, that then will show the checkout-form.
`npm install --save react-stripe-checkout`

```jsx
class Payments extends Component {
  render() {
    return (
      <StripeCheckout
        name="Emaily"
        description="$5 for 5 email credits"
        amount={500}
        token={token => console.log(token)}
        stripeKey={process.env.REACT_APP_STRIPE_KEY}
      >

        <button className="btn">
          Add Credits
        </button>
      </StripeCheckout>
      /* amount: here you can define the amount of money you request. The default is U.S. Dollars in cents
         token: is expecting to receive a callback function, which is called after we succesfully received an authorization token */
    );
  }
}

export default Payments;
```


### Test Stripe Integration
We can check whether the integration has been succesful by including dummy data. Make to make sure to include Stripe's test credit-card number `4242 4242 4242 4242`.

### Create Action Creator
```js
export const handleToken = (token) => async dispatch => {
  const res = await axios.post('/api/stripe', token);

  dispatch({ type: FETCH_USER, payload: res.data });
};
```

### Hook-Up Action Creator
```jsx
import React, { Component } from 'react';
import StripeCheckout from 'react-stripe-checkout';
import { connect } from 'react-redux';
import * as actions from '../actions';

class Payments extends Component {
  render() {
    return (
      <StripeCheckout
        name="Emaily"
        description="$5 for 5 email credits"
        amount={500}
        token={token => this.props.handleToken(token)}
        stripeKey={process.env.REACT_APP_STRIPE_KEY}
      >
        <button className="btn">
          Add Credits
        </button>
      </StripeCheckout>
      /* amount: here you can define the amount of money you request. The default is U.S. Dollars in cents
         token: is expecting to receive a callback function, which is called after we succesfully received an authorization token */
    );
  }
}

export default connect(null, actions)(Payments);
```

### Install Strip Library Server
`npm install --save stripe`

### Require Stripe
```js
const keys = require('../config/keys')
const stripe = require('stripe')(keys.stripeSecretKey);


module.exports = app => {
  app.post('/api/stripe', (req, res) => {
    console.log(req.body)
  })
};
```

### Bill User
For more info see: https://stripe.com/docs/api/node#create_charge
```js
const keys = require('../config/keys')
const stripe = require('stripe')(keys.stripeSecretKey);

module.exports = app => {
  app.post('/api/stripe', async (req, res) => {

    console.log(stripe)
    const charge = await stripe.charges.create({
      amount: 500,
      currency: 'usd',
      description: '$5 for 5 credits', // can by any description
      source: req.body.id, // what credit-card source you try to bill
    });

    console.log(charge)
  });
};
```

### Update Credit In DB (credit is stored in User Model)
```js
const keys = require('../config/keys')
const stripe = require('stripe')(keys.stripeSecretKey);
const requireLogin = require('../middlewares/requireLogin')

module.exports = app => {
  app.post('/api/stripe', requireLogin, async (req, res) => {
    const charge = await stripe.charges.create({
      amount: 500,
      currency: 'usd',
      description: '$5 for 5 credits', // can by any description
      source: req.body.id, // what credit-card source you try to bill
    });

    req.user.credit += 5;
    const user = await req.user.save();

    res.send(user); // you can send it back in case you need to new credit as your state
  });
};
```

### State now automatically includes updated credit
You receive the user model (which include the credit) and send it to your reducers.
```js
export const handleToken = (token) => async dispatch => {
  const res = await axios.post('/api/stripe', token);

  dispatch({ type: FETCH_USER, payload: res.data });
};
```
```js
import { FETCH_USER } from '../actions/types';

export default function (state = null, action) { // we return null, so be default we don't know if he is logged-in
  switch (action.type) {
    case FETCH_USER:
      return (action.payload.length !== 0) ? action.payload : false; // if the string is empty string  return false
    default:
      return state;
  }
}
```
```js
import { combineReducers } from 'redux';
import authReducer from './authReducer';

export default combineReducers({
  auth: authReducer,
});
```

### Display Credit
```return <li key="credit"> Credits: {this.props.auth.credit} </li>```
       














