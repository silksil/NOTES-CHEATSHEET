Go to Stripe and make an account.
`https://stripe.com/gb`

Install the React Stripe Checkout library
`npm install --save react-stripe-checkout`

For the fron-end we need the publishable key. On the back-end we need both the publishable and secret key. Both keys can be found on Stripe under the tab `Developer`. Copy both keys and insert them in your configuration variables (for both DEV and PROD) on the server-side. For PROD you only need to define the environment path, e.g. `stripePublishableKey: process.env.STRIPE_PUBLISHABLE_KEY`. Make sure to include the environment variables in you actual production environment. 

