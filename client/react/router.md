 ```jsx 
 import { BrowserRouter, Route} from 'react-router-dom';
 ```
   
```jsx
<BrowserRouter>
 <div>
  <Route path="/hello" component={Hello}/>
  <Route path="/goodbye" component={Goodbye}/>
 </div>
</BrowserRouter>
```

```jsx
```



## Navigation
You don't ues anchor tags because you do discrete navigation. Instead you want to show a new set of components. In order to do this you use the Link library.
```jsx
import { Link } from 'react-router-dom';
```

```jsx
<Link className="btn btn-primary" to="/posts/new">
 Add a post
</Link>
```
