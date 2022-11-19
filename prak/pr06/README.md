# Internship Sheet 6 - Loadbalancer Scenario
## Task 1
## Now we want to build a load balancer scenario with an nginx based load balancer and three different miniwhoami services miniwhoami-s1, miniwhoami-s2 and miniwhoami-s3. The load balancer should be reachable from outside via the IP address 2001:638:408:200:ff??:cafe::9999/96 and the URL http://loadbalancer.yourDomain.xy or the URL http://lb.yourDomain.xy, the miniwhoami services should run in the local IPv6 network loc_ipv6.

## (1b.i) What HTTP load balancing methods are there? How do these work?
* Round-robbin: requests are sent to each upstream server one at a time such that the load is evenly distributed with no preference given to any of the server.

* Least Connection: the load balancer considers the number of connections an upsrteam server is already processing and the server withe fewest connections would be preferred for every new request.
  
* IP_hash: this is used for session persistence. It considers the IP address of the client making the request then it choses an upsream server to handle the request; if the client IP address stys the same, then the same upstream server will handle any future request made by the client 

* Fixed Weight: In order to indicate the relative traffic-handling capacity of each server, a weight is assigned to each server using fixed weighting, a load balancing algorithm. Hence, more traffic will go to the application server with the highest weight.


## (1b.ii) What is session persistence? What types of session persistence are there?

