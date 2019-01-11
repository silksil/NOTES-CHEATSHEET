### Set-up Configuration
First, we have to set-up that we have access to the query throughout the pages in the app:
```js
import App, { Container } from 'next/app';
import Page from '../components/Page';
import { ApolloProvider } from 'react-apollo';

class MyApp extends App {
  static async getInitialProps({ Component, ctx }) {
    let pageProps = {};
    if (Component.getInitialProps) {
      pageProps = await Component.getInitialProps(ctx);
    }
    // this exposes the query to the user
    pageProps.query = ctx.query;
    return { pageProps };
  }
  render() {
    //rest of code here
  }
```

### Pass query from page to component
The query is only accessible in the pages, so if we want to access it in a component we have to pass it. 
```js
import UpdateItem from '../components/UpdateItem';

// destructure query from props
const Sell = ({ query }) => (
  <div>
    <UpdateItem id={query.id} />
  </div>
  );

export default Sell;
```


### Access query 
The query should be available in the component throught the props
```js
const id = this.props.id
```