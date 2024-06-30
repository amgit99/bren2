## Lecture 1:
<span style="color:#e1db3d">Extensibility</span> : Is your code robust to changes. Are you providing enough abstraction so that if a component changes, its relatively easy to adapt to the newer component. A fence from your business logic, so if that changes its not a big problem. BUSINESS COMES FIRST this may sometimes lead to shitty (non-extensible) code, incurring technical debt, due to harsh deadlines.

If low level code is not abstracted enough then the time to shift to a new technology will be very slow (e.g. switching from AWS → Azure)![[Pasted image 20231211181224.png]]
### Important components:
* <span style="color:#e1db3d">Databases</span>: Need to store data somewhere
* <span style="color:#e1db3d">Caching</span>: Same as that in COA.
* <span style="color:#e1db3d">Scaling</span>: Horizontal or Vertical
* <span style="color:#e1db3d">Delegation</span>: Most underrated, like Messaging Queues
* <span style="color:#e1db3d">Concurrency</span>: Multiple users, how can one handle, locks and critical section handling.
* <span style="color:#e1db3d">Communication</span>: Over Http ? GRPC ? raw TCP, UPD
### Databases :
If the blog application has very few writes and multiple reads, we may skip the Relational DB, and only use something like<span style="color:#e1db3d"> Elastic Search</span> along with a lot of emphasis on Caching. <span style="color:#e1db3d">Redis</span> is a way to do caching, (<span style="color:#a90a0a">sharding</span> of data). The final level is CDN level caching, this exploits geographic closeness. (<span style="color:#a90a0a">e-tags cache</span> ?)

## Lecture 2: Intro ….

<span style="color:#e1db3d">Vertical Scaling<span style="color:#e1db3d"></span></span> is when we beef up the hardware on the single server to handle more traffic.
<span style="color:#e1db3d">Horizontal Scaling</span> is where the number of individual systems increase in number to handle the traffic.

VS- : Has a ceiling on how much power can be added, single point of failure, not efficient
VS+ : Easier to manage, Low licencing fee, Easy to implement.
HS- : Larger footprint, licencing fee, App becomes complicated, Networking and Partitioning HS+ : Infinite Scaling, scaling acc to traffic, fault tolerant, cheaper.

For HS we always need a [[Load Balancer]], its the single point of contact that will forward the requests to the servers. The load balancers can target the request to the server that best serves it. LBs read the URLs and accordingly forwards. LBs are itself a set of machines.

The first issue is scaling the database, as the number of server instances increase the number of requests to the database will increase and that will cause a failure.

A common feature upgrade strategy : If we have 100 instances of a server running with a database the way to update the software is to create ~100 new instances with the updated code (image) and then delete the previous instances one by one, for a short duration, this increases the load on the database leading to a failure.

Vertical Scaling → same problems → go for Horizontal Scaling → now a thicc boi problem emerges…..

One strategy is, having more read only servers for the DB rather than the update/write servers. <span style="color:#e1db3d">Sharding</span> is the strategy where the DB is divided data wise.

Vertical Scaling → Read only servers (acc to demand) → sharding.
<span style="color:#e1db3d">Delegation</span>: Anything that does not need to be done in real-time, should be delegated to a different service. SQS is simple-queue-service, the queues hold these tasks that are delegated and can be done at a later time, the messages have an expiration time too.
<span style="color:#e1db3d">Message Queues</span> have two types: The ones with Homogeneous consumers(SQS, Redis, Rabbit MQ) and Heterogeneous consumers(Kafka, Kinesis). The second category has different types of consumers and they perform different tasks, all of them need to process every messages in the queue.![[Pasted image 20231211181352.png]]
Go lang threads are really great !

> The JavaScript Runtime: Chrome has the v8 engine which is a JIT compiler. It has the stack that has the running functions as usual but when things are done async the functions are added to the event queue, they are then run when the stack is empty. This allows for the hoax of parallel programming.

<span style="color:#e1db3d">Polling</span>: For services like Cric-buzz where new data is created and the users demand it very frequently, polling refers to the strategy that the server may use wrt to what data it sends back. <span style="color:#e1db3d">Short Polling</span> is when a request is given the updates, or nothing if there is nothing to send.
<span style="color:#e1db3d">Long Polling</span> on the other hand means keeping the connection open until some data is available to send, i.e. never send an empty response.

<span style="color:#e1db3d">Web Sockets</span>: This is a resource intensive case where the TCP connections are kept open and very frequent updates are needed. Eg a message application, trading apps.

## Lecture 3: Relational DB

RDBs show ACID properties, Atomicity of transactions, Consistency of Data(always legal state), Durability means recoverability from system failure, Isolation means multiple transactions run as if they are the only ones on the system(like a process).