### Create 1 CSS and 1 Javascript File
When it’s time to move your app to production, having multiple JavaScript or CSS files isn’t ideal. When a user visits your site, each of your files will require an additional HTTP request, making your site slower to load. So to remedy this, you can create a “build” of our app, which merges all your CSS files into one file, and does the same with your JavaScript. This way, you minimize the number and size of files the user gets. To create this “build”, you use a “build tool”. Hence the use of `npm run build`.

After you run npm run build your build directory will be:
```
build/
  static/
    css/
      main.css
    js/
      main.js
```

### Make Express and React work in Sync
There are gonna be routes that are being handled by React Router and there are requests that are being handled by Express. Thus, we have to instruct the Expresss server that if there is a request that it doesn't know about, that it it's probably gonna be a request done by React Router.

In order to do this, Express server hands back the index.html file (so in the cases the route is not available in Express). Inside the index.html there is a script tag that loads up the javascript bundle `/client/build/static/js/main.js`. The browser now has access to the React part of the application. React Router then detects what kind of component you want, and will load only that one in.

In order to do this we go to our router area in our root file in express and make sure we define this piece of configuration only for production. Make sure to place the piece of code, below your other routes. 
```js 
if (process.env.NODE_ENV === 'production') { 

}
```
Then, we state that the app should look in to `client/build` for a file that we are looking for, if we can't find it in the other routes. This refers to our production assets, like main.js and main.css. 
```js 
if (process.env.NODE_ENV === 'production') { 
  app.use(express.static('client/build'));
}
```
If we have nothing in our express routes, and none in our `client/build` we give back the index.html file:
```js
if (process.env.NODE_ENV === 'production') {
  app.use(express.static('client/build'));

  const path = require('path');
  app.get('*', (req, res) => {
    res.sendFile(path.resolve(__dirname, 'client', 'build', 'index.html'))
  });
}
```

###
Push it to a Continious Integration server. A CI server is a server that is a server that is able to run tests.
.
