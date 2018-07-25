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
