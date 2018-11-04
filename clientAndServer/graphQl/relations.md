We gonna relate a company to a user by adding a company id property to a particular user.

Let's start with creating dummy data:
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
