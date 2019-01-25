# Learning Goals?
- What is an API?
- What is HTTP?

# What is an API?
When two systems (websites, desktops, smartphones) link up through an API, we say they are "integrated." In an integration, you have two sides, each with a special name. One side we have already talked about: the server. This is the side that actually provides the API. It helps to remember that the API is simply another program running on the server. It may be part of the same program that handles web traffic, or it can be a completely separate one. In either case, it is sitting, waiting for others to ask it for data. The other side is the "client." This is a separate program that knows what data is available through the API and can manipulate it, typically at the request of a user. A great example is a smartphone app that syncs with a website. When you push the refresh button in your app, it talks to a server via an API and fetches the newest info. The same principle applies to websites that are integrated. When one site pulls in data from the other, the site providing the data is acting as the server, and the site fetching the data is the client.

 ##### Source: https://zapier.com/learn/apis/chapter-1-introduction-to-apis/

# What is HTTP?
As you probably know very well by now, the internet is made up of a bunch of interconnected computers called servers. When you are surfing the web and navigating between web pages, what you are really doing is telling your browser to request information from any of these servers. It kinda looks as follows: your browser sends a request, waits awkwardly for the server to respond to the request, and (once the server responds) processes the request. All of this communication is made possible because of something known as the HTTP protocol.

The HTTP protocol provides a common language that allows your browser and a bunch of other things to communicate with all the servers that make up the internet. The requests your browser makes on your behalf using the HTTP protocol are known as HTTP requests, and these requests go well beyond simply loading a new page as you are navigating.

##### Source: https://www.kirupa.com/html5/making_http_requests_js.htm