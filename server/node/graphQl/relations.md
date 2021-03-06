
## Nested Query
We gonna relate a company to a user by adding a company id property to a particular user.

Let's start defining the dummy data we get back from the db:
```json
{
  "users": [
    { "id": "23", "firstName": "Bill", "age": 20, "companyId": "1" },
    { "id": "43", "firstName": "Alex", "age": 40, "companyId": "2" },
    { "id": "40", "firstName": "Joost", "age": 30, "companyId": "2" }
  ],
  "companies": [
    { "id": "1", "name": "Apple", "description": "iphone" },
    { "id": "2", "name": "Google", "description": "search" }
  ]
}
```
We treat associations between types, as it would be another field in the schema. So, let's say we have user that can work add a certain company. We would then create the Company type:
```js
const CompanyType = const UserType = new GraphQLObjectType({
  name: "Company",
  fields: {
    id: { type: GraphQLString },
    name: { type: GraphQLString },
    description: { type: GraphQLString }
  }
});
```
Then, we would add the company as a field to the user to set the association:  

```js
const UserType = new GraphQLObjectType({
  name: 'User',
  fields: {
    id: { type: GraphQLString },
    firstName: { type: GraphQLString },
    age: { type: GraphQLInt },
    company: {
      type: CompanyType
    }
  }
});
```
Next, we want to teach GraphQL how to walk from a user over to a company.
<img src="../images/model-type.png?" width="600">

Remember that GraphQl works together with a db; on the left you see data that you get from the db, on the right what we define in GraphQl's schema file. If the property names are the same, GraphQl automatically gets the data from the db. Nonetheless, notice that we use `company` as a field,  but what we get from the db is a `companyId`. If this is the case, we have to teach GraphQl to populate the correct using the `resolve` function. In this case we do this by adding a resolve function to the `UserType`:
```js
const UserType = new GraphQLObjectType({
  name: 'User',
  fields: {
    id: { type: GraphQLString },
    firstName: { type: GraphQLString },
    age: { type: GraphQLInt },
    company: {
      type: CompanyType,
      resolve(parentValue, args) {
        return axios.get(`http://localhost:3000/companies/${parentValue.companyId}`)
          .then(res => res.data);
      }
    }
  }
});
```
We can than test in the GraphQl tool whether we can retrieve the correct data :
```
{
  user(id: "23") {
    id,
    company {
      id,
      name,
      description
    }
  }
}
```
## Nested Query List
To get a list of of a data type associated with a certain type, we need specify this in the schema. For example, we need all users associated with a company. In order to do this, we have to define the type as `GraphQLList`.
```js
const CompanyType = new GraphQLObjectType({
  name: "Company",
  fields: {
    id: { type: GraphQLString },
    name: { type: GraphQLString },
    description: { type: GraphQLString }
    users: {
     type: new GraphQLList(UserType),
     resolve(parentValue, args) {
       // parentValue refers to the current company you are currently using
       return axios.get(`http://localhost:3000/companies/${parentValue.id}/users`)
         .then(res => res.data)
    }
  }
});
```
This potentially could lead to an error if you haven't defined the `UserType` yet, aka circular references. As a workaround, you can add an arrow function to the `field` property. This assures that the entire file will be executed, and only after that it will execute the arrow function and resolve:
```jsx
const CompanyType = new GraphQLObjectType({
  name: "Company",
  fields: () => ({
    id: { type: GraphQLString },
    name: { type: GraphQLString },
    description: { type: GraphQLString }
    users: {
     type: new GraphQLList(UserType),
     resolve(parentValue, args) {
       return axios.get(`http://localhost:3000/companies/${parentValue.id}/users`)
         .then(res => res.data)
    }
  })
});
```
We can now test it in the GraphQl tool:
```
{
  company(id: "2") {
    id,
		name,
    users {
      firstName
    }
  }
}
```
