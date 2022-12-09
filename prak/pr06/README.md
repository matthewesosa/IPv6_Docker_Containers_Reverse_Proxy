# Internship Sheet 6 - Loadbalancer Scenario
## Task 1
### Now we want to build a load balancer scenario with an nginx based load balancer and three different miniwhoami services miniwhoami-s1, miniwhoami-s2 and miniwhoami-s3. The load balancer should be reachable from outside via the IP address 2001:638:408:200:ff??:cafe::9999/96 and the URL http://loadbalancer.yourDomain.xy or the URL http://lb.yourDomain.xy, the miniwhoami services should run in the local IPv6 network loc_ipv6.

### (1b.i) What HTTP load balancing methods are there? How do these work?
[Reference source: kemptechnologies.com](https://kemptechnologies.com/load-balancer/load-balancing-algorithms-techniques)
#
* ### Round Robin : 
application servers receive client requests in a straightforward rotation. The first client request is sent to the first application server in the list, the second client request is sent to the second application server, the third client request is delivered to the third application server, the fourth client request is sent to the first application server, and so on.
#

* ### Weighted Round Robin: 
this approach is comparable to round-robin, but it has the added advantage of allocating incoming client requests among the server farm's  in line with each server's individual capacity as defined by the administrator using weighted values. As a result, the application server with the higher weight will receive more traffic.
#

* ### Least Connection:  
client requests are sent to the application server that has the fewest active connections at the time .   This method works well with incoming requests that have variable connection times and a group of servers that have similar processing capabilities and resource availability.
#

* ### Weighted Least Connection:
the weighted least connection load balancing algorithm expands on the least connection load balancing technique to take into consideration various application server properties. Based on the relative processing power and resource availability of each server in the farm, the administrator gives a weight to each application server.  Decision is taken by the algorithm using both the assigned server weights and active connections criteria (e.g., if there are two servers with the lowest number of connections, the server with the highest weight is chosen).
#

* ### Source IP Hash: 
this is used for session persistence. The source and destination IP addresses of the client request are used to create a special hash key for assigning the client to a certain server. The client request is sent to the same server it previously used because the key can be created if the connection is terminated. 
#

* ### URL Hash: 
comparable to source IP hashing, but uses the client request URL as the basis for the hash. By doing this, it is guaranteed that client queries for a specific URL will always be routed to the same back-end server.
#

* ### Fixed Weighting: 
the administrator assigns a weight to each application server using fixed weighting based on their traffic handling capacity . All traffic will be sent to the application server with the highest weight. All traffic will be routed to the application server with the next greatest weight if the application server with the highest weight fails. This approach is suitable for workloads where a single server can handle all anticipated incoming requests and there are one or more redundant servers prepared to take over in the event that the active server fails.
#

* ### Resource Based (Adaptive): 
bases its judgments on back-end server status indications collected by LoadMaster. An individual application (an "agent") running on each server determines  the status indicator. , LoadMaster periodically reques For this status informationts from each server, after which it adjusts the dynamic weight of the real server. In this way, the load balancing technique effectively does a thorough "health check" on the actual server.
#

* ### Resource Based (SDN Adaptive): 
this uses information from Layers 2, 3, 4, and 7 along with input from an SDN controller to provide more effective traffic distribution decisions. As a result, information on the servers' condition, the state of the applications , the infrastructure's health, and the network's level of congestion can all be taken into account when making load balancing decisions. This approach is suitable for deployments using an SDN controller.
#

* ###  Weighted Response Time:  
here  a server's weight is determined by its response time. The next request is sent to the application server that is responding the quickest. This technique works well in situations where the speed of the application response is crucial.
#

### (1b.ii) What is session persistence? What types of session persistence are there?
* Session persistence is a process where requests from a single user are always forwarded by a loadbalancer to the same server on which they first originated. In cookie based session persistence, it is made possible with the help of generated session ID passed along as cookies, and it is known as "sticky sessions" in some load balancing products and services.
#  
* ### Types of cookie based session persistence
* ### Duration-based session persistence:
a cookie that specifies a certain duration for session stickiness is sent by your load balancer. The load balancer determines whether this cookie is present each time a client request comes in. The session is no longer sticky after the allotted time has passed and the cookie expires.

* ### Application-controlled session persistence:
The length of the session's stickiness is determined by a cookie created by the application. Although the load balancer continues to issue its own session cookie on top of it, it now adheres to the application cookie's lifetime. By guaranteeing that users are never forwarded to a server after their local session cookie has already expired, this improves the efficiency of sticky sessions. However, because it necessitates more integration between the load balancer and the application, its implementation is more difficult.

#

My solution:

http://[2001:638:408:200:ff6c:cafe:0:9999]

http://loadbalancer.migbin2s-servemgmt.site