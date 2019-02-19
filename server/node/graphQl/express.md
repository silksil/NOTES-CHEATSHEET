## Express / Server

GraphQl works as a mediator between Express and a database to return the correct data.</br>

<img src="images/graphql-2.png?" width="600">

In a small app the architecture is likely look a bit like this: </br>

<img src="images/small-app.png?" width="400">

Nonetheless, in a large app you are likely to have multiple servers that store data. GraphQl can serve as a proxy, making multiple http requests to other servers in the database, and sending data it in one .json file back.
<img src="images/large-app.png?" width="600">

#### Install, require schema and initialize:

```js
const express = require("express");
const expressGraphQL = require("express-graphql");

const schema = require("./schema/schema");

app.use(
  "/graphql",
  expressGraphQL({
    schema,
    graphiql: true
  })
);
```

#### Schema

You have to specifically inform GraphQl how data is arranged. This is done in the schema file. First, let's require GraphQl:

```js
const graphql = require("graphql");
```

Then, include the types we need for this schema:

```js
const { GraphQLObjectType, GraphQLString, GraphQLInt, GraphQLSchema } = graphql;
```

We continue with defining a model, or what is referred to as a type in GraphQl. The GraphQLObjectType always has two required properties:

- `name` is a capital string that describes the type we are defining, mostly equal to the const.
- `Field` describes the different properties. Every property should include the type.

```js
const UserType = new GraphQLObjectType({
  name: "User",
  fields: {
    id: { type: GraphQLString },
    firstName: { type: GraphQLString },
    age: { type: GraphQLInt }
  }
});
```

Next, we have to state to GraphQL where to jump in a certain part of graph. This is defined as the `RootQuery`. You can see it as an entry point into our data:

```js
const RootQuery = new GraphQLObjectType({
  name: "RootQueryType",
  fields: {
    // 1.) If you are looking for a user
    user: {
      // 3. I give back to you a user
      type: UserType,

      /*
       * `args` stands for arguments that you have to provide to give something back,
       * 2. If you give me a id
       */
      args: { id: { type: GraphQLString } },

      /*
       * resolve:  purpose is to actually get the data from the DB
       * parentValue: an argument that is hardly being used
       * args: stands for arguments, basically include the argument defined above in it
       */
      resolve(parentValue, args) {
        return axios
          .get(`http://localhost:3000/users/${args.id}`)
          .then(resp => resp.data);
      }
    }
  }
});
```

Lastly, we have to return a GraphQl instance:

```js
// Takes in a root query and returns a GraphQl instance
module.exports = new GraphQLSchema({
  query: RootQuery
});
```
