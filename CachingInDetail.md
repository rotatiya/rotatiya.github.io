#### What is Cache?
Please be informed that data is always present in memory. Memory could be anything and anywhere.
Caching is the way to improve performance by reducing/removing network call and I/O operations between client and server using memory cache. The cache helps to improve performance by storing the data on client itself and would not make a call to the server to get that require data.
So, finally the web caching is a core design feature of the HTTP/HTTPS protocol meant to minimize network traffic to improve user experience and application performance.

#### What is cache memory
A faster and smaller segment of memory whose access time is as close as registers perhaps it uses RAM to store the data are known as Cache memory. As this is very fast its expensive too if not utilize correctly. 

#### How to measure Cache performance
Performance of cache is measured by the number of cache hits to the number of searches. This parameter of measuring performance is known as the Hit Ratio.
Hit ratio=(Number of cache hits)/(Number of searches).
Performance of cache is directly proportional to Hit Ratio.

#### Benefits
- Increase availability by decreasing network traffic
- Improve responsiveness and user experience

#### Types of cache
There are broadly two types of cache
- 1. Local cache also known as browser cache
- 2. Proxy cache also known as shared cache or cache server.

#### How to control cache
I am categorising cache for web meaning HTTP which can be controled using headers only. So HTTP has its own header to control the behaviour of cache.
cache 



#### What is Network call and I/O?
When ever client, it could be browser or API test tool makes a call to FQDN, Fully Qualified Domain or IP Address outside current host it needs to set up socket/connection to request or post information. THe network connection is completely depends on type of scheme(protocol) used to make a call. When such connection establishes through tcp handshak followed by TLS/SSL  handshake if ischeme is HTTPS it needs to reansfer some data and form channel for data to transferred and that entire process cost I/O operation.


