This is a device that intercepts the requests to the application servers and balances the traffic between them. The LB my use various strategies to do so, Round Robin, Smart Sharing (i.e. communication with the application servers about their traffic and routing accordingly). In this kind where the load balancer has the metrics to gauge traffic, it may shut down servers when the traffic is minimal and create new instances when the traffic is higher. But this solution is not cheap in terms of overhead, strategies that find a sweet spot between the two are employed.

Algorithms for LBs : :
Round Robin
Least Connections
Least Response Time
Least Bandwidth
Hashing 

The Node Balancers can be of two types, based on which network layer they function:
* Network Layer
* Application Layer