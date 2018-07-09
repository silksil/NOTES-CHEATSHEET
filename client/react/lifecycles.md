Lifecycle method is automatically called by React.

## componentDidMount () {}
Will be automatically called after the component is loaded in the dom. Ideal for fetching data or a one-time loading procedure. You might think why do you want to fetch data after the component is loaded on the screen > because it doesn't matter > fetching data is a syncronous and takes some time. React doesn't have a functionally that waits. Even componentWillMount will end-up a component being rendered one-time before we succesfully render our data. 

## componentWillMount () {}
Called before the component is load in the dom
