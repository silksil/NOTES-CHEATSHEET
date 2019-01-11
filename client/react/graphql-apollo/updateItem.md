The following examples explains how to *update* an item through a form using React, Graphql and Apollo. 

First, let's include all necessary dependencies:
```js
React, { Component } from 'react';
import { Mutation, Query } from 'react-apollo';
import gql from 'graphql-tag';
// this optional if you want to move to the next page after it is being updated
import Router from 'next/router';
```
Then, we create a gql-query to get the information that we already have regarding an item. 
```js
const SINGLE_ITEM_QUERY = gql`
  query SINGLE_ITEM_QUERY($id: ID!) {
    item(where: { id: $id }) {
      id
      title
      description
      price
      }
  } 
`; 
```
Next we create a UpdateItem class with an empty state (as we only pass the variables that are being changed). In the in the class, we include a Query component in which we will pass the id of the item we want to update. We will display the data of the form in the defaulValue property. 
```js
class UpdateItem extends Component {
  state = {};

  render() {
    return (
      <Query
      query={SINGLE_ITEM_QUERY}
      variables={{
        id: this.props.id,
      }}
        >
        {({data, loading }) => {
          if (loading) return <p>Loading...</p>;
          if (!data.item) return <p>No Item Found for ID {this.props.id}</p>;
          return (
            <Form>
              <label htmlFor="title">
                Title
                <input
                  type="text"
                  id="title"
                  name="title"
                  placeholder="Title"
                  required
                  defaultValue={data.item.title}
                />
              </label>

              <label htmlFor="price">
                Price
                <input
                  type="number"
                  id="price"
                  name="price"
                  placeholder="Price"
                  required
                  defaultValue={data.item.price}
                />
              </label>

              <label htmlFor="description">
                Description
                <textarea
                  id="description"
                  name="description"
                  placeholder="Enter A Description"
                  required
                  defaultValue={data.item.description}
                />
              </label>
              <button type="submit">Save Changes</button>
            </Form>
          )
        }}
      </Query>
    );
  }
}

export default UpdateItem;
```
If changes are being made, we have to made sure we include the properties that have been changed in the state. In order to do this, we create a handleChange function:
```js
// We make it a arrow function, so we can access `this` 
// and refer to `setState` without having to include the 
// `constructor`, `super` having to to bind the function to `this`
handleChange = e => {
  const { name, type, value } = e.target;
  // transform from string to proper number
  const val = type === 'number' ? parseFloat(value) : value;
  this.setState({ [name]: val });
};
```
We make sure that the `handleChange` function is invoked every time form field is being changed:
```js
<Form>
    <label htmlFor="title">
      Title
      <input
        type="text"
        id="title"
        name="title"
        placeholder="Title"
        required
        defaultValue={data.item.title}
        onChange={this.handleChange}
      />
    </label>

    <label htmlFor="price">
      Price
      <input
        type="number"
        id="price"
        name="price"
        placeholder="Price"
        required
        defaultValue={data.item.price}
        onChange={this.handleChange}
      />
    </label>

    <label htmlFor="description">
      Description
      <textarea
        id="description"
        name="description"
        placeholder="Enter A Description"
        required
        defaultValue={data.item.description}
        onChange={this.handleChange}
      />
    </label>
    <button type="submit">Save Changes</button>
</Form>
```
Next, we add the ggl-mutation:

```js
const UPDATE_ITEM_MUTATION = gql`
  mutation UPDATE_ITEM_MUTATION($id: ID!, $title: String, $description: String, $price: Int) {
    updateItem(id: $id, title: $title, description: $description, price: $price) {
      id
      title
      description
      price
    }
  }
`;
```
We include the Mutation component and extract the mutation and payload. We include an error handler, and disable the form if it is loading. Also, if it is loading the button text will change. When we submit the form we will invoke the updateItem *function* which we will pass the event and the updateItem *mutation*. 
```js
return (
  <Mutation mutation={UPDATE_ITEM_MUTATION} variables={this.state}>
    {/* Mutations gives us the mutation function and payload */}
    {(updateItem, { loading, error }) => (
      <Form onSubmit={e => this.updateItem(e, updateItem)}>
        <Error error={error} />
        <fieldset disabled={loading} aria-busy={loading}>
          <label htmlFor="title">
            Title
            <input
              type="text"
              id="title"
              name="title"
              placeholder="Title"
              required
              defaultValue={data.item.title}
              onChange={this.handleChange}
            />
          </label>

          <label htmlFor="price">
            Price
            <input
              type="number"
              id="price"
              name="price"
              placeholder="Price"
              required
              defaultValue={data.item.price}
              onChange={this.handleChange}
            />
          </label>

          <label htmlFor="description">
            Description
            <textarea
              id="description"
              name="description"
              placeholder="Enter A Description"
              required
              defaultValue={data.item.description}
              onChange={this.handleChange}
            />
          </label>
          <button type="submit">Sav{loading ? 'ing' : 'e'} Changes</button>
        </fieldset>
      </Form>
    )}
  </Mutation>
);
```
Next, we include the updateItem function, which is being passed the event and the updateItem mutation. We pass the id that received through the props, and pass the properties of the state that have been changed. 
```js
  updateItem = async (e, updateItemMutation) => {
    e.preventDefault();
    console.log('Updating Item!!');
    console.log(this.state);
    const res = await updateItemMutation({
      variables: {
        id: this.props.id,
        ...this.state,
      },
    });
      console.log('Updated!!');
  };
```