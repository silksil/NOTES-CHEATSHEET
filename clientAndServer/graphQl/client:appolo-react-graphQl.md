### Introduction
The purpose of GraphQl client is a bonding layer between React and the GraphQl graph. Some major GraphQl clients are Lokka, Apollo Client and Relay.
<img src="images/graphql-client.png?" width="600">

<img src="images/apollo-store-provider.png?" width="600">
Apollo consists out of the Apollo store and provider. The store is a client-side piece of tech that directly communicate with the server and saves the data in the store. It can be used across different technologies, not just React. Nonetheless, the Apollo provider glues React with the Apollo store by taking data from the store in injecting it in the client-side. Most set-up is related to the Apollo provider.

### Set-up
To set-up Apollo we have to install dependencies::
 ```
 - apollo-client
 - react-apollo
 ```
```jsx

Next, we set-up the config of Apollo:
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import ApolloClient from 'apollo-client';
import { ApolloProvider } from 'react-apollo';

// Create a new instance of ApolloClient and pass it to ApolloProvider
// We pass an empty config object => it then assumes the back-end is set-up on the route /graphql
const client = new ApolloClient({});

const Root = () => {
  return (
    <ApolloProvider client={client}>
      <div>Lyrical</div>
    </ApolloProvider>
  );
};

ReactDOM.render(
  <Root />,
  document.querySelector('#root')
);
```

### How to write GraphQl queries in React
<img src="images/graphql-apollo-react.png?"
