### What is it?
GraphQl provides a way to define complete description of data in schema and allows client to ask for what they need. It simply returns the response in JSON.

### Advantages compared to REST API
- Versioning: To avoid multiple versioning of your rest API.
- Ask for what you need: Client has a provision to ask only those fields which they needs. There would be no handling on server side specific to the platform.
- Get many API’s response in single request: Client has to call one query to get data from multiple rest API’s.

### Graph: notes & Edges
<img src="images/graphql-1.png?" width="600">
A graph is a data structure that includes notes (which are the rectangles)  and the edges (which are the relations). The way data is stored is not different; SQL or NoSQL can still be used. Once have put the data into graph, we query it through GraphQL.

You can think of the schema or data, as a bunch of functions that return references to other objects/pieces of data in our graph. Every edge can then be perceived as function that resolves data.
<img src="images/resolve.png?" width="600">


### Express / Server
GraphQl works as a mediator between Express and a database to return the correct data.
<img src="images/graphql-2.png?" width="600">

In a small app the architecture is likely look a bit like this:
<img src="images/small-app.png?" width="600">

Nonetheless, in a large app you are likely to have multiple servers that store data. GraphQl can serve as a proxy, making multiple http requests to other servers in the database, and sending data it in one .json file back.
<img src="images/large-app.png?" width="600">

#### Install, require schema and initialize:
```js
const express = require('express');
const expressGraphQL = require('express-graphql');

const schema = require('./schema/schema');

app.use('/graphql', expressGraphQL({
  schema,
  graphiql: true
}));
```
#### Schema
You have to specifically inform graphQl how data is arranged. This is done in the schema file:
```js
const graphql = require('graphql');
const _ = require('lodash');

const {
  GraphQLObjectType,
  GraphQLString,
  GraphQLInt,
  GraphQLSchema
} = graphql;

const users = [
  { id: '23', firstName: 'Bill', age: 20 },
  { id: '47', firstName: 'Samantha', age: 21 }
];

const UserType = new GraphQLObjectType({

  /*
   * GraphQLObjectType always has two required properties
   * Name is a capital string that describes the type we are defining, mostly equal to const
   * Field describes the different properties. Every property should include the type
  */
  name: 'User',
  fields: {
    id: { type: GraphQLString },
    firstName: { type: GraphQLString },
    age: { type: GraphQLInt }
  }
});

/*
 * A root query allows us to jump is in a certain part of the Graph
 * You can see it as an entry point into our data
*/
const RootQuery = new GraphQLObjectType({
  name: 'RootQueryType',
  fields: {

    // 1.) If you are looking for a user
    user: {

      // 3. I give back to you a user
      type: UserType,

      /*
       * args stands for arguments that you have to provide to give something back,
       * 2. If you give me a id
      */
      args: { id: { type: GraphQLString } },

      /*
       * resolve:  purpose is to actually get the data from the DB
       * parentValue: an argument that is hardly being used
       * args: stands for arguments, basically include the argument defined above in it
      */
      resolve(parentValue, args) {

        // Go through all users and find the first user with a certain id
        return _.find(users, { id: args.id });
      }
    }
  }
});

// Takes in a root query and returns a GraphQl instance
module.exports = new GraphQLSchema({
  query: RootQuery
});

```

#### The GraphQL tool
It's a tool provided in the browser by the GraphQL Express library. We can add a query in the tool and see the result on the right. On the top-right there is a documentation explorer to understand the data structure and data fetching possibilities.