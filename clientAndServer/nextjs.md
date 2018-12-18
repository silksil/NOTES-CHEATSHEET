Next.js is a React framework for building web applications. It does:
- All of the tooling: webpack, code splitting etc.
- It does server-side rendering: if you care about instant loading, pre-loading, seo etc., server-side rendering is important. It provides GetInitialProps: allows you to fetch data on the server. You wait on the data to be resolved before the page is send to the browser
- it does the routing; there is no routing, there are pages. Every singel page you want to visit those gonna be .js files. In those files you can create a React component. 
