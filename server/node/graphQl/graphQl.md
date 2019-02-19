# What is it?
GraphQl provides a way to define complete description of data in schema and allows client to ask for what they need and returns the response in JSON. It used to replace REST API or to sit in front of a REST API.

GraphQL has 2 main operations:
- **Query** - to read (query for) data
- **Mutation** - to write (mutate, create/update/delete) data
- **Subscription** - to listen for (subscribe to) data (events)

GraphQL itself doesn't fetch, filter, sort etc. It allows you to focalize what you want, which will be send to the servers, and then the server will implement _resolvers_ which answers the question: how and where will I get the data from? The answer on this questions, is where you will end up using a db and change data (filter, sort etc.).

# Graphql vs. API

| GraphQL                            | REST | API |
| ---------------------------------- | ---- | --- |
| No Over/Under Fetching Problem     | +    | -   |
| Easy Fetching of Related Resources | +    | -   |
| Great Client Performance           | +    | -   |
| Easy Caching                       | -    | +   |
| Scoped Includes                    | -    | -   |
| Easy Deprecations                  | +    | -   |


#### No Over/Under fetching problems, easy fetching, great performance
Client has a provision to ask only those fields which they needs. There would be no handling on server side specific to the platform.

Also, you can get many API’s response in single request: Client has to call one query to get data from multiple rest API’s.

Laslty, includes type-checking when receiving data, making the code more robust.

#### Easy Caching
Caching (pron: kashing) is the business of storing responses to requests in a fast memory and serving the cached responses to identical or similar future requests until the response needs to change and the stored response cache is invalidated.

Two Scenarios: 1) a cache-hit serves a response from the cache, and 2) a cache-miss serves a response from the application server and updates the cache.

Determining which cached data to return when caching requests, is based on the request data, e.g.:

- The URL or path (e.g. /posts/1)
- The request method (e.g. GET)
- Any descriptive headers (e.g. Content-Type: application/json)

Also, some requests may not be cached at all (e.g POST requests, or requests with Authorization or Set-Cookie headers). REST API responses are easy to cache based on a few simple rules like above. We will see that GraphQL API responses are a lot harder to cache.

#### Scoped Includes:
 Scoped includes is how we describe situations like these:
```
"Say I have a team object, that has a name property, a city property and a players property, where the players property is a an array of possibly many players. This is represented in an SQL database with a teams table and a players table, where each player has a name and a team_id."
```
🤔 How do you think the backend developers implemented this feature in their APIs?
🤓 "Include vs Endpoint Indecision" Deciding on including related data in an API response or using a separate endpoint to fetch the extra data is such a common challenge for API developers, that there is a name for it: "Include vs Endpoint" Indecision.

#### Easy Deprications:
 This means API responses are not supposed to change in shape: an endpoint returns a fixed set of fields, each with values of a certain data type. Since other peoples' code depends on their consistency, APIs are usually versioned. Sometimes, though, versioning is not enough and the API needs to change or evolve to a new situation. In that case, fields may be marked as deprecated and API users will be notified of a sunset window, which can be anything from 24 hours up to a year or more, after which the API will irreversibly evolve to the new version. REST APIs have no standardized way to mark fields as deprecated in their response, but GraphQL APIs do!

# Graph Data Relation
<img src="images/graphql-1.png?" width="600">
 Graph data structures hold information about how data relates (the connections) to other data in the graph and about the domain knowledge (names of the connections, e.g. movies have directors). Relations in graphs may be recursive (movie => director => directedMovies). The graphql data includes notes (which are the rectangles)  and the edges (which are the relations). The way data is stored is not different; SQL or NoSQL can still be used. Once have put the data into graph, we query it through GraphQL. You can think of the schema or data, as a bunch of functions that return references to other objects/pieces of data in our graph. Every edge can then be perceived as function that resolves data.
<img src="images/resolve.png?" width="600">

 # Queries
GraphQL APIs allow the client to query for specific data, and even for specific fields within that data. The responses of GraphQL APIs are typed, predictable, yet customisable. A GraphQL query exists of 2 parts:
- The query name (academies)
- The fields to return (id, start_date, end_date, full). 

A GraphQl API has built in validation of the query:If you don't tell it which fields to return, or you're asking for fields that do not exist in the defined type, the query will error.

An example of an query:
```gql
query {
  academies {
    id
    start_date
    end_date
    full
  }
}
```

#### With Arguments
```gql
query {
  academies(where: {
    type: CODE
    future: true
  }) {
    id
    start_date
    end_date
    full
  }
}
```
We know which arguments we can use, since their types are defined in the schema as well. Complex or custom arguments may be defined as input types.  

```gql
# Input types are used to type data in arguments
input AcademyWhereInput {
  type: AcademyType
  future: Boolean
}

# Enums define a finite list of possible values
enum AcademyType {
  CODE
  DESIGN
  DATA_SCIENCE
}

Query {
  academies(where: AcademyWhereInput): [Academy!]
}
```
We may also use built in data types as arguments:
```gql
Query {
  course(pathToContent: String!): Course
}
```

#### Variables
To make carefully written queries reusable on the client side, GraphQL came up with a variables system that exists of 2 parts:

- The Operation Name
- Variables

Here is a query:
```gql
query {
  academies(where: {
    type: CODE
    future: true
  }) {
    id
    start_date
    end_date
    full
  }
}
```
In the above example, we only used the query keyword to describe the type of operation. However, we may name our query by giving it a name as well: an operation name:

```gql
query FutureAcademies {
  academies(where: {
    type: CODE
    future: true
  }) {
    id
    start_date
    end_date
    full
  }
}
```
Now we named our query FutureAcademies. And now, we can make this query reusable for different academy types (CODE, DESIGN or DATA_SCIENCE) by introducing a variable and assigning it a type:
```gql
query FutureAcademies($type: AcademyType!) {
  academies(where: {
    type: $type
    future: true
  }) {
    id
    start_date
    end_date
    full
  }
}
```
And finally, we can assign a value to our variable, using this JSON syntax:
```JSON
{
  "type": "CODE"
}
```