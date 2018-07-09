Lodash makes JavaScript easier by taking the hassle out of working with arrays,
numbers, objects, strings, etc. It is the most dependent on npm package.

``` 
npm install --save lodash
import _ from 'lodash';
```

Mapping over an object > same as array.map, but then for an object
```jsx
renderPosts() {
 return  _.map(this.props.posts, post => {
  return (
   <li className="list-group-item" key={post.id}>
    <Link to={`/posts/${post.id}`}>
     {post.title}
    </Link>
   </li>
  );
 });
}
```
