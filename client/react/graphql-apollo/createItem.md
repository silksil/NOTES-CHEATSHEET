The following examples explains how to create an item through a form using React, Graphql and Apollo. 

First, let's create a form, a state set-up and a state handler.
```js
import React, { Component } from 'react';
import gql from 'graphql-tag';
import Form from './styles/Form';

class CreateItem extends Component {
  state = {
    title: '',
    description: '',
    price: 0,
  };

    // We make it a arrow function, so we can access `this` 
    // and refer to `setState` without having to include the 
    // `constructor`, `super` having to to bind the function to `this`
    handleChange = e => {
      const { name, type, value } = e.target;
      // transform from string to proper number
      const val = type === 'number' ? parseFloat(value) : value;
      this.setState({ [name]: val });
    };

  render() {
    return (
      <Form
      onSubmit={ e => {
        // Stop the form from submitting
        e.preventDefault();
        console.log(this.state)
      }}
      >
        <fieldset>
          <label htmlFor="title">
            Title
            <input
              type="text"
              id="title"
              name="title"
              placeholder="Title"
              required
              value={this.state.title}
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
              value={this.state.price}
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
              value={this.state.description}
              onChange={this.handleChange}
            />
          </label>
          <button type="submit">Submit</button>
        </fieldset>
      </Form>
    );
  }
}

export default CreateItem;
```

Next we set-up the mutation that will send the data to our db. We export the function so we can use it for testing and to share it among other components.
```js
import { Mutation } from 'react-apollo';

const CREATE_ITEM_MUTATION = gql`
// When you call the mutation we gonna pass these arguments
// These are available through variables in the createItem func
  mutation CREATE_ITEM_MUTATION(
    $title: String!
    $description: String!
    $price: Int!
  ) {
    createItem(
      title: $title
      description: $description
      price: $price
    ) {
      // State that we expect an id back
      id
    }
  }

// put at end of the page
export { CREATE_ITEM_MUTATION };
`;
```
Next we want to expose the mutation function (and also the error and loading property) by wrapping our entire form into a mutation component. In this mutation component, we can include the mutation function (in this case CREATE_ITEM_MUTATION) and the variables that the mutation function will receive.
```js
<Mutation mutation={CREATE_ITEM_MUTATION} variables={this.state}>
{/* Mutations gives us the mutation function and payload */}
{(mutationFunction, payload ) => (
  <Form> </Form>
)}
</Mutation>
```
We will restructure the code above by renaming the mutation function and destructuring the properties we gonna use from the payload. We only use 'loading' and 'error', but other properties that exists on the payload are: 'called'(boolean on whether it is run) &'data'(the data that you get back). 
```js
<Mutation mutation={CREATE_ITEM_MUTATION} variables={this.state}>
{/* Mutations component gives us the mutation function and payload */}
{(createItem, { loading, error }) => (
  <Form> </Form>
)}
</Mutation>
```
Now we have exposed`createItem`, `loading` and `error` to the form we can include it in the form. 

We start with an error handler. If there is an error, we will also still show the form, as it may happen that something is wrong with a form field, which then can be solved by the user to then submit the form again. 

For this example, we include a component that displays errors and can be used throughout an application:

```js
import styled from 'styled-components';
import React from 'react';

import PropTypes from 'prop-types';

const ErrorStyles = styled.div`
  padding: 2rem;
  background: white;
  margin: 2rem 0;
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-left: 5px solid red;
  p {
    margin: 0;
    font-weight: 100;
  }
  strong {
    margin-right: 1rem;
  }
`;

const DisplayError = ({ error }) => {
  if (!error || !error.message) return null;
  if (error.networkError && error.networkError.result && error.networkError.result.errors.length) {
    return error.networkError.result.errors.map((error, i) => (
      <ErrorStyles key={i}>
        <p data-test="graphql-error">
          <strong>Shoot!</strong>
          {error.message.replace('GraphQL error: ', '')}
        </p>
      </ErrorStyles>
    ));
  }
  return (
    <ErrorStyles>
      <p data-test="graphql-error">
        <strong>Shoot!</strong>
        {error.message.replace('GraphQL error: ', '')}
      </p>
    </ErrorStyles>
  );
};

DisplayError.defaultProps = {
  error: {},
};

DisplayError.propTypes = {
  error: PropTypes.object,
};

export default DisplayError;
```
We will include this error-handler in the form. It will only show this component of there is an actual error. 

```js
import Error from './ErrorMessage';

<Mutation>
  <Form> 
    <Error error={error} />
    <input>
    </input>
  </Form>
</Mutation>

```
If the user submits a form, it can take a while before we receive a response from the server. To prevent that the user in the mean time changes the form and submits the form again we can add a `fieldset` attribute that disables the form from being filled in if the `disabled` property is set to `true`. We also include an `aria-budy` attribute which we can use to apply css when it is loading. 
```js
<Mutation>
  <Form> 
    <Error error={error} />
    <fieldset disabled={loading} aria-busy={loading}>
      <input>
      </input>
    </fieldset>
  </Form>
</Mutation>
```
Next we call the createItem mutation if the form is submitted. If that is done, we change the route of the page (which is determined by the response of the mutation). 

```js
import Router from 'next/router';

<Mutation>
  <Form
  data-test="form"
  onSubmit={async e => {
    // Stop the form from submitting
    e.preventDefault();
    // call the mutation
    const res = await createItem();
    // change them to the single item page
    Router.push({
      pathname: '/item',
      query: { id: res.data.createItem.id },
    });
  }}
  >
    <Error error={error} />
    <fieldset disabled={loading} aria-busy={loading}>
      <input>
      </input>
    </fieldset>
  </Form>
</Mutation>
```
At the end, putting everything together, the code of the form will look like this:
```js
import React, { Component } from 'react';
import { Mutation } from 'react-apollo';
import gql from 'graphql-tag';
import Router from 'next/router';
import Form from './styles/Form';
import formatMoney from '../lib/formatMoney';
import Error from './ErrorMessage';

const CREATE_ITEM_MUTATION = gql`
  mutation CREATE_ITEM_MUTATION(
    $title: String!
    $description: String!
    $price: Int!
  ) {
    createItem(
      title: $title
      description: $description
      price: $price
    ) {
      id
    }
  }
`;

class CreateItem extends Component {
  state = {
    title: '',
    description: '',
    price: 0,
  };

    // We make it a arrow function, so we can access `this` 
    // and refer to `setState` without having to include the 
    // `constructor`, `super` having to to bind the function to `this`
    handleChange = e => {
      const { name, type, value } = e.target;
      // transform from string to proper number
      const val = type === 'number' ? parseFloat(value) : value;
      this.setState({ [name]: val });
    };

  render() {
    return (
      <Mutation mutation={CREATE_ITEM_MUTATION} variables={this.state}>
      {/* Mutations gives us the mutation function and payload */}
      {(createItem, { loading, error }) => (
        <Form
        data-test="form"
        onSubmit={async e => {
          // Stop the form from submitting
          e.preventDefault();
          // call the mutation
          const res = await createItem();
          // change them to the single item page

          Router.push({
            pathname: '/item',
            query: { id: res.data.createItem.id },
          });
        }}
        >
          <Error error={error} />
          <fieldset>
            <label htmlFor="title">
              Title
              <input
                type="text"
                id="title"
                name="title"
                placeholder="Title"
                required
                value={this.state.title}
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
                value={this.state.price}
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
                value={this.state.description}
                onChange={this.handleChange}
              />
            </label>
            <button type="submit">Submit</button>
          </fieldset>
        </Form>
      )}
      </Mutation>
    );
  }
}

export default CreateItem;
export { CREATE_ITEM_MUTATION };
```
