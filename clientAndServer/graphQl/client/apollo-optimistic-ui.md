Optimistic UI is a pattern that you can use to simulate the results of a mutation and update the UI even before receiving a response from the server. Once the response is received from the server, the optimistic result is thrown away and replaced with the actual result.

<img src="../images/apollo-optimistic-update.png?">

For example, let's say we like a certain action. It may take some time before that the like is included in the db, and fetched again, causing a delay in the amount of likes on the screen after you have clicked. So, let's say we have the following mutation that handles the like request:
```jsx
const mutation = gql`
  mutation LikeLyric($id: ID) {
    likeLyric(id: $id) {
      id
      likes
    }
  }
`;
```

Then, we initialise the mutation and include the optimistic response:
```jsx
onLike(id, likes) {
  this.props.mutate({
    // send variables to mutation
    variables: { id },
    // here the optimistic response starts
    optimisticResponse: {
      // you have the explicitly say it's a mutation
      __typename: 'Mutation',
      // here you have to define the response you expect to see from the back-end server (check response log)
      likeLyric: {
        id,
        __typename: 'LyricType',
        // the current amount of looks in this case is already included in the react component
        // the optimistic response is the `+1`
        likes: likes + 1
      }
    }
  });
}
```

Once again: if the response is received from the server, the optimistic result is thrown away and replaced with the actual result. Thus, if in the meantime other people like the action, the amount of like shown to the user can be incremented with more than 1. The disadvantage, is that if many people have liked in the meantime, it can show behaviour that the user doesn't expect. 
