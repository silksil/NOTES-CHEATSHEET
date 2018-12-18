Mutations are used to change our data; remove, change, create etc. All the mutations are related to the `Mutations` object, not the `RootQuery` or the types related to it.
<img src="../images/mutations.png?" width="600">

In order to do this, we add a `mutation` const in our schema:
```js
const mutation = new GraphQLObjectType({
  name: 'Mutation',
  fields: {
    // Describe the operation it's gonna execute
    addUser: {
      // Refers to the type of data we eventually will resolve (not the type we give)
      type: UserType,
      args: {
        // Here we define with GraphQLNonNull that name is required. If not, throws error.
        firstName: { type: new GraphQLNonNull(GraphQLString) },
        age: { type: GraphQLInt },
        companyId: { type: GraphQLString }
      },
      resolve(parentValue, { firstName, age, companyId }) {
      	// it's asynchronous, therefore include `return`
        return axios.post('http://localhost:3000/users', { firstName, age, companyId })
          .then(res => res.data);
      }
    },
    deleteUser: {
      type: UserType,
      args: {
        id: { type: new GraphQLNonNull(GraphQLString) }
      },
      resolve(parentValue, { id }) {
        return axios.delete(`http://localhost:3000/users/${id}`)
          .then(res => res.data);
      }
    }
  }
});
```
Add a to the GraphQl graph:
```js
module.exports = new GraphQLSchema({
  mutation,
  query: RootQuery
});
```
Now, in the query tool we should be able to see mutations in the docs. To query a mutation we have to define it is a mutation. Additionally, we have to define which properties we expect to get back (basically the properties included in the db):
```
mutation {
	addUser(firstName: "Stephen", age: 26){
  	id
    firstName
    age
  }
}
```
