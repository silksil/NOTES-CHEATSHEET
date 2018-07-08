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

The route functionality passes `history` to handle with navigation (used outside the render method). By default, when you click on a <Link> from React Router, it will use history.push to navigate. More specifically, the push method allows you to go to a new location. Example:
 
```jsx
onDeleteClick(id) {
  this.props.deletePost(id, () => {
   this.props.history.push('/')
  });
}
```

Sources:
- https://medium.com/@pshrmn/a-simple-react-router-v4-tutorial-7f23ff27adf
- https://medium.com/@pshrmn/a-little-bit-of-history-f245306f48dd
