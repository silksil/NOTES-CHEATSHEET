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

### Why server-side rendering?
With React client-side rendering it often takes several steps before the users sees al l the content on the screen. First the browser requests a page, then it will render all the javascript and only after that it will conduct additional requests to get data from the back-end. This takes time, and more time to load isn't good for the UX. 
<img src="./images/client-side.png?" width="600">

Server-side rendering radically reduces the time to load the page that is shown to the user. Through server-side rendering you will see an html document that has already preloaded all html, including data that had to be fetched from the back-end.
<img src="./images/server-side-1.png?" width="600">

Html content is not reactive, so after we ship down the html we still want to load up our react part of the application that includes all the javascript. So, after we load the html, we load up the javascript bundle. This will eventually lead to the following flow: 
<img src="./images/server-side-2.png?" width="600">
<img src="./images/server-side-3.png?" width="600">
<img src="./images/server-side-4.png?" width="600">

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

To do this you import the app and container component from Next. In this container we will include a `page` component, which will we wrap around the `props component`, which causes to show the `page` component on all props components.

```js
import App, { Container } from 'next/app';
import Page from '../components/Page';

class MyApp extends App {
  render() {
    // refers to your selected component
    const { Component } = this.props;

    return (
      <Container>
        <Page>
          <Component />
        </Page>
      </Container>
    );
  }
}

export default MyApp;
```
The page component which includes the passed props component. 
```js
import React, { Component } from 'react';

class Page extends Component {
  render() {
    return (
      <div>
        <p> Hey i'm the page component</p>
        {this.props.children}
      </div>
    );
  }
}

export default Page;
```


