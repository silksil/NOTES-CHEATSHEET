 ## Router
 
 ### Installation & Import
 `npm install --save react-router-dom`
 ```
 ```jsx 
import { BrowserRouter, Route, Switch} from 'react-router-dom';
 ```
- **BrowserRouter**: type of route > used for servers that handle dynamic requests.
- **Route**: used if you want to render content based on the locations pathname. Location is defined in the first prop (path), component second prop
- **Switch**: <Route>s can be created anywhere inside of the router, but often it makes sense to render them in the same place. You can use the<Switch> component to group <Route>s. The <Switch> will iterate over its children elements (the routes) and only render the first one that matches the current pathname, so you want to put your most specific route at the top.
```jsx
<BrowserRouter>
 <div>
  <Switch>
   <Route path="/posts/new" component={PostsNew}/>
   <Route path="/posts/:id" component={PostsShow}/>
   <Route path="/" component={PostsIndex}/>
  </Switch>
 </div>
</BrowserRouter>
```
 
## Current route as piece of state
To load a specific state out of an array we could create two states:
```js
let posts = [ {title: 'Hello', id: 4, content: 'Hi'}, {title: 'Bye', id: 12, content: 'Bye'}];
let activePost = {title: 'Hello', id: 4, content: 'Hi', tags: 'greetings'};
```
Nonetheless, if you include a specific state in the url -- like `/posts/5` -- this would could create a duplicate piece of state. Thus, activePost state does not have to be created. To make it easier to find a particuler item you could consider writing it as an object with keys and assigned objects. This would prevent writing a for loop or a findArray helper, and instead allow you do something like `state.posts[postId]`. 
```javascript
let posts = {4: {title: 'Hello', id: 4, content: 'Hi'}, 12: {title: 'Bye', id: 12, content: 'Bye'}};
```
- See this link to find how how to transform the array to an object as stated in the code above: https://github.com/silksil/best-practices-cheatsheets/blob/master/javascript/array's.md
- See this link on how to use lodash to map over over an object with objects: https://github.com/silksil/best-practices-cheatsheets/blob/master/client/lodash.md

#### Loading page based on parameter
Let's say we are on a page with all the posts and for every post we created a link based on the id of the post. We could then load the page  of a specific post based on the paramater in the link and extract all the info of a specific post from the state with all posts. Nonetheless, this state maybe not be available if a user would directly navigate to a specific post, as the page with with all posts is not first loading the state. Thus, the specific component should be responsible for fetching the data.
```jsx
componentDidMount() {
 const { id } = this.props.match.params; // get the wild-cart tokens of the url
 this.props.fetchPost(id);
}
```
`match.params` is directly provided by React Router: `match` is the top-level property,  `params` list  all wild-card tokens (could be multiple).

## Navigation
Within the render method, you don't use anchor tags because you do discrete navigation. Instead you want to show a new set of components. In order to do this you use the Link library.
```jsx
import { Link } from 'react-router-dom';
```

```jsx
<Link className="btn" to="/posts/new">
 Add a post
</Link>
```

The route functionality passes `history` to handle with navigation ( `<Link>` is only for within the DOM, instead this is programmetric navigation). By default, when you click on a `<Link>` from React Router, it will use history.push to navigate. More specifically, the push method allows you to go to a new location. For example:
 
```jsx
onDeleteClick(id) {
  this.props.deletePost(id, () => {
   this.props.history.push('/')
  });
}
```

## Navigation with active links based on route
With NavLink you dynamically change the classname based on the route that is active. 

```
import React from 'react';
import { NavLink } from'react-router-dom';

export default function Nav () {
 return (
  <ul className='nav'>
   <li>
    <NavLink exact activeClassName='active' to='/'>Home</NavLink>
   </li>
   <li>
    <NavLink activeClassName='active' to='/battle'>Battle</NavLink>
   </li>
  </ul>
 );
}
```

Sources:
- https://medium.com/@pshrmn/a-simple-react-router-v4-tutorial-7f23ff27adf
- https://medium.com/@pshrmn/a-little-bit-of-history-f245306f48dd
