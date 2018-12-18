Next.js is a React framework for building web applications. It does:
- All of the tooling: webpack, code splitting etc.
- It does server-side rendering: if you care about instant loading, pre-loading, seo etc., server-side rendering is important. It provides GetInitialProps: allows you to fetch data on the server. You wait on the data to be resolved before the page is send to the browser
- It does the routing; there is no routing, there are pages. Every singel page you want to visit those gonna be .js files. In those files you can create a React component. 

So, let's get started. Let's say you want to create a page where you sell items. You simply can include the following page, which we name sell.js, and then this page will be shown if we go to the link `/sell`. 
```js
const Sell = props => (
  <div>
    <p>Sell!</p>
  </div>
);

export default Sell;
````


### Linking
If you want to allow a user to go to a different page, Next.js provides you with functionality that allows you to push the state: you don't use anchor tags because you do discrete navigation.
```js
import Link from 'next/link';

const Home = props => (
  <div>
    <p>Hey!</p>
    <Link href="/sell">
        <a>Sell!</a>
    </Link>
  </div>
)

export default Home;
```

### _App.js
Next.js uses the App component to initialize pages. You can override it and control the page initialization. Which allows you to:
- Persisting layout between page changes
- Keeping state when navigating pages
- Custom error handling using componentDidCatch
- Inject additional data into pages (for example by processing GraphQL queries)

To do this you import the app and container component from Next.


