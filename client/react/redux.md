# General
Whereas React displays the views, Redux collects all the data of the application. With Redux we centralize all the data in a single objec - we refer to this as 'state'.

# Reducer
Is a function that reduces a piece of the application state. Because an application can have many different pieces of state, it can have many different reducers. 

```
{
  books: [{ title: 'Harry Potter'}, {title: 'Javascript' }], // >> Books reducer
  activeBook: { title: 'Javascript' }
}
```
