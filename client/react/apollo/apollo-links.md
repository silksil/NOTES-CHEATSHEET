Apollo Client has a pluggable network interface layer, which can let you configure how queries are sent over HTTP, or replace the whole network part with something completely custom, like a websocket transport, mocked server data, or anything else you can imagine.

It allows you to used as middleware and afterware:

- Middleware: are used to inspect and modify every request made over the link, for example, adding authentication tokens to every query.
- Afterware: ‘Afterware’ is very similar to a middleware, except that an afterware runs after a request has been made, that is when a response is going to get processed. It’s perfect for responding to the situation where a user becomes logged out during their session.
