### How to chain: with or without return?
If we use fat arrow functions, we don't have to include `return` in order to get the value in the next `.then`. This only works if we don't include open and closing brackets, and it is suggested to apply this to situations where the function can be written on one line. Nonetheless, if we create function with open and closing brackets, which is commonly used for function that include multiple lines,  we do have to include return. Example:
```js
joe.save()
// fat arrow + 1 line = no open/closing brackets = exclude return
.then(() => User.findOne({ name: 'Joe' }))
// open/closing brackets = include return
.then((user) => {
  user.posts.push({ title: 'New Post' });
  // save Joe
  return user.save();
})

```
