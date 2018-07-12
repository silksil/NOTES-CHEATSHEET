Lodash makes JavaScript easier by taking the hassle out of working with arrays,
numbers, objects, strings, etc. It is the most dependent on npm package.

``` js
npm install --save lodash
import _ from 'lodash';
```

### Mapping over an object 
Similar as array.map, but then for an object.
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

### Delete a object from an motherObject based on its key
```js
_.omit(object, key) // it looks to the object > if it is has the inserted key > delete it
```

