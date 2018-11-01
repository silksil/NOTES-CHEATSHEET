The key to using an back-end server with a project is to use a proxy. This tells the Web-pack development server to proxy our API requests to our API server, given that our server is running on localhost:5000.
Now we don't have to use a fully qualified URL like `http://localhost:5000/api/hello` to call our API, even though our Client app runs on a different port (3000). This is because of the proxy line you can in the package.json file:
```json
"proxy": {
    "/auth/google": {
      "target": "http://localhost:5000"
    },
    "/api/*": {
      "target": "http://localhost:5000"
    }
  },
```
