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
```

export default Payments;


### Test Stripe Integration
We can check whether the integration has been succesful by including dummy data. Make to make sure to include Stripe's test credit-card number `4242 4242 4242 4242`.








