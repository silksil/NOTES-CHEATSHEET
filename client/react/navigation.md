 ## Router
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

The route functionality passes `history` to handle with navigation (used outside the render method). By default, when you click on a <Link> from React Router, it will use history.push to navigate. More specifically, the push method allows you to go to a new location. For example:
 
```jsx
onDeleteClick(id) {
  this.props.deletePost(id, () => {
   this.props.history.push('/')
  });
}
```
## Current route as piece of state
To load a specific state out of an array we could create two states:
```js
let posts = [ {title: 'Hello', id: 4, content: 'Hi'}, {title: 'Bye', id: 12, content: 'Bye}];
let activePost = {title: 'Hello', id: 4, content: 'Hi', tags: 'greetings'};
```
Nonetheless, if you include a specific state in the url -- like `/posts/5` -- this would could create a duplicate piece state. Thus, activePost state does not have to be created. To make it easier to find a particulur post you could consider writing it as an object with keys and assigned objects. This would prevent writing a for loop or a findArray helper, and instead allow you do something like: state.posts[postId]. 
```javascript
let posts = {4: {title: 'Hello', id: 4, content: 'Hi'}, 12: {title: 'Bye', id: 12, content: 'Bye'}};
```





Sources:
- https://medium.com/@pshrmn/a-simple-react-router-v4-tutorial-7f23ff27adf
- https://medium.com/@pshrmn/a-little-bit-of-history-f245306f48dd
