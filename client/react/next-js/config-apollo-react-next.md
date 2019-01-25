The purpose of Apollo is bonding layer between React and the GraphQl graph. Apollo consists out of the Apollo `Store` and `Provider`. The Store is a client-side piece of tech that directly communicate with the server and saves the data in the store. It can be used across different technologies, not just React. Nonetheless, the Apollo Provider glues React with the Apollo store by taking data from the store and injecting it in the client-side. Most set-up is related to the Apollo Provider.

This configuration includes Next.js for server-side rendering, which includes some additional steps for setting-up everything.

First, we set-up the Apollo Client file:
```js
// Gives us a high-order component that exposes the Apollo Client through a prop
import withApollo from 'next-with-apollo';
// Apollo Boost includes preconfigured packages to develop with with Apollo Client
import ApolloClient from 'apollo-boost';
import { endpoint } from './config';

// headers are used, among other things, for authentication
function createClient({ headers }) {
  return new ApolloClient({
    // currently thie endpoint remains the same, but this can change if you go to production
    uri: process.env.NODE_ENV === 'development' ? endpoint : endpoint,
    request: operation => {
      operation.setContext({
        fetchOptions: {
          // We instruct to pass credentials for  every single request
          // for example, when being logged in, cookies are passed
          credentials: 'include',
        },
        headers,
      });
    },
  });
}

export default withApollo(createClient);
```
Then we include the Apollo Client in Apollo Provider, linking the Apollo state handler (Apollo Client) with React through Apollo Provider.
```js
import App, { Container } from 'next/app';
import Page from '../components/Page';
import { ApolloProvider } from 'react-apollo';
import withData from '../withData';

class MyApp extends App {
  render() {
    // apollo comes from the higher-order component 'withData'
    const { Component, apollo } = this.props;

    return (
      <Container>
        {/* To expose Aopllo client to React you wrap the app around ApolloProvider */}
        <ApolloProvider client={apollo}>
          <Page>
            <Component {...pageProps} />
          </Page>
        </ApolloProvider>
      </Container>
    );
  }
}

export default withData(MyApp);
```
In the Next.js 7.0 version page numbers are not automatically exposed to components, thus we have to set this up. GetInitialProps is a Next.js lifecycle that runs first, returns the pageprops, making it available to this.props.
```js
import App, { Container } from 'next/app';
import Page from '../components/Page';
import { ApolloProvider } from 'react-apollo';
import withData from '../withData';

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
    // apollo comes from the higher-order component 'withData'
    const { Component, apollo, pageProps } = this.props;

    return (
      <Container>
        {/* To expose appllo client to react you wrap the app around ApolloProvider */}
        <ApolloProvider client={apollo}>
          <Page>
            <Component {...pageProps} />
          </Page>
        </ApolloProvider>
      </Container>
    );
  }
}

export default withData(MyApp);
```

