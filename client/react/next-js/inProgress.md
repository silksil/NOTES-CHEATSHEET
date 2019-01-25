To show a progress bar we have to import two things:
```js
import Router from 'next/router';
import NProgress from 'nprogress';
```

Through the Router of Next.js we can access several events ( when it starts & ends) that allow us to create a progress bar. We link these event handlers to NProgress.
```js
Router.onRouteChangeStart = () => {
  NProgress.start();
};

Router.onRouteChangeComplete = () => {
  NProgress.done();
};

Router.onRouteChangeError = () => {
  NProgress.done();
};

```
