## No need to include the loading state

All cache writes and reads are synchronous, so you don’t have to worry about loading state.
With Apollo you can also handle your local state. In order to do this you need `apollo-link-state`; allows you to store your local data inside the Apollo cache alongside your remote data.

# Setting Up

Apollo-link-state is already included in Apollo Boost, so you don’t have to install it. It’s configurable on the `clientState` property on the Apollo Boost constructor:

```js
import ApolloClient from "apollo-boost";
import { defaults, resolvers } from "./resolvers";

const client = new ApolloClient({
  uri: `https://nx9zvp49q7.lp.gql.zone/graphql`,
  clientState: {
    defaults,
    resolvers,
    typeDefs
  }
});
```

The three options you can pass to clientState are:

- **defaults: Object**
  The initial data you want to write to the Apollo cache when the client is initialized
- **resolvers: Object**
  A map of functions that your GraphQL queries and mutations call in order to read and write to the cache
- **typeDefs: string | Array**
  A string representing your client-side schema written in Schema Definition Language. This schema is not used for validation (yet!), but is used for introspection in Apollo DevTools

None of these options are required. If you don’t specify anything, you will still be able to use the @client directive to query the cache.

# Update the local cache

There are two ways to perform mutations in apollo-link-state. The first way is directly writing to the cache by calling cache.writeData within an ApolloConsumer or Query component. Direct writes are great for one-off mutations that don’t depend on the data that’s currently in the cache, such as writing a single value. The second way is creating a Mutation component with a GraphQL mutation that calls a client-side resolver. We recommend using resolvers if your mutation depends on existing values in the cache, such as adding an item to a list or toggling a boolean.
