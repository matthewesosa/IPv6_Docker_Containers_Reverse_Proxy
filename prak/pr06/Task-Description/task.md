# Internship Sheet 6 - Loadbalancer Scenario
A notice:
In the following tasks, you create services on your server serv-ws22that can be reached from the outside via a precisely defined IPv6 address from your IPv6 subnet. Please keep these services running on your server at all times .

You can use the taskchecker [(LEA: https://lea.hochschule-bonn-rhein-sieg.de/goto.php?target=webr_1214931&client_id=db_040811)](http://[2001:638:408:200:ff00:cafe:0:beaf]:8080/) to check that your services are actually running .

The ongoing services are part of the pre- examination service (H-BRS) or the ULP (TH-Cologne).

 

# Task 1
Now let's build a load balancer scenario with an nginx based load balancer and three different miniwhoami services miniwhoami-s1, iniwhoami-s2 and miniwhoami-s3. The load balancer should be accessible from the outside via the IP address and the URL http://loadbalancer.yourDomain.xy or the URL http://lb.yourDomain.xy .The miniwhoami services should run in the local IPv6 network loc_ipv6 2001:638:408:200:ff??:cafe::9999/96

a. Create a compose file with the three miniwhoami services miniwhoami-s1, miniwhoami-s2 and miniwhoami-s3 . Test the miniwhoami services on the local IPv6 network loc_ipv6 using curl.

b. For an introduction to load balancing with NGINX, read the NGINX documentation chapters: HTTP Load Balancing .
- What HTTP load balancing methods are there? How do these work?
- What is session persistence? What types of session persistence are there?

c. Add an Nginx load balancer to your compose file.  For the load balancer we use an  official Nginx  image .  Configure the load balancer  nginx.conf for weighted round robin load balancing. 

Test your load balancer!

My solution:

http://[2001:638:408:200:ff00:cafe:0:9999]
http://loadbalancer.servmgmt.de
 

exercise 2
To prepare for next week's lectures, read from Lukša Marko:  Kubernetes in Action , Hanser, 2018:

Chapter 3 - Pods: running containers in Kubernetes.

Chapter 4 - Replication and other controllers: deploying managed pods.

Chapter 5 - Services: enabling clients to discover and talk to pods.