# Learning Goals

- What is caching?
- In which 4 environments can you cache and how do the differ?
- How does client-side caching working in frameworks like Apollo and Redux?

Caching is the process of storing data in a cache. A cache is a temporary storage area.

A Website could store data for the purpose of speed up future requests on three different layers and environments: 1) Client, 2) Network, 3) Server, and 4) Application.

# 1. Client

Different pages of a Website commonly share the same assets, and thus can be cached for minutes on the Client Browser. HTTP headers have the responsibility to define if a response could be cached and for how long. The Browser is going to request it again only if the cache expires or if the user force refreshes the page:

# 2. Network

Wikipedia defines a Content Delivery Network (CDN) as a globally distributed network of proxy servers. CDNs are about caching — shared caching.

The Cache-control: public HTTP header directive allows different parts of the Network to cache a response. It is common to find assets with Cache-Control: public, max-age=31536000 meaning that it last a year anywhere.

You might know that there are others header cache directives. There is also a powerful header to handle authenticated and other kinds of dynamic responses.

# 3. Server

The first approach to faster responses and save resources is setting up a cache server between the application and the client.

There is also a directive called proxy_cache_lock that allows the proxy server to delegate only the first of similar client requests at a time for the application. If it is set on, the clients are going to receive the response when the first request returns.It is a simple but powerful mechanism that avoids chaos on the application side when a content expires, and many clients are requesting for it.

Last but not least, the proxy could improve the fault tolerance of the application. There are flags for the proxy_cache_use_stale directive to deliver expired content when the application returns error statuses or when the communication between the server proxy and the application is not working as expected.

# 4. Application

Application Caching reduces the time of specific operations. Complex computations, data requests to other services or common data shared across request are some examples.

### 4.1 Memoization

A global code memoization is going to last in-memory during all the application execution cycle.

### 4.2 Smart In-Memory Caching

There are many libraries that provide this pattern. But it is important to mention that the application memory is a finite resource. The node-cache module, for example, doesn't manage the amount of memory consumed. It could be a problem if your application massively caches data consuming all the available memory.

### 4.3 Cache Storages — Shared Caching

Handle a growing amount of users and requests is an important subject of Web development. One of the ways to scale an application is through adding more application instances (scale horizontally). And as you might imagine, the simple in-memory cache can't be shared between instances.

The Twelve-factor App, a methodology for building Software as a Service (SaaS), points that an application should never assume that anything cached in memory or on disk will be available on a future request or job — with many processes of each type running, chances are high that a future request will be served by a different process.

A Key-value Storage like Memcached or Redis could be used to share cache data between application instances.

It is difficult to completely eliminate cache updating race conditions issues with multiple application instances. A solution for that is to update the cache data outside the application flow and just consume cached data on the application. On a micro services architecture, it is also possible to protect the communication between application and service with a nginx proxy server as explained above.
